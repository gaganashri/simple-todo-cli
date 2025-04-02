# simple-todo-cli
# A simple command-line to-do list application using Python and JSON for task management
# To-Do List Application
# Author: [Your Name]
# Description: A simple command-line to-do list application using JSON for persistence.

import json

def load_tasks(filename="tasks.json"):
    """Load tasks from a JSON file. Returns a list of tasks."""
    try:
        with open(filename, "r") as file:
            return json.load(file)
    except (FileNotFoundError, json.JSONDecodeError):
        return []

def save_tasks(tasks, filename="tasks.json"):
    """Save tasks to a JSON file."""
    with open(filename, "w") as file:
        json.dump(tasks, file, indent=4)

def add_task(tasks, task):
    """Add a new task to the list."""
    tasks.append({"task": task, "done": False})
    save_tasks(tasks)
    print(f"Task '{task}' added.")

def remove_task(tasks, index):
    """Remove a task from the list by index."""
    if 0 <= index < len(tasks):
        removed_task = tasks.pop(index)
        save_tasks(tasks)
        print(f"Task '{removed_task['task']}' removed.")
    else:
        print("Invalid index!")

def complete_task(tasks, index):
    """Mark a task as completed by index."""
    if 0 <= index < len(tasks):
        tasks[index]["done"] = True
        save_tasks(tasks)
        print(f"Task '{tasks[index]['task']}' marked as done.")
    else:
        print("Invalid index!")

def view_tasks(tasks):
    """Display the list of tasks with their status."""
    if not tasks:
        print("No tasks available.")
        return
    for i, task in enumerate(tasks):
        status = "[✓]" if task["done"] else "[ ]"
        print(f"{i}. {status} {task['task']}")

def main():
    """Main function to handle user input and interactions."""
    tasks = load_tasks()
    while True:
        print("\nTo-Do List:")
        view_tasks(tasks)
        print("\nOptions: add, remove, complete, exit")
        choice = input("Enter command: ").strip().lower()
        
        if choice == "add":
            task = input("Enter task: ")
            add_task(tasks, task)
        elif choice == "remove":
            try:
                index = int(input("Enter task index to remove: "))
                remove_task(tasks, index)
            except ValueError:
                print("Please enter a valid number.")
        elif choice == "complete":
            try:
                index = int(input("Enter task index to mark as done: "))
                complete_task(tasks, index)
            except ValueError:
                print("Please enter a valid number.")
        elif choice == "exit":
            print("Exiting the to-do list application.")
            break
        else:
            print("Invalid option, try again.")

if __name__ == "__main__":
    main()

