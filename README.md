import tkinter as tk
#from PIL import Image, ImageTk

root = tk.Tk()
root.title("Greenfield Local Hub")
root.geometry("600x500")
root.configure(bg="#e8f5e9")  # light green background

# ---------------- HEADER ----------------
header = tk.Frame(root, bg="#2e7d32", height=80)
header.pack(fill="x")

title = tk.Label(
    header,
    text="Greenfield Local Hub",
    font=("Arial", 18, "bold"),
    fg="white",
    bg="#2e7d32"
)
title.pack(pady=20)

# ---------------- MAIN CARD ----------------
card = tk.Frame(root, bg="white", bd=2, relief="raised")
card.place(relx=0.5, rely=0.5, anchor="center", width=350, height=300)

# ---------------- LOGO IMAGE ----------------
try:
    logo = tk.PhotoImage(file=r"C:\Users\TLevel-Digital-OS-11\Desktop\OSP\logo.png")  # your logo file
    img = img.resize((80, 80))
    logo = ImageTk.PhotoImage(img)

    logo_label = tk.Label(card, image=logo, bg="white")
    logo_label.pack(pady=10)
except:
    tk.Label(card, text="[Logo Here]", bg="white").pack(pady=10)

# ---------------- TEXT ----------------
tk.Label(
    card,
    text="Welcome",
    font=("Arial", 14, "bold"),
    bg="white"
).pack()

tk.Label(
    card,
    text="Select your role to continue",
    font=("Arial", 10),
    bg="white"
).pack(pady=5)

# ---------------- FUNCTIONS ----------------
def open_customer():
    open_login("Customer Login")

def open_producer():
    open_login("Producer Login")

def open_login(role):
    win = tk.Toplevel(root)
    win.title(role)
    win.geometry("350x250")

    tk.Label(win, text=role, font=("Arial", 14, "bold")).pack(pady=10)

    tk.Label(win, text="Email").pack()
    email = tk.Entry(win)
    email.pack()

    tk.Label(win, text="Password").pack()
    password = tk.Entry(win, show="*")
    password.pack()

    tk.Button(win, text="Login", bg="#4CAF50", fg="white").pack(pady=10)

# ---------------- BUTTONS ----------------
btn_customer = tk.Button(
    card,
    text="Customer",
    width=20,
    bg="#4CAF50",
    fg="white",
    font=("Arial", 11),
    command=open_customer
)
btn_customer.pack(pady=10)

btn_producer = tk.Button(
    card,
    text="Producer",
    width=20,
    bg="#1976D2",
    fg="white",
    font=("Arial", 11),
    command=open_producer
)
btn_producer.pack(pady=5)

# ---------------- FOOTER ----------------
footer = tk.Label(
    root,
    text="Supporting Local Farmers & Producers",
    bg="#e8f5e9",
    font=("Arial", 9)
)
footer.pack(side="bottom", pady=10)

root.mainloop()
