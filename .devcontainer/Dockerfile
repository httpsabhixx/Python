# https://github.com/microsoft/vscode-dev-containers/blob/main/containers/python-3/README.md
ARG VARIANT=3.12-bookworm
FROM mcr.microsoft.com/vscode/devcontainers/python:${VARIANT}
COPY requirements.txt /tmp/pip-tmp/
RUN python3 -m pip install --upgrade pip \
  && python3 -m pip install --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
  && pipx install pre-commit ruff \
  && pre-commit install
import pygame
import sys
import random

# Initialize pygame
pygame.init()

# Constants
GRID_SIZE = 20
GRID_WIDTH = 30
GRID_HEIGHT = 20
SCREEN_WIDTH = GRID_WIDTH * GRID_SIZE
SCREEN_HEIGHT = GRID_HEIGHT * GRID_SIZE
FPS = 10

# Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Directions
UP = (0, -1)
DOWN = (0, 1)
LEFT = (-1, 0)
RIGHT = (1, 0)

# Snake class
class Snake:
    def __init__(self):
        self.length = 1
        self.positions = [((SCREEN_WIDTH // 2), (SCREEN_HEIGHT // 2))]
        self.direction = random.choice([UP, DOWN, LEFT, RIGHT])
        self.color = GREEN
        self.score = 0

    def get_head_position(self):
        return self.positions[0]

    def turn(self, point):
        if self.length > 1 and (point[0] * -1, point[1] * -1) == self.direction:
            return
        else:
            self.direction = point

    def move(self):
        cur = self.get_head_position()
        x, y = self.direction
        new = (((cur[0] + (x * GRID_SIZE)) % SCREEN_WIDTH), (cur[1] + (y * GRID_SIZE)) % SCREEN_HEIGHT)
        if len(self.positions) > 2 and new in self.positions[2:]:
            self.reset()
        else:
            self.positions.insert(0, new)
            if len(self.positions) > self.length:
                self.positions.pop()

    def reset(self):
        self.length = 1
        self.positions = [((SCREEN_WIDTH // 2), (SCREEN_HEIGHT // 2))]
        self.direction = random.choice([UP, DOWN, LEFT, RIGHT])
        self.score = 0

    def draw(self, surface):
        for p in self.positions:
            r = pygame.Rect((p[0], p[1]), (GRID_SIZE, GRID_SIZE))
            pygame.draw.rect(surface, self.color, r)
            pygame.draw.rect(surface, WHITE, r, 1)

    def handle_keys(self):
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    self.turn(UP)
                elif event.key == pygame.K_DOWN:
                    self.turn(DOWN)
                elif event.key == pygame.K_LEFT:
                    self.turn(LEFT)
                elif event.key == pygame.K_RIGHT:
                    self.turn(RIGHT)

    def collision_with_apple(self, apple_position):
        return self.get_head_position() == apple_position

    def check_collision(self):
        if any((self.get_head_position() == p) for p in self.positions[1:]):
            self.reset()

    def get_score(self):
        return self.score

    def increase_score(self):
        self.score += 1
        self.length += 1

# Apple class
class Apple:
    def __init__(self):
        self.position = (0, 0)
        self.color = RED
        self.randomize_position()

    def randomize_position(self):
        self.position = (random.randint(0, GRID_WIDTH - 1) * GRID_SIZE, random.randint(0, GRID_HEIGHT - 1) * GRID_SIZE)

    def draw(self, surface):
        r = pygame.Rect((self.position[0], self.position[1]), (GRID_SIZE, GRID_SIZE))
        pygame.draw.rect(surface, self.color, r)
        pygame.draw.rect(surface, WHITE, r, 1)

# Main function
def main():
    clock = pygame.time.Clock()
    screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT), 0, 32)
    pygame.display.set_caption('Snake Game')

    snake = Snake()
    apple = Apple()

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        snake.handle_keys()
        snake.move()
        snake.check_collision()

        if snake.collision_with_apple(apple.position):
            snake.increase_score()
            apple.randomize_position()

        screen.fill(BLACK)
        snake.draw(screen)
        apple.draw(screen)
        pygame.display.update()
        clock.tick(FPS)

if __name__ == '__main__':
    main()
