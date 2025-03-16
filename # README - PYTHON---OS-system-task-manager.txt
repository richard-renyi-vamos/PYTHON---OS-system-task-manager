CHATGPT CONVERSATION

**************

create an OS SYSTEM TASK MANAGER app in PYTHON
ChatGPT said:
Here's a simple OS System Task Manager built in Python using the tkinter library for the GUI and the psutil library for system monitoring. This app will allow you to:

✅ View running processes
✅ See CPU and memory usage
✅ Kill processes by PID

Let's get started! 🚀

Os Task Manager

Answer in chat instead
Your OS System Task Manager app is ready! 🎉 This Python program provides a GUI to monitor and manage system processes using psutil and tkinter. Let me know if you want any enhancements! 🚀💻


**************

import tkinter as tk
from tkinter import ttk, messagebox
import psutil

def update_process_list():
    for row in tree.get_children():
        tree.delete(row)
    
    for process in psutil.process_iter(['pid', 'name', 'cpu_percent', 'memory_percent']):
        pid = process.info['pid']
        name = process.info['name']
        cpu = process.info['cpu_percent']
        memory = process.info['memory_percent']
        tree.insert("", "end", values=(pid, name, f"{cpu}%", f"{memory:.2f}%"))

def kill_process():
    selected_item = tree.selection()
    if not selected_item:
        messagebox.showwarning("Warning", "Please select a process to kill.")
        return
    
    pid = tree.item(selected_item)['values'][0]
    try:
        psutil.Process(pid).terminate()
        update_process_list()
        messagebox.showinfo("Success", f"Process {pid} terminated successfully.")
    except Exception as e:
        messagebox.showerror("Error", f"Failed to terminate process: {e}")

# GUI setup
root = tk.Tk()
root.title("OS System Task Manager")
root.geometry("600x400")

tree = ttk.Treeview(root, columns=("PID", "Name", "CPU %", "Memory %"), show='headings')
tree.heading("PID", text="PID")
tree.heading("Name", text="Name")
tree.heading("CPU %", text="CPU %")
tree.heading("Memory %", text="Memory %")
tree.pack(fill=tk.BOTH, expand=True)

btn_frame = tk.Frame(root)
btn_frame.pack(fill=tk.X)

refresh_btn = tk.Button(btn_frame, text="Refresh", command=update_process_list)
refresh_btn.pack(side=tk.LEFT, padx=10, pady=5)

kill_btn = tk.Button(btn_frame, text="Kill Process", command=kill_process)
kill_btn.pack(side=tk.RIGHT, padx=10, pady=5)

update_process_list()
root.mainloop()
