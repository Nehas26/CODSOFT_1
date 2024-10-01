# CODSOFT_1
import tkinter as tk
from tkinter import messagebox, simpledialog
import json
import os

# File to store tasks
TASKS_FILE = "tasks.json"

# Load tasks from JSON file
def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, "r") as f:
            return json.load(f)
    return []

# Save tasks to JSON file
def save_tasks(tasks):
    with open(TASKS_FILE, "w") as f:
        json.dump(tasks, f, indent=4)

# Add a task
def add_task():
    task_title = simpledialog.askstring("Add Task", "Enter the task:")
    if task_title:
        tasks.append({"title": task_title, "completed": False})
        update_task_list()
        save_tasks(tasks)

# Update a selected task
def update_task():
    selected_task_index = task_listbox.curselection()
    if not selected_task_index:
        messagebox.showwarning("Update Task", "Please select a task to update.")
        return
    task_index = selected_task_index[0]
    new_title = simpledialog.askstring("Update Task", "Enter the new title:")
    if new_title:
        tasks[task_index]["title"] = new_title
        update_task_list()
        save_tasks(tasks)

# Mark a task as completed
def mark_completed():
    selected_task_index = task_listbox.curselection()
    if not selected_task_index:
        messagebox.showwarning("Mark Completed", "Please select a task to mark as completed.")
        return
    task_index = selected_task_index[0]
    tasks[task_index]["completed"] = True
    update_task_list()
    save_tasks(tasks)

# Delete a selected task
def delete_task():
    selected_task_index = task_listbox.curselection()
    if not selected_task_index:
        messagebox.showwarning("Delete Task", "Please select a task to delete.")
        return
    task_index = selected_task_index[0]
    del tasks[task_index]
    update_task_list()
    save_tasks(tasks)

# Update the task list display
def update_task_list():
    task_listbox.delete(0, tk.END)
    for task in tasks:
        task_title = task["title"]
        if task["completed"]:
            task_title += " (Completed)"
        task_listbox.insert(tk.END, task_title)

# Initialize the Tkinter window
root = tk.Tk()
root.title("To-Do List")

# Load the task list
tasks = load_tasks()

# Create the task listbox
task_listbox = tk.Listbox(root, width=50, height=10)
task_listbox.pack(pady=20)

# Update task list on startup
update_task_list()

# Buttons for task operations
button_frame = tk.Frame(root)
button_frame.pack(pady=10)

add_button = tk.Button(button_frame, text="Add Task", width=15, command=add_task)
add_button.grid(row=0, column=0, padx=5)

update_button = tk.Button(button_frame, text="Update Task", width=15, command=update_task)
update_button.grid(row=0, column=1, padx=5)

complete_button = tk.Button(button_frame, text="Mark Completed", width=15, command=mark_completed)
complete_button.grid(row=0, column=2, padx=5)

delete_button = tk.Button(button_frame, text="Delete Task", width=15, command=delete_task)
delete_button.grid(row=0, column=3, padx=5)

# Start the main loop
root.mainloop()
