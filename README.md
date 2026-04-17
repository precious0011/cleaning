Version 1

import tkinter as tk
from tkinter import messagebox


# ---------------- MAIN WINDOW ----------------
root = tk.Tk()
root.title("TOKA FITNESS")
root.geometry("700x550")
root.configure(bg="#e8f5e9")

# ---------------- HEADER ----------------
header = tk.Frame(root, bg="#2e7d32", height=80)
header.pack(fill="x")

tk.Label(header, text="TOKA FITNESS",
         font=("Arial", 18, "bold"),
         fg="white", bg="#2e7d32").pack(pady=20)

# ---------------- CARD ----------------
card = tk.Frame(root, bg="white", bd=2, relief="raised")
card.place(relx=0.5, rely=0.5, anchor="center", width=400, height=300)

tk.Label(card, text="Welcome", font=("Arial", 16, "bold"), bg="white").pack(pady=10)
tk.Label(card, text="Select your role", bg="white").pack(pady=5)

# ---------------- FUNCTIONS ----------------

def open_customer():
    # Changed to show "Coming Soon" instead of opening login
    messagebox.showinfo("Info", "Customer portal coming soon")

def open_producer():
    messagebox.showinfo("Info", "Producer dashboard coming soon")

# Note: open_login, open_register, and open_dashboard are kept in your 
# script background so you can use them later when you're ready!

# ---------------- BUTTONS ----------------
tk.Button(card, text="Customer", width=20,
          bg="#4CAF50", fg="white",
          command=open_customer).pack(pady=10)

tk.Button(card, text="Producer", width=20,
          bg="#1976D2", fg="white",
          command=open_producer).pack()

# ---------------- FOOTER ----------------
tk.Label(root,
         text="Supporting Local Farmers & Producers",
         bg="#e8f5e9").pack(side="bottom", pady=10)

# ---------------- RUN ----------------
root.mainloop()


Version 2
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
conn.commit()

# ---------------- MAIN WINDOW ----------------
root = tk.Tk()
root.title("TOKA FINESS")
root.geometry("700x550")
root.configure(bg="#e8f5e9")

# ---------------- HEADER ----------------
header = tk.Frame(root, bg="#2e7d32", height=80)
header.pack(fill="x")

tk.Label(header, text="TOKA FITNESS",
         font=("Arial", 18, "bold"),
         fg="white", bg="#2e7d32").pack(pady=20)

# ---------------- CARD ----------------
card = tk.Frame(root, bg="white", bd=2, relief="raised")
card.place(relx=0.5, rely=0.5, anchor="center", width=400, height=300)

