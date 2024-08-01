import pygame
import random

# راه‌اندازی PyGame
pygame.init()

# تعریف اطلاعات اعضای خانواده
family_members = [
    {"name": "Ali", "age": 45, "relation": "Father", "photo": "C:/Users/rezae/Desktop/ali.jpg"},
    {"name": "Zahra", "age": 40, "relation": "Mother", "photo": "C:/Users/rezae/Desktop/zahra.jpg"},
    {"name": "Hossein", "age": 20, "relation": "Brother", "photo": "C:/Users/rezae/Desktop/hossein.jpg"},
    {"name": "Maryam", "age": 18, "relation": "Sister", "photo": "C:/Users/rezae/Desktop/maryam.jpg"},
]

# تعریف متغیرهای بازی
score = 0
mistakes = 0
max_mistakes = 3
current_member = None

# تنظیمات صفحه نمایش
screen = pygame.display.set_mode((800, 600))
pygame.display.set_caption("Family Memory Game")

# رنگ‌ها
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# فونت‌ها
font = pygame.font.Font(None, 36)

# تابع برای نمایش عکس و اطلاعات تصادفی
def show_random_member():
    global current_member
    current_member = random.choice(family_members)
    image = pygame.image.load(current_member["photo"])
    image = pygame.transform.scale(image, (400, 400))
    screen.blit(image, (200, 50))
    info_text = f"Age: {current_member['age']}  Relation: {current_member['relation']}"
    text_surface = font.render(info_text, True, BLACK)
    screen.blit(text_surface, (200, 470))
    pygame.display.update()

# تابع بررسی پاسخ
def check_answer(user_answer):
    global score, mistakes
    correct_answer = current_member["name"].strip()
    if user_answer.lower() == correct_answer.lower():
        score += 1
        show_message("Correct!", f"Well done! Your score: {score}")
        show_random_member()
    else:
        mistakes += 1
        show_message("Wrong!", f"Sorry! Wrong answer. Your mistakes: {mistakes}")
        if mistakes >= max_mistakes:
            show_message("Game Over", f"Game over. Your final score: {score}")
            pygame.quit()
            exit()

# تابع برای نمایش پیام
def show_message(title, message):
    screen.fill(WHITE)
    title_surface = font.render(title, True, BLACK)
    message_surface = font.render(message, True, BLACK)
    screen.blit(title_surface, (400 - title_surface.get_width() // 2, 250))
    screen.blit(message_surface, (400 - message_surface.get_width() // 2, 300))
    pygame.display.update()
    pygame.time.wait(2000)

# نمایش اولین عکس و اطلاعات
show_random_member()

# حلقه اصلی بازی
running = True
user_input = ""
while running:
    screen.fill(WHITE)
    show_random_member()
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RETURN:
                check_answer(user_input)
                user_input = ""
            elif event.key == pygame.K_BACKSPACE:
                user_input = user_input[:-1]
            else:
                user_input += event.unicode
    # نمایش ورودی کاربر
    input_surface = font.render(user_input, True, BLACK)
    screen.blit(input_surface, (200, 520))
    pygame.display.update()

pygame.quit()
