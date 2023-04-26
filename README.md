import pygame
import random

# initialize pygame
pygame.init()

# set up the screen
WIDTH = 800
HEIGHT = 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Car Racing Game")

# set up the car
car_image = pygame.image.load("car.png")
car_width = 50
car_height = 100
car_x = WIDTH / 2 - car_width / 2
car_y = HEIGHT - car_height - 10
car_speed = 10

# set up the obstacles
obstacle_width = 50
obstacle_height = 50
obstacle_x = random.randint(0, WIDTH - obstacle_width)
obstacle_y = -obstacle_height
obstacle_speed = 5

# set up the font
font = pygame.font.SysFont(None, 30)

# set up the game loop
clock = pygame.time.Clock()
score = 0
game_over = False

while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    # move the car
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and car_x > 0:
        car_x -= car_speed
    if keys[pygame.K_RIGHT] and car_x < WIDTH - car_width:
        car_x += car_speed

    # move the obstacle
    obstacle_y += obstacle_speed
    if obstacle_y > HEIGHT:
        obstacle_x = random.randint(0, WIDTH - obstacle_width)
        obstacle_y = -obstacle_height
        score += 1
        obstacle_speed += 1

    # check for collision
    if car_x < obstacle_x + obstacle_width and car_x + car_width > obstacle_x and car_y < obstacle_y + obstacle_height and car_y + car_height > obstacle_y:
        game_over = True

    # draw the objects on the screen
    screen.fill((255, 255, 255))
    screen.blit(car_image, (car_x, car_y))
    pygame.draw.rect(screen, (255, 0, 0), (obstacle_x, obstacle_y, obstacle_width, obstacle_height))
    score_text = font.render("Score: " + str(score), True, (0, 0, 0))
    screen.blit(score_text, (10, 10))

    # update the screen
    pygame.display.update()

    # set the FPS
    clock.tick(60)

# game over message
game_over_text = font.render("Game Over! Your Score: " + str(score), True, (0, 0, 0))
screen.blit(game_over_text, (WIDTH/2 - game_over_text.get_width()/2, HEIGHT/2 - game_over_text.get_height()/2))
pygame.display.update()

# wait for 3 seconds before quitting
pygame.time.wait(3000)

# quit the game
pygame.quit()