tk.Label(card, text="Welcome", font=("Arial", 16, "bold"), bg="white").pack(pady=10)
tk.Label(card, text="Select your role", bg="white").pack(pady=5)

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
        if email.get() == "" or password.get() == "":
            messagebox.showerror("Error", "Please fill all fields")
            return

        cursor.execute("SELECT * FROM users WHERE email=? AND password=?",
                       (email.get(), password.get()))
        user = cursor.fetchone()

        if user:
            messagebox.showinfo("Success", "Login successful")
            win.destroy()
            open_dashboard(user)
        else:
            messagebox.showerror("Error", "Invalid email or password")

    tk.Button(win, text="Login", bg="#4CAF50", fg="white",
              command=login).pack(pady=10)

    tk.Button(win,
              text="Don't have an account?",
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
        if (username.get() == "" or email.get() == "" or
            password.get() == "" or address.get() == "" or county.get() == ""):
            messagebox.showerror("Error", "Please fill all fields")
            return

        cursor.execute("SELECT * FROM users WHERE email=?", (email.get(),))
        existing_user = cursor.fetchone()

        if existing_user:
            messagebox.showerror("Error", "Email already registered")
            return

        cursor.execute("""
        INSERT INTO users (username, email, password, address, county)
        VALUES (?, ?, ?, ?, ?)
        """, (username.get(), email.get(), password.get(),
              address.get(), county.get()))

        conn.commit()

        messagebox.showinfo("Success", "Account created successfully")

        win.destroy()
        open_dashboard()

    tk.Button(win, text="Register", bg="#4CAF50", fg="white",
              command=register).pack(pady=15)

# -------- DASHBOARD --------
def open_dashboard(user=None):
    dash = tk.Toplevel(root)
    dash.title("GLH Store")
    dash.geometry("800x600")
    dash.configure(bg="white")

    # HEADER BAR
    top = tk.Frame(dash, bg="#2e7d32", height=60)
    top.pack(fill="x")

    tk.Label(top, text="GLH Store",
             fg="white", bg="#2e7d32",
             font=("Arial", 16, "bold")).pack(side="left", padx=10)

    search = tk.Entry(top, width=40)
    search.pack(side="left", padx=20)

    tk.Button(top, text="Search").pack(side="left")

    # NAV BAR
    nav = tk.Frame(dash, bg="#388e3c", height=40)
    nav.pack(fill="x")

    for item in ["All", "Fresh", "Deals", "Local", "Popular"]:
        tk.Button(nav, text=item, bg="#388e3c", fg="white", bd=0)\
            .pack(side="left", padx=10)

    # PRODUCT AREA
    main = tk.Frame(dash, bg="white")
    main.pack(fill="both", expand=True, pady=10)

    # LOAD IMAGES
    milk = tk.PhotoImage(file="images/milk.png")
    bread = tk.PhotoImage(file="images/bread.png")
    eggs = tk.PhotoImage(file="images/eggs.png")
    veg = tk.PhotoImage(file="images/veg.png")

    products = [
        ("Fresh Milk", "£1.50", milk),
        ("Organic Bread", "£1.20", bread),
        ("Farm Eggs", "£2.50", eggs),
        ("Fresh Veg", "£3.00", veg),
    ]

    row = 0
    col = 0

    for name, price, img in products:
        box = tk.Frame(main, bd=1, relief="solid", bg="white", width=180, height=200)
        box.grid(row=row, column=col, padx=15, pady=15)

        img_label = tk.Label(box, image=img, bg="white")
        img_label.image = img
        img_label.pack(pady=5)

        tk.Label(box, text=name, bg="white").pack()
        tk.Label(box, text=price, fg="green", bg="white").pack()

        tk.Button(box, text="Add to Cart", bg="#4CAF50", fg="white").pack(pady=5)

        col += 1
        if col == 4:
            col = 0
            row += 1

    # FOOTER
    footer = tk.Frame(dash, bg="#2e7d32", height=40)
    footer.pack(fill="x")

    tk.Label(footer, text="GLH © 2026",
             fg="white", bg="#2e7d32").pack()

# ---------------- BUTTONS ----------------
tk.Button(card, text="Customer", width=20,
          bg="#4CAF50", fg="white",
          command=open_customer).pack(pady=10)

tk.Button(card, text="Producer", width=20,
          bg="#1976D2", fg="white",
          command=open_producer).pack()

# ---------------- FOOTER ----------------
tk.Label(root,
         text="Supporting Local Farmers & Producers",
         bg="#e8f5e9").pack(side="bottom", pady=10)

# ---------------- RUN ----------------
root.mainloop()



Version 4(Producer Dashboard)
def open_producer_dashboard():
    p_win = tk.Toplevel(root)
    p_win.title("Producer Management")
    p_win.geometry("850x550")

    # Sidebar
    sidebar = tk.Frame(p_win, bg="#1b5e20", width=180) # Darker green sidebar
    sidebar.pack(side="left", fill="y")
    
    # Main Content Area
    container = tk.Frame(p_win, bg="white")
    container.pack(side="right", fill="both", expand=True)

    def clear_view():
        for widget in container.winfo_children():
            widget.destroy()

    # 1. VIEW PRODUCTS
    def view_products():
        clear_view()
        tk.Label(container, text="Current Inventory", font=("Arial", 16, "bold"), fg="#2e7d32", bg="white").pack(pady=10)
        
        tree = ttk.Treeview(container, columns=("ID", "Name", "Price", "Stock"), show="headings")
        tree.heading("ID", text="ID")
        tree.heading("Name", text="Product")
        tree.heading("Price", text="Price (£)")
        tree.heading("Stock", text="Current Stock")
        tree.pack(fill="both", expand=True, padx=20, pady=10)

        cursor.execute("SELECT id, name, price, stock FROM products")
        for row in cursor.fetchall():
            tree.insert("", "end", values=row)

    # 2. UPDATE INVENTORY (Name, Quantity, and Price)
    def update_inventory_ui():
        clear_view()
        tk.Label(container, text="Update Inventory", font=("Arial", 16, "bold"), fg="#2e7d32", bg="white").pack(pady=20)
        
        # Product Name
        tk.Label(container, text="Product Name:", bg="white").pack()
        name_entry = tk.Entry(container, width=35)
        name_entry.pack(pady=5)

        # Quantity
        tk.Label(container, text="Quantity in Stock:", bg="white").pack()
        stock_entry = tk.Entry(container, width=35)
        stock_entry.pack(pady=5)

        # Price
        tk.Label(container, text="Price per Unit (£):", bg="white").pack()
        price_entry = tk.Entry(container, width=35)
        price_entry.pack(pady=5)

        def save_changes():
            try:
                name = name_entry.get().strip()
                qty = int(stock_entry.get())
                price = float(price_entry.get())
                
                # Check if product exists first
                cursor.execute("SELECT * FROM products WHERE name=?", (name,))
                if cursor.fetchone():
                    cursor.execute("UPDATE products SET stock=?, price=? WHERE name=?", (qty, price, name))
                    messagebox.showinfo("Success", f"Updated {name} successfully.")
                else:
                    # Optional: Add as new product if it doesn't exist
                    cursor.execute("INSERT INTO products (name, price, stock) VALUES (?, ?, ?)", (name, price, qty))
                    messagebox.showinfo("Success", f"Added {name} as a new product.")
                
                conn.commit()
                view_products()
            except ValueError:
                messagebox.showerror("Error", "Please enter valid numbers for Stock and Price.")

        # Updated Green Button
        tk.Button(container, text="Save Inventory Changes", bg="#4CAF50", fg="white", 
                  font=("Arial", 10, "bold"), command=save_changes, width=25, height=2).pack(pady=20)

    # 3. VIEW ORDERS
    def view_orders():
        clear_view()
        tk.Label(container, text="Customer Orders", font=("Arial", 16, "bold"), fg="#2e7d32", bg="white").pack(pady=10)
        
        tree = ttk.Treeview(container, columns=("Email", "Items", "Total"), show="headings")
        tree.heading("Email", text="Customer Email")
        tree.heading("Items", text="Items Purchased")
        tree.heading("Total", text="Total (£)")
        tree.pack(fill="both", expand=True, padx=20, pady=10)

        cursor.execute("SELECT user_email, items, total FROM orders")
        for row in cursor.fetchall():
            tree.insert("", "end", values=row)

    # SIDEBAR BUTTONS
    tk.Label(sidebar, text="DASHBOARD", fg="white", bg="#1b5e20", font=("Arial", 12, "bold")).pack(pady=20)
    
    btn_opts = {"bg": "#2e7d32", "fg": "white", "bd": 0, "pady": 12, "width": 20, "font": ("Arial", 10)}
    tk.Button(sidebar, text="View Products", command=view_products, **btn_opts).pack(pady=2)
    tk.Button(sidebar, text="Update Inventory", command=update_inventory_ui, **btn_opts).pack(pady=2)
    tk.Button(sidebar, text="View Orders", command=view_orders, **btn_opts).pack(pady=2)

    view_products()
