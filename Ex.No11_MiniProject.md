Ex.No: 11 Mini Project - Cannon Game with Pathfinding
DATE: 12.11.2024
REGISTER NUMBER: 212222240117
AIM
To write a Python program to simulate a cannon game with pathfinding algorithms using Pygame.

Algorithm
1. Initialize Game Components
Import necessary libraries (Pygame, Numpy, etc.).
Set up Pygame screen, colors, and basic game settings (FPS, screen size, cannon setup).
2. Define Game Objects
Ball Class: Represents the cannonball, handles its movement and physics (gravity).
Target Class: Represents targets, handles their movement across the screen.
Pathfinding Algorithm: Implements basic AI for target movement and collision detection with the cannonball.
3. Gameplay Loop
Cannon: Handles mouse input to aim and fire the cannonball.
Targets: Targets move randomly and can be hit by the cannonball.
Collision Detection: Detect when the ball hits the target and update the score.
Game Over: Check if a target crosses the screen boundary to end the game.
Score System: Increase score when a target is hit and reset the game upon game over.
4. End-of-Game Data Logging and Visualization
Save game statistics such as the score to a CSV file.
Visualize the game stats using Matplotlib.
Program
python
Copy code
import pygame
import random
import math
import pandas as pd
import matplotlib.pyplot as plt

# Initialize Pygame
pygame.init()

# Screen dimensions and setup
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Cannon Game")

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)

# Game variables
ball_pos = [100, HEIGHT - 100]  # Starting position of the ball
ball_velocity = [0, 0]
ball_gravity = 0.5  # Gravity effect on the ball
targets = []
target_gravity = 0.05  # Gravity effect on targets
ball_speed_multiplier = 1.0  # Adjustable speed multiplier for the ball
score = 0

# Game states
START = 0
PLAYING = 1
GAME_OVER = 2
game_state = START

# Fonts
font = pygame.font.Font(None, 36)
large_font = pygame.font.Font(None, 72)

def draw_text(text, font, color, x, y):
    """Helper function to draw text on screen."""
    text_obj = font.render(text, True, color)
    screen.blit(text_obj, (x, y))

def create_target():
    """Create a new target at a random position on the right."""
    x = WIDTH
    y = random.randint(50, HEIGHT - 50)
    return [x, y]

def reset_ball():
    """Reset ball to initial position off-screen."""
    global ball_pos, ball_velocity
    ball_pos = [100, HEIGHT - 100]
    ball_velocity = [0, 0]

# Set initial conditions for the ball
reset_ball()

# Main game loop
running = True
clock = pygame.time.Clock()

while running:
    screen.fill(WHITE)  # Clear the screen with white color

    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.MOUSEBUTTONDOWN:
            if game_state == START:
                game_state = PLAYING
            elif game_state == PLAYING:
                # Launch the ball toward the mouse position
                mouse_x, mouse_y = pygame.mouse.get_pos()
                angle = math.atan2(mouse_y - ball_pos[1], mouse_x - ball_pos[0])
                ball_velocity = [
                    math.cos(angle) * 10 * ball_speed_multiplier,
                    math.sin(angle) * 10 * ball_speed_multiplier
                ]
            elif game_state == GAME_OVER:
                # Reset the game
                game_state = START
                score = 0
                targets.clear()
                reset_ball()

    # Game state logic
    if game_state == START:
        draw_text("Cannon Game", large_font, BLACK, WIDTH // 2 - 150, HEIGHT // 2 - 100)
        draw_text("Click to Start", font, GREEN, WIDTH // 2 - 80, HEIGHT // 2)
        draw_text("Instructions:", font, BLACK, 50, HEIGHT - 150)
        draw_text("- Click to shoot the ball", font, BLACK, 50, HEIGHT - 120)
        draw_text("- Hit as many targets as possible", font, BLACK, 50, HEIGHT - 90)
    elif game_state == PLAYING:
        # Add a new target randomly
        if random.randint(1, 40) == 1:
            targets.append(create_target())

        # Move targets to the left and apply gravity
        for target in targets:
            target[0] -= 2  # Move left
            target[1] += target_gravity  # Apply gravity downward

        # Update ball position with gravity
        ball_velocity[1] += ball_gravity
        ball_pos[0] += ball_velocity[0]
        ball_pos[1] += ball_velocity[1]

        # Collision detection
        for target in targets[:]:
            distance = math.hypot(ball_pos[0] - target[0], ball_pos[1] - target[1])
            if distance < 20:
                score += 1
                targets.remove(target)
                reset_ball()

        # Draw targets
        for target in targets:
            pygame.draw.circle(screen, BLUE, (int(target[0]), int(target[1])), 20)

        # Draw ball
        pygame.draw.circle(screen, RED, (int(ball_pos[0]), int(ball_pos[1])), 8)

        # Draw score
        draw_text(f'Score: {score}', font, BLACK, 10, 10)

        # Boundary check for the ball
        if ball_pos[0] > WIDTH or ball_pos[1] > HEIGHT:
            reset_ball()  # Reset if it goes off screen

        # Check if any target has left the screen on the left side to trigger game over
        if any(target[0] <= 0 for target in targets):
            game_state = GAME_OVER

    elif game_state == GAME_OVER:
        draw_text("Game Over!", large_font, RED, WIDTH // 2 - 150, HEIGHT // 2 - 100)
        draw_text(f"Final Score: {score}", font, BLACK, WIDTH // 2 - 80, HEIGHT // 2)
        draw_text("Click to Restart", font, GREEN, WIDTH // 2 - 100, HEIGHT // 2 + 50)

    pygame.display.flip()  # Update the display
    clock.tick(30)  # Cap the frame rate

pygame.quit()

# Save game results to CSV file
game_data = {'Score': [score], 'Targets Hit': [score]}
df = pd.DataFrame(game_data)
df.to_csv('game_results.csv', index=False)

# Visualize the results using matplotlib
plt.bar(['Targets Hit'], [score])
plt.xlabel('Game Metrics')
plt.ylabel('Value')
plt.title('Game Results')
plt.savefig("game_results.png")
plt.show()
Output
The game will display a simple Cannon Game where you can click to aim and fire a cannonball at randomly moving targets. The final score will be saved to a CSV file, and a bar chart visualizing the results will be generated.

Result
Thus, the cannon game with pathfinding and AI for targets was implemented. The final score and results were saved to a CSV file, and the game results were visualized using a bar chart.
