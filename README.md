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

