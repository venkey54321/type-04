import tkinter as tk
from tkinter import messagebox

# Create a dictionary to store contacts (name as key, details as value)
contacts = {}

# Function to add a new contact
def add_contact():
    name = name_entry.get()
    phone = phone_entry.get()
    email = email_entry.get()
    address = address_entry.get()
    
    if name and phone:
        contacts[name] = {"Phone": phone, "Email": email, "Address": address}
        clear_fields()
        update_contact_list()
    else:
        messagebox.showerror("Error", "Name and Phone are required fields.")

# Function to clear input fields
def clear_fields():
    name_entry.delete(0, tk.END)
    phone_entry.delete(0, tk.END)
    email_entry.delete(0, tk.END)
    address_entry.delete(0, tk.END)

# Function to update the contact list
def update_contact_list():
    contact_list.delete(0, tk.END)
    for name, details in contacts.items():
        contact_list.insert(tk.END, f"{name}: {details['Phone']}")

# Function to search for a contact
def search_contact():
    search_term = search_entry.get()
    if search_term:
        results.delete(0, tk.END)
        for name, details in contacts.items():
            if search_term.lower() in name.lower() or search_term in details['Phone']:
                results.insert(tk.END, f"{name}: {details['Phone']}")
    else:
        messagebox.showerror("Error", "Enter a search term.")

# Function to view contact details
def view_contact():
    selected_item = contact_list.curselection()
    if selected_item:
        name = contact_list.get(selected_item)
        details = contacts.get(name.split(':')[0])
        details_window = tk.Toplevel(root)
        details_window.title("Contact Details")
        for key, value in details.items():
            label = tk.Label(details_window, text=f"{key}: {value}")
            label.pack(padx=10, pady=5)
    else:
        messagebox.showerror("Error", "Select a contact to view details.")

# Function to delete a contact
def delete_contact():
    selected_item = contact_list.curselection()
    if selected_item:
        name = contact_list.get(selected_item)
        name = name.split(':')[0]
        del contacts[name]
        update_contact_list()
    else:
        messagebox.showerror("Error", "Select a contact to delete.")

# Create the main application window
root = tk.Tk()
root.title("Contact Management System")

# Create input fields
name_label = tk.Label(root, text="Name:")
name_label.pack()
name_entry = tk.Entry(root)
name_entry.pack()

phone_label = tk.Label(root, text="Phone:")
phone_label.pack()
phone_entry = tk.Entry(root)
phone_entry.pack()

email_label = tk.Label(root, text="Email:")
email_label.pack()
email_entry = tk.Entry(root)
email_entry.pack()

address_label = tk.Label(root, text="Address:")
address_label.pack()
address_entry = tk.Entry(root)
address_entry.pack()

# Create buttons
add_button = tk.Button(root, text="Add Contact", command=add_contact)
add_button.pack()

view_button = tk.Button(root, text="View Contact", command=view_contact)
view_button.pack()

delete_button = tk.Button(root, text="Delete Contact", command=delete_contact)
delete_button.pack()

# Create a search field and results list
search_label = tk.Label(root, text="Search:")
search_label.pack()
search_entry = tk.Entry(root)
search_entry.pack()

search_button = tk.Button(root, text="Search", command=search_contact)
search_button.pack()

results = tk.Listbox(root)
results.pack()

# Create a list of contacts
contact_list = tk.Listbox(root)
contact_list.pack()

# Start the main loop
root.mainloop()
