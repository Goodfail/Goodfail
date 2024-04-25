import pygame
import random

# Initialisation de pygame
pygame.init()

# Définition des couleurs
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)

# Paramètres de la fenêtre
WINDOW_WIDTH = 400
WINDOW_HEIGHT = 400
CELL_SIZE = 20

# Création de la fenêtre
window = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))
pygame.display.set_caption('Snake Game')

clock = pygame.time.Clock()

# Fonction principale du jeu
def game():
    snake = [(100, 100)]  # Position initiale du serpent
    direction = (1, 0)  # Direction initiale (vers la droite)
    food = create_food(snake)  # Position de la nourriture

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    direction = (0, -1)
                elif event.key == pygame.K_DOWN:
                    direction = (0, 1)
                elif event.key == pygame.K_LEFT:
                    direction = (-1, 0)
                elif event.key == pygame.K_RIGHT:
                    direction = (1, 0)

        # Déplacer le serpent
        head = (snake[0][0] + direction[0] * CELL_SIZE, snake[0][1] + direction[1] * CELL_SIZE)
        snake.insert(0, head)

        # Vérifier les collisions
        if snake[0] == food:
            food = create_food(snake)
        else:
            snake.pop()

        # Vérifier les limites de l'écran
        if not (0 <= snake[0][0] < WINDOW_WIDTH and 0 <= snake[0][1] < WINDOW_HEIGHT):
            running = False

        # Vider l'écran
        window.fill(BLACK)

        # Dessiner le serpent
        for segment in snake:
            pygame.draw.rect(window, GREEN, (segment[0], segment[1], CELL_SIZE, CELL_SIZE))

        # Dessiner la nourriture
        pygame.draw.rect(window, WHITE, (food[0], food[1], CELL_SIZE, CELL_SIZE))

        # Mettre à jour l'affichage
        pygame.display.flip()

        clock.tick(10)  # Vitesse du serpent

    pygame.quit()

# Fonction pour créer une nouvelle position de nourriture
def create_food(snake):
    while True:
        food = (random.randint(0, WINDOW_WIDTH // CELL_SIZE - 1) * CELL_SIZE,
                random.randint(0, WINDOW_HEIGHT // CELL_SIZE - 1) * CELL_SIZE)
        if food not in snake:
            return food

# Lancer le jeu
game()

