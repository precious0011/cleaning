import tkinter as tk
from tkinter import messagebox
import sqlite3

# ---------------- DATABASE ----------------
conn = sqlite3.connect("glh.db")
cursor = conn.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT,
    email TEXT,
    password TEXT,
    address TEXT,
    county TEXT
)
""")

cursor.execute("""
CREATE TABLE IF NOT EXISTS orders (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_email TEXT,
    items TEXT,
    total REAL
)
""")

conn.commit()

# ---------------- MAIN WINDOW ----------------
root = tk.Tk()
root.title("Greenfield Local Hub")
root.geometry("700x550")
root.configure(bg="#e8f5e9")

# ---------------- HEADER ----------------
header = tk.Frame(root, bg="#2e7d32", height=80)
header.pack(fill="x")

tk.Label(header, text="Greenfield Local Hub",
         font=("Arial", 18, "bold"),
         fg="white", bg="#2e7d32").pack(pady=20)

# ---------------- CARD ----------------
card = tk.Frame(root, bg="white", bd=2, relief="raised")
card.place(relx=0.5, rely=0.5, anchor="center", width=400, height=350)

# ---------------- LOGO ----------------
try:
    logo = tk.PhotoImage(file="images/logo.png")
    logo_label = tk.Label(card, image=logo, bg="white")
    logo_label.image = logo
    logo_label.pack(pady=5)
except:
    tk.Label(card, text="[Logo Here]", bg="white").pack(pady=10)

# ---------------- TEXT ----------------
tk.Label(card, text="Welcome",
         font=("Arial", 14, "bold"),
         bg="white").pack()

tk.Label(card,
         text="Select your role to continue",
         bg="white").pack(pady=5)

# ---------------- FUNCTIONS ----------------

def open_customer():
    open_login()

def open_producer():
    messagebox.showinfo("Info", "Producer dashboard coming soon")

# -------- LOGIN --------
def open_login():
    win = tk.Toplevel(root)
    win.title("Customer Login")
    win.geometry("350x300")

    tk.Label(win, text="Login", font=("Arial", 14)).pack(pady=10)

    tk.Label(win, text="Email").pack()
    email = tk.Entry(win)
    email.pack()

    tk.Label(win, text="Password").pack()
    password = tk.Entry(win, show="*")
    password.pack()

    def login():
        user_email = email.get().strip().lower()
        user_password = password.get().strip()

        if user_email == "" or user_password == "":
            messagebox.showerror("Error", "Fill all fields")
            return

        cursor.execute("SELECT * FROM users WHERE email=? AND password=?",
                       (user_email, user_password))
        user = cursor.fetchone()

        if user:
            messagebox.showinfo("Success", "Login successful")
            win.destroy()
            open_dashboard(user)
        else:
            messagebox.showerror("Error", "Invalid email or password")

    tk.Button(win, text="Login", bg="#4CAF50", fg="white",
              command=login).pack(pady=10)

    tk.Button(win, text="Don't have an account?",
              fg="blue",
              command=lambda: open_register(win)).pack()

# -------- REGISTER --------
def open_register(parent):
    parent.destroy()

    win = tk.Toplevel(root)
    win.title("Register")
    win.geometry("350x450")

    tk.Label(win, text="Create Account", font=("Arial", 14)).pack(pady=10)

    tk.Label(win, text="Username").pack()
    username = tk.Entry(win)
    username.pack()

    tk.Label(win, text="Email Address").pack()
    email = tk.Entry(win)
    email.pack()

    tk.Label(win, text="Password").pack()
    password = tk.Entry(win, show="*")
    password.pack()

    tk.Label(win, text="Home Address").pack()
    address = tk.Entry(win)
    address.pack()

    tk.Label(win, text="County").pack()
    county = tk.Entry(win)
    county.pack()

    def register():
        user_username = username.get().strip()
        user_email = email.get().strip().lower()
        user_password = password.get().strip()
        user_address = address.get().strip()
        user_county = county.get().strip()

        if (user_username == "" or user_email == "" or
            user_password == "" or user_address == "" or user_county == ""):
            messagebox.showerror("Error", "Fill all fields")
            return

        cursor.execute("SELECT * FROM users WHERE email=?", (user_email,))
        if cursor.fetchone():
            messagebox.showerror("Error", "Email already exists")
            return

        cursor.execute("""
        INSERT INTO users (username, email, password, address, county)
        VALUES (?, ?, ?, ?, ?)
        """, (user_username, user_email, user_password,
              user_address, user_county))

        conn.commit()

        messagebox.showinfo("Success", "Account created")
        win.destroy()
        open_dashboard((None, user_username, user_email))

    tk.Button(win, text="Register", bg="#4CAF50", fg="white",
              command=register).pack(pady=15)

# -------- DASHBOARD --------
def open_dashboard(user):
    dash = tk.Toplevel(root)
    dash.title("GLH Store")
    dash.geometry("950x650")
    dash.configure(bg="white")

    user_email = user[2]
    cart = []

    # HEADER
    header = tk.Frame(dash, bg="#2e7d32", height=60)
    header.pack(fill="x")

    tk.Label(header, text="GLH Store",
             fg="white", bg="#2e7d32",
             font=("Arial", 16, "bold")).pack(side="left", padx=10)

    # CART
    def view_cart():
        win = tk.Toplevel(dash)
        win.title("Cart")
        win.geometry("400x400")

        total = 0

        for item in cart:
            tk.Label(win, text=f"{item[0]} - £{item[1]}").pack()
            total += item[1]

        tk.Label(win, text=f"Total: £{total}",
                 font=("Arial", 12, "bold")).pack(pady=10)

        def checkout():
            if not cart:
                messagebox.showerror("Error", "Cart empty")
                return

            items = ", ".join([i[0] for i in cart])

            cursor.execute("""
            INSERT INTO orders (user_email, items, total)
            VALUES (?, ?, ?)
            """, (user_email, items, total))
            conn.commit()

            cart.clear()
            messagebox.showinfo("Success", "Order placed")
            win.destroy()

        tk.Button(win, text="Checkout",
                  bg="#4CAF50", fg="white",
                  command=checkout).pack(pady=10)

    tk.Button(header, text="Cart 🛒",
              command=view_cart).pack(side="right", padx=10)

    # ORDER HISTORY
    def view_orders():
        win = tk.Toplevel(dash)
        win.title("Orders")
        win.geometry("400x400")

        cursor.execute("SELECT items, total FROM orders WHERE user_email=?", (user_email,))
        orders = cursor.fetchall()

        if not orders:
            tk.Label(win, text="No orders yet").pack()
        else:
            for o in orders:
                tk.Label(win, text=f"{o[0]} | £{o[1]}").pack()

    tk.Button(header, text="Orders 📦",
              command=view_orders).pack(side="right")

    # PRODUCTS
    main = tk.Frame(dash, bg="white")
    main.pack(pady=20)

    # LOAD IMAGES
    milk = tk.PhotoImage(file="images/milk.png")
    bread = tk.PhotoImage(file="images/bread.png")
    eggs = tk.PhotoImage(file="images/eggs.png")
    veg = tk.PhotoImage(file="images/veg.png")

    products = [
        ("Milk", 1.50, milk),
        ("Bread", 1.20, bread),
        ("Eggs", 2.50, eggs),
        ("Vegetables", 3.00, veg),
    ]

    def add_to_cart(name, price):
        cart.append((name, price))
        messagebox.showinfo("Cart", f"{name} added")

    row = 0
    col = 0

    for name, price, img in products:
        box = tk.Frame(main, bd=1, relief="solid")
        box.grid(row=row, column=col, padx=15, pady=15)

        lbl = tk.Label(box, image=img)
        lbl.image = img
        lbl.pack()

        tk.Label(box, text=name).pack()
        tk.Label(box, text=f"£{price}", fg="green").pack()

        tk.Button(box, text="Add to Cart",
                  command=lambda n=name, p=price: add_to_cart(n, p)).pack()

        col += 1
        if col == 2:
            col = 0
            row += 1

# ---------------- BUTTONS ----------------
tk.Button(card, text="Customer",
          bg="#4CAF50", fg="white",
          width=20,
          command=open_customer).pack(pady=10)

tk.Button(card, text="Producer",
          bg="#1976D2", fg="white",
          width=20,
          command=open_producer).pack()

# ---------------- FOOTER ----------------
tk.Label(root,
         text="Supporting Local Farmers & Producers",
         bg="#e8f5e9").pack(side="bottom", pady=10)

root.mainloop()
