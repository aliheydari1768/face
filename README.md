import tkinter as tk
from tkinter import messagebox
from PIL import Image, ImageTk
import random

# تعریف اطلاعات اعضای خانواده
family_members = [
    {"name": "Ali", "age": 45, "relation": "Father", "photo": "C:/Users/ASUS/OneDrive/Desktop/heydari/ali.jpg"},
    {"name": "Zahra", "age": 40, "relation": "Mother", "photo": "C:/Users/ASUS/OneDrive/Desktop/heydari/zahra.jpg"},
    {"name": "Hossein", "age": 20, "relation": "Brother", "photo": "C:/Users/ASUS/OneDrive/Desktop/heydari/hossein.jpg"},
    {"name": "Maryam", "age": 18, "relation": "Sister", "photo": "C:/Users/ASUS/OneDrive/Desktop/heydari/maryam.jpg"}
]

# تعریف متغیرهای بازی
score = 0
mistakes = 0
max_mistakes = 3
current_member = None
language = "English"  # زبان پیش‌فرض انگلیسی است

# ایجاد پنجره اصلی
root = tk.Tk()
root.title("Family Memory Game")

# تابع انتخاب زبان
def choose_language():
    global language
    lang_choice = messagebox.askquestion("Language Selection", "Game in English or Persian?")
    if lang_choice == "no":
        language = "Persian"

choose_language()

# تابع نمایش پیام به زبان انتخابی
def get_text(english_text, persian_text):
    return persian_text if language == "Persian" else english_text

# ایجاد فریم برای نمایش عکس و اطلاعات
frame = tk.Frame(root)
frame.pack(pady=20)

# نمایش عکس
photo_label = tk.Label(frame)
photo_label.pack()

# نمایش اطلاعات
info_label = tk.Label(frame, text="", font=("Helvetica", 14))
info_label.pack(pady=10)

# ایجاد کادر برای ورود نام
entry = tk.Entry(root, font=("Helvetica", 14))
entry.pack(pady=10)

# تابع برای نمایش عکس و اطلاعات تصادفی
def show_random_member():
    global current_member
    if not family_members:
        messagebox.showinfo(get_text("Game Completed", "پایان بازی"),
                            get_text(f"You've seen all the family members! Your final score: {score}", 
                                     f"شما همه اعضای خانواده را مشاهده کرده‌اید! امتیاز نهایی شما: {score}"))
        root.quit()
        return
    
    current_member = random.choice(family_members)
    family_members.remove(current_member)  # حذف عضو فعلی از لیست
    try:
        image = Image.open(current_member["photo"])
        photo = ImageTk.PhotoImage(image)
        photo_label.config(image=photo)
        photo_label.image = photo
        info_label.config(text=get_text(f"Age: {current_member['age']}\nRelation: {current_member['relation']}",
                                        f"سن: {current_member['age']}\nرابطه: {current_member['relation']}"))
    except Exception as e:
        messagebox.showerror("Error", f"Could not load image: {e}")

# تابع بررسی پاسخ
def check_answer():
    global score, mistakes
    user_answer = entry.get().strip()
    correct_answer = current_member["name"].strip()
    print(f"User Answer: '{user_answer}'")  # چاپ پاسخ کاربر
    print(f"Correct Answer: '{correct_answer}'")  # چاپ پاسخ صحیح
    if user_answer.lower() == correct_answer.lower():
        score += 1
        messagebox.showinfo(get_text("Correct Answer", "پاسخ درست"),
                            get_text(f"Well done! Your score: {score}", f"آفرین! امتیاز شما: {score}"))
        show_random_member()
    else:
        mistakes += 1
        messagebox.showwarning(get_text("Wrong Answer", "پاسخ نادرست"),
                               get_text(f"Sorry! Wrong answer. Your mistakes: {mistakes}",
                                        f"متاسفانه پاسخ نادرست است. تعداد اشتباهات شما: {mistakes}"))
        if mistakes >= max_mistakes:
            messagebox.showerror(get_text("Game Over", "پایان بازی"),
                                 get_text(f"Game over. Your final score: {score}",
                                          f"بازی تمام شد. امتیاز نهایی شما: {score}"))
            root.quit()
    entry.delete(0, tk.END)

# ایجاد دکمه ارسال
submit_button = tk.Button(root, text=get_text("Submit", "ارسال"), font=("Helvetica", 14), command=check_answer)
submit_button.pack(pady=10)

# نمایش اولین عکس و اطلاعات
show_random_member()

# شروع حلقه اصلی tkinter
root.mainloop()
