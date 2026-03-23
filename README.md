# prototype-pokegame
import math
import random
import sys
from dataclasses import dataclass, field

import pygame

# =========================================================
# Monster Catching RPG (single-file, pygame)
# Inspired by creature-collection games, but original/simple
# =========================================================

pygame.init()
pygame.display.set_caption("Pocket Monsters Lite")

SCREEN_W, SCREEN_H = 960, 640
SCREEN = pygame.display.set_mode((SCREEN_W, SCREEN_H))
CLOCK = pygame.time.Clock()

FONT = pygame.font.SysFont("arial", 22)
SMALL_FONT = pygame.font.SysFont("arial", 18)
BIG_FONT = pygame.font.SysFont("arial", 32, bold=True)

TILE = 32
MAP_W = 25
MAP_H = 18

# ---------------------------
# Colors
# ---------------------------

WHITE = (245, 245, 245)
BLACK = (20, 20, 20)
GRAY = (90, 90, 90)
LIGHT_GRAY = (170, 170, 170)
GREEN = (80, 180, 90)
DARK_GREEN = (40, 120, 60)
BLUE = (80, 130, 220)
DARK_BLUE = (50, 80, 180)
RED = (210, 80, 80)
DARK_RED = (160, 50, 50)
YELLOW = (240, 210, 90)
BROWN = (120, 85, 60)
SAND = (216, 195, 140)
PURPLE = (150, 95, 210)

# ---------------------------
# Game tuning
# ---------------------------
GRASS_ENCOUNTER_CHANCE = 0.10
MAX_TEAM_SIZE = 6

# ---------------------------
# Monster definitions
# ---------------------------

SPECIES = [
    {
        "name": "Sproutle",
        "color": (90, 210, 120),
        "base_hp": 34,
        "base_attack": 9,
        "base_defense": 7,
        "moves": ["Leaf Tap", "Tackle"],
          },
    {
        "name": "Embercub",
        "color": (240, 120, 70),
        "base_hp": 30,
        "base_attack": 11,
        "base_defense": 6,
        "moves": ["Spark Paw", "Tackle"],
    },
    {
        "name": "Bubbloon",
        "color": (80, 180, 240),
        "base_hp": 36,
        "base_attack": 8,
        "base_defense": 8,
        "moves": ["Bubble Pop", "Tackle"],
    },
    {
        "name": "Pebblit",
        "color": (140, 140, 150),
        "base_hp": 40,
        "base_attack": 8,
        "base_defense": 10,
        "moves": ["Rock Bonk", "Tackle"],
    },
    {
        "name": "Zappip",
        "color": (240, 225, 90),
        "base_hp": 28,
        "base_attack": 12,
        "base_defense": 6,
        "moves": ["Static Zip", "Tackle"],
    },
]

MOVE_DATA = {
    "Tackle": {"power": 8, "accuracy": 0.95},
    "Leaf Tap": {"power": 10, "accuracy": 0.95},
    "Spark Paw": {"power": 11, "accuracy": 0.90},
    "Bubble Pop": {"power": 10, "accuracy": 0.95},
    "Rock Bonk": {"power": 12, "accuracy": 0.85},
    "Static Zip": {"power": 13, "accuracy": 0.80},
}

# ---------------------------
# Helper classes
# ---------------------------

@dataclass
class Monster:
    name: str
    species_name: str
    level: int
    color: tuple
    max_hp: int
    hp: int
    attack: int
    defense: int
    moves: list
    exp: int = 0

     def is_fainted(self):
        return self.hp <= 0

     def heal_full(self):
        self.hp = self.max_hp

     def gain_exp(self, amount):
        self.exp += amount
        leveled = False
        while self.exp >= self.exp_to_next():
            self.exp -= self.exp_to_next()
            self.level += 1
            self.max_hp += random.randint(2, 4)
            self.attack += random.randint(1, 2)
            self.defense += random.randint(1, 2)
            self.hp = self.max_hp
            leveled = True
        return leveled

    def exp_to_next(self):
        return 12 + self.level * 6

        
