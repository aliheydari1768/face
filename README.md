import pygame
import random

# Start PyGame
pygame.init()

family_members = [
    {"name": "Ali", "age": 45, "relation": "Father", "photo": "C:\\Users\\ASUS\\OneDrive\\Desktop\\heydari\\ali.jpg"},
    {"name": "Zahra", "age": 40, "relation": "Mother", "photo": "C:\\Users\\ASUS\\OneDrive\\Desktop\\heydari\\zahra.jpg"},
    {"name": "Hossein", "age": 20, "relation": "Brother", "photo": "C:\\Users\\ASUS\\OneDrive\\Desktop\\heydari\\hossein.jpg"},
    {"name": "Maryam", "age": 18, "relation": "Sister", "photo": "C:\\Users\\ASUS\\OneDrive\\Desktop\\heydari\\maryam.jpg"}
]

# Definition of game variables
score = 0
errors = 0
max_mistakes = 3
current_member = None

# Screen settings
screen = pygame.display.set_mode((800, 600))
pygame.display.set_caption("Family Memory Game")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Fonts
font = pygame.font.Font(None, 36)

# Function to display the current member's photo and information
def show_current_member():
    screen.fill(WHITE)
    if current_member:
        image = pygame.image.load(current_member["photo"])
        image = pygame.transform.scale(image, (400, 400))
        screen.blit(image, (200, 50))
        info_text = f"Age: {current_member['age']} Relation: {current_member['relation']}"
        text_surface = font.render(info_text, True, BLACK)
        screen.blit(text_surface, (200, 470))
    pygame.display.update()

# Function to select a random family member
def select_random_member():
    global current_member
    current_member = random.choice(family_members)

# Response check function
def check_answer(user_answer):
    global score, errors
    correct_answer = current_member["name"].strip()
    if user_answer.lower() == correct_answer.lower():
        score += 1
        show_message("Correct!", f"Well done! Your score: {score}")
    else:
        errors += 1
        show_message("Wrong!", f"Sorry! Wrong answer. Your mistakes: {errors}")
        if errors >= max_mistakes:
            show_message("Game Over", f"Game over. Your final score: {score}")
            pygame.quit()
            exit()

# Function to display a message
def show_message(title, message):
    screen.fill(WHITE)
    title_surface = font.render(title, True, BLACK)
    message_surface = font.render(message, True, BLACK)
    screen.blit(title_surface, (400 - title_surface.get_width() // 2, 250))
    screen.blit(message_surface, (400 - message_surface.get_width() // 2, 300))
    pygame.display.update()
    pygame.time.wait(2000)

# The main loop of the game
running = True
user_input = ""
show_member_info = False
select_random_member()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RETURN and not show_member_info:
                check_answer(user_input)
                show_member_info = True
            elif event.key == pygame.K_SPACE and show_member_info:
                select_random_member()
                user_input = ""
                show_member_info = False
            elif event.key == pygame.K_BACKSPACE:
                user_input = user_input[:-1]
            else:
                if not show_member_info:
                    user_input += event.unicode

    screen.fill(WHITE)
    if show_member_info:
        show_current_member()
    else:
        input_prompt = font.render("Enter the full name:", True, BLACK)
        screen.blit(input_prompt, (200, 20))
        input_surface = font.render(user_input, True, BLACK)
        screen.blit(input_surface, (200, 60))

    pygame.display.update()

pygame.quit()

