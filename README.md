# prototype-pokegame
import math
import random
import sys
from dataclasses import dataclass, field

import pygame

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

