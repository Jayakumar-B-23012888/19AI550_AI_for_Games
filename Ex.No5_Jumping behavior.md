# Ex.No: 5  Implementation of Jumping behavior 

#### DATE: 23.08.2024 
#### NAME: JAYAKUMAR B
#### REGISTER NUMBER : 212222240117

### AIM: 
To write a python program to simulate Jumbing behavior. 
### Algorithm:
1. Start the program
2. Import the necessary modules
3. Initiate the pygame engine and window
4. Specify the necessary parameter for player height,depth,gravity,jump power. 
5. Create a game loop to simulate the continuous behavior.
6. If Quit button is pressed then quit the pygame window.
7. Move the player left when left button is pressed
8. Move the player right when right button is pressed
9. If space bar is pressed then enable the jump by increasing y axis value.
10. land the player and display the player at every timestep
11.  Stop the program
 ### Program:
```
import pygame
from pygame.locals import *
from sys import exit

sprite_image_filename = r'C:\Users\SEC\OneDrive\Pictures\Screenshots\Screenshot 2024-04-11 225559.png'
pygame.init()
screen = pygame.display.set_mode((640, 480), 0, 32)
sprite = pygame.image.load(sprite_image_filename)
screen.fill((0, 0, 0))
clock = pygame.time.Clock()
```
```

speed = 250
x = 0.0

y = 100
y_velocity = 0
gravity = 1000  # pixels per second squared
jump_speed = -500  # initial jump velocity
```
```
is_jumping = False

while True:
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            exit()
        elif event.type == KEYDOWN:
            if event.key == K_SPACE and not is_jumping:
                y_velocity = jump_speed
                is_jumping = True
    
    screen.fill((0, 0, 0))
    

    time_passed = clock.tick(30)
    time_passed_seconds = time_passed / 1000.0
    

    distance_moved = time_passed_seconds * speed
    x += distance_moved
    

    y_velocity += gravity * time_passed_seconds
    y += y_velocity * time_passed_seconds
    

    if y >= 100:
        y = 100
        y_velocity = 0
        is_jumping = False
    

    screen.blit(sprite, (x, y))
    

    if x > 640:
        x -= 640
    
 
    pygame.display.update()
```

### Output:
![pygame simple jumping](https://github.com/user-attachments/assets/1d8f2364-f1a5-4f34-90a7-24210c5af93e)


### Result:
Thus the simple jumping behavior  was implemented.
