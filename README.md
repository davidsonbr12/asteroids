# Asteroids

A "faithful" (it is not great, first time with pygame) recreation of the classic arcade game, built in Python with Pygame.

## Gameplay

Pilot a ship through an asteroid field. Shoot asteroids to split them into smaller fragments and straight-up destroy them completely to clear the field. One hit from an asteroid ends the game.

**Controls**

| Key | Action |
|-----|--------|
| `W` | Thrust forward |
| `S` | Thrust backward |
| `A` / `D` | Rotate left / right |
| `Space` | Fire |

## Architecture

The game uses a component-based sprite system via Pygame's `Group` abstraction. All entities inherit from `CircleShape`, which provides position, velocity, and collision detection via radius overlap. The game loop runs at 60 FPS with delta-time physics so movement is framerate-independent.

| Module | Responsibility |
|--------|---------------|
| `main.py` | Game loop, collision resolution, group wiring |
| `player.py` | Input handling, shooting with cooldown, triangle rendering |
| `asteroid.py` | Drift movement, recursive split logic |
| `asteroidfield.py` | Spawns asteroids from screen edges on a timer |
| `shot.py` | Projectile movement |
| `circleshape.py` | Base class — position, velocity, radius-based collision |
| `constants.py` | All tunable game parameters |
| `logger.py` | Structured event and state logging to `.jsonl` files |

Asteroid splitting is recursive: a hit asteroid spawns two children at ±20–50° from the parent's velocity vector, each with radius reduced by `ASTEROID_MIN_RADIUS` and speed scaled by 1.2×. Asteroids below `ASTEROID_MIN_RADIUS` are destroyed outright.

## Setup

Requires Python 3.13+ and [uv](https://github.com/astral-sh/uv).

```bash
uv sync
uv run main.py
```
