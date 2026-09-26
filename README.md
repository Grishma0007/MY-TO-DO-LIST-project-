# MY-TO-DO-LIST-project-
import tkinter as tk
from tkinter import messagebox


# -----------------------------
# Functions
# -----------------------------

def add_task():
    task = task_entry.get().strip()

    if task == "":
        messagebox.showwarning("Warning", "Please enter a task!")
        return

    task_listbox.insert(tk.END, task)
    task_entry.delete(0, tk.END)


def delete_task():
    selected = task_listbox.curselection()

    if not selected:
        messagebox.showwarning("Warning", "Please select a task!")
        return

    task_listbox.delete(selected[0])


def complete_task():
    selected = task_listbox.curselection()

    if not selected:
        messagebox.showwarning("Warning", "Please select a task!")
        return

    index = selected[0]
    task = task_listbox.get(index)

    if not task.startswith("✓ "):
        task_listbox.delete(index)
        task_listbox.insert(index, "✓ " + task)


def clear_tasks():
    if task_listbox.size() == 0:
        return

    result = messagebox.askyesno(
        "Clear All",
        "Are you sure you want to delete all tasks?"
    )

    if result:
        task_listbox.delete(0, tk.END)


def save_tasks():
    try:
        with open("tasks.txt", "w") as file:
            for task in task_listbox.get(0, tk.END):
                file.write(task + "\n")

        messagebox.showinfo("Success", "Tasks saved successfully!")

    except Exception as e:
        messagebox.showerror("Error", str(e))


def load_tasks():
    try:
        with open("tasks.txt", "r") as file:
            tasks = file.readlines()

        for task in tasks:
            task = task.strip()

            if task:
                task_listbox.insert(tk.END, task)

    except FileNotFoundError:
        pass


# -----------------------------
# Main Window
# -----------------------------

root = tk.Tk()
root.title("To-Do List")
root.geometry("500x550")
root.resizable(False, False)


# -----------------------------
# Title
# -----------------------------

title_label = tk.Label(
    root,
    text="MY TO-DO LIST",
    font=("Arial", 22, "bold")
)

title_label.pack(pady=20)


# -----------------------------
# Entry Box
# -----------------------------

task_entry = tk.Entry(
    root,
    font=("Arial", 14),
    width=32
)

task_entry.pack(pady=10)


# -----------------------------
# Add Button
# -----------------------------

add_button = tk.Button(
    root,
    text="Add Task",
    font=("Arial", 12, "bold"),
    width=15,
    command=add_task
)

add_button.pack(pady=5)


# -----------------------------
# Listbox
# -----------------------------

task_listbox = tk.Listbox(
    root,
    font=("Arial", 14),
    width=38,
    height=12
)

task_listbox.pack(pady=15)


# -----------------------------
# Buttons
# -----------------------------

complete_button = tk.Button(
    root,
    text="✓ Complete",
    font=("Arial", 11),
    width=12,
    command=complete_task
)

complete_button.pack(pady=4)


delete_button = tk.Button(
    root,
    text="Delete",
    font=("Arial", 11),
    width=12,
    command=delete_task
)

delete_button.pack(pady=4)


clear_button = tk.Button(
    root,
    text="Clear All",
    font=("Arial", 11),
    width=12,
    command=clear_tasks
)

clear_button.pack(pady=4)


save_button = tk.Button(
    root,
    text="Save Tasks",
    font=("Arial", 11),
    width=12,
    command=save_tasks
)

save_button.pack(pady=4)


# -----------------------------
# Load Saved Tasks
# -----------------------------

load_tasks()


# -----------------------------
# Run Application
# -----------------------------

root.mainloop()