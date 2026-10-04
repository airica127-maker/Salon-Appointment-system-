import tkinter as tk
from tkinter import ttk, messagebox
from PIL import Image, ImageTk
from pathlib import Path
import json

# ============================================================
# GLAM SALON
# SALON APPOINTMENT AND MANAGEMENT SYSTEM
# ============================================================

APP_DIR = Path(__file__).resolve().parent

# ============================================================
# COLORS
# ============================================================

PINK = "#E889B4"
DARK_PINK = "#B84F82"
LIGHT_PINK = "#FCEAF3"
VERY_LIGHT = "#FFF8FC"
WHITE = "#FFFFFF"
TEXT = "#3F3540"
MUTED = "#8E8490"
BORDER = "#E9D8E1"


# ============================================================
# STAFF DATA
# ============================================================

STAFF = [
    (1, "Maria Santos", "Hair Stylist", "0917-111-1111"),
    (2, "Anna Cruz", "Nail Technician", "0917-222-2222"),
    (3, "Jessica Garcia", "Hair Colorist", "0917-333-3333"),
    (4, "Sofia Reyes", "Beauty Specialist", "0917-444-4444"),
    (5, "Angela Flores", "Spa Therapist", "0917-555-5555"),
    (6, "Patricia Ramos", "Hair Stylist", "0917-666-6666"),
    (7, "Catherine Lopez", "Nail Technician", "0917-777-7777"),
    (8, "Isabella Torres", "Beauty Specialist", "0917-888-8888"),
    (9, "Michelle Aquino", "Spa Therapist", "0917-999-9999"),
    (10, "Rachel Mendoza", "Hair Stylist", "0917-000-0000")
]


# ============================================================
# 5 AVAILABLE SERVICES
# ============================================================

SERVICES = {

    "Rebond": {
        "price": 1500,
        "desc": "Smooth and silky hair treatment",
        "image": "c:fae2c150-7a49-4c54-a67f-c11ecae7b99f.jpg"
    },

    "Manicure": {
        "price": 500,
        "desc": "Beautiful and relaxing nail care",
        "image": "c:0dd3bfa1-0605-4806-897c-a347794614d5.jpg"
    },

    "Spa": {
        "price": 800,
        "desc": "Relaxing spa and facial treatment",
        "image": "c:images/15dc8847-8f86-4ddc-b44e-54d3aaa14867.jpg"
    },

    "Haircut": {
        "price": 400,
        "desc": "Professional haircut and styling",
        "image": "images/1f2f5f04-723f-4504-9ca3-4bbd1df8aaf5.jpg"
    },

    "Hair Color": {
        "price": 1200,
        "desc": "Beautiful hair coloring service",
        "image":"images/26c2c601-62ff-4d96-bedb-7aafdaf8130a.jpg"
    }
}


# ============================================================
# PROMOS
# ============================================================

PROMOS = [

    (
        "Rebond",
        1800,
        1500,
        "20% OFF",
        "images/61aa3ffe-869a-4d87-b1d4-ec8259e1ea01.jpg"
    ),

    (
        "Manicure",
        600,
        500,
        "15% OFF",
        "images/f545a6c2-e293-4726-aec0-034331df1d03.jpg"
    ),

    (
        "Spa",
        1000,
        800,
        "15% OFF",
        "images/b3237a39-cbfb-4860-b5a6-4f7aff65cd10.jpg"
    )
]


# ============================================================
# APPOINTMENT HISTORY
# ============================================================

HISTORY = []
DATA_FILE = APP_DIR / "appointments.json"


def load_history():
    global HISTORY

    if DATA_FILE.exists():
        try:
            with open(DATA_FILE, "r", encoding="utf-8") as file:
                data = json.load(file)
            if isinstance(data, list):
                HISTORY = data
        except (json.JSONDecodeError, OSError):
            HISTORY = []


def save_history():
    try:
        with open(DATA_FILE, "w", encoding="utf-8") as file:
            json.dump(HISTORY, file, indent=4, ensure_ascii=False)
    except OSError:
        messagebox.showerror(
            "Save Error",
            "The appointment history could not be saved."
        )


# ============================================================
# MONEY FORMAT
# ============================================================

def money(value):
    return f"₱{value:,.2f}"


# ============================================================
# FIND IMAGE
# ============================================================

def find_image(filename):

    locations = [

        APP_DIR / filename,

        APP_DIR / "images" / filename,

        Path.cwd() / filename,

        Path.cwd() / "images" / filename
    ]

    for path in locations:

        if path.exists():
            return path

    return None


# ============================================================
# MAIN SALON APPLICATION
# ============================================================

class SalonApp(tk.Tk):

    def __init__(self):

        super().__init__()

        self.title(
            "GLAM SALON - Appointment and Management System"
        )

        self.geometry(
            "1250x760"
        )

        self.minsize(
            1050,
            650
        )

        self.configure(
            bg=VERY_LIGHT
        )

        self.images = {}

        self.build_layout()

        self.show_dashbo