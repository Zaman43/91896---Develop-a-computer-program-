from tkinter import *
from tkinter import messagebox

# Global variables
main_window = None
entry_full_name = None
entry_receipt_number = None
entry_item_hired = None
entry_quantity = None
delete_item = None
hire_details = []
counters = {'total_entries': 0}

# Quit subroutine
def quit():
    main_window.destroy()

# Print details of all the items
def print_item_details():
    # Clear existing labels before printing again
    clear_labels()
    
    # Create the column headings with bold and outlined appearance
    headings = ["Row", "Customer Full Name", "Receipt Number", "Item Hired", "Quantity"]
    colors = ["#ff6f61", "#ff8a65", "#ffab40", "#ffd54f", "#fff176"]
    
    for col, (heading, color) in enumerate(zip(headings, colors)):
        Label(main_window, font=("Helvetica", 12, "bold"), text=heading, bg=color, fg="white", borderwidth=2, relief="solid").grid(column=col, row=7, sticky="nsew")
    
    # Add each item in the list into its own row
    for index, hire_detail in enumerate(hire_details):
        row_number = index + 8  # Start from row 8 onwards
        Label(main_window, text=index, bg="#fce4ec", borderwidth=2, relief="solid").grid(column=0, row=row_number, sticky="nsew")
        for col, detail in enumerate(hire_detail):
            Label(main_window, text=detail, bg="#f8bbd0", borderwidth=2, relief="solid").grid(column=col+1, row=row_number, sticky="nsew")

# Clear labels function
def clear_labels():
    # Clear previous labels in the rows starting from row 8
    for widget in main_window.grid_slaves():
        if int(widget.grid_info()["row"]) >= 8:
            widget.grid_forget()

# Add the next hired item to the list
def append_item():
    global hire_details, counters
    # Check full name is not blank and the quantity is between 1 and 500
    if len(entry_full_name.get()) != 0:
        try:
            quantity = int(entry_quantity.get())
            if 1 <= quantity <= 500:
                # Append each item to its own area of the list
                hire_details.append([entry_full_name.get(), entry_receipt_number.get(), entry_item_hired.get(), str(quantity)])
                # Clear the boxes
                entry_full_name.delete(0, 'end')
                entry_receipt_number.delete(0, 'end')
                entry_item_hired.delete(0, 'end')
                entry_quantity.delete(0, 'end')
                counters['total_entries'] += 1
            else:
                messagebox.showerror("Error", "Quantity must be between 1 and 500.")
        except ValueError:
            messagebox.showerror("Error", "Quantity must be a valid integer.")
    else:
        messagebox.showerror("Error", "Customer Full Name cannot be blank.")

# Delete a row from the list
def delete_row():
    global hire_details, counters
    # Find which row is to be deleted and delete it
    try:
        index_to_delete = int(delete_item.get())
        if 0 <= index_to_delete < counters['total_entries']:
            del hire_details[index_to_delete]
            counters['total_entries'] -= 1
            clear_labels()
            print_item_details()
        else:
            messagebox.showerror("Error", "Row number out of range.")
    except ValueError:
        messagebox.showerror("Error", "Row number must be a valid integer.")
    delete_item.delete(0, 'end')

# Create the buttons and labels
def setup_buttons():
    global main_window, entry_full_name, entry_receipt_number, entry_item_hired, entry_quantity, delete_item
    
    main_window.configure(bg='#fff3e0')  # Set background color to light cream
    
    labels = [
        ("Customer Full Name", 0, 0),
        ("Receipt Number", 0, 1),
        ("Item Hired", 0, 2),
        ("Quantity", 0, 3),
        ("Row #", 2, 3),
    ]
    
    for text, col, row in labels:
        Label(main_window, text=text, bg="#ffb74d", fg="white", font=("Helvetica", 12), borderwidth=2, relief="solid").grid(column=col, row=row, padx=15, pady=15, sticky="nsew")
    
    Button(main_window, text="Quit", command=quit, bg="#d32f2f", fg="white", font=("Helvetica", 12), relief="raised").grid(column=2, row=1, padx=15, pady=15, sticky="nsew")
    Button(main_window, text="Append Details", command=append_item, bg="#f57c00", fg="white", font=("Helvetica", 12), relief="raised").grid(column=3, row=1, padx=15, pady=15, sticky="nsew")
    Button(main_window, text="Print Details", command=print_item_details, bg="#f57c00", fg="white", font=("Helvetica", 12), relief="raised").grid(column=4, row=1, padx=15, pady=15, sticky="nsew")
    Button(main_window, text="Delete", command=delete_row, bg="#d32f2f", fg="white", font=("Helvetica", 12), relief="raised").grid(column=4, row=3, padx=15, pady=15, sticky="nsew")
    
    entry_full_name = Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Helvetica", 12))
    entry_full_name.grid(column=1, row=0, padx=15, pady=15, sticky="nsew")
    
    entry_receipt_number = Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Helvetica", 12))
    entry_receipt_number.grid(column=1, row=1, padx=15, pady=15, sticky="nsew")
    
    entry_item_hired = Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Helvetica", 12))
    entry_item_hired.grid(column=1, row=2, padx=15, pady=15, sticky="nsew")
    
    entry_quantity = Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Helvetica", 12))
    entry_quantity.grid(column=1, row=3, padx=15, pady=15, sticky="nsew")
    
    delete_item = Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Helvetica", 12))
    delete_item.grid(column=3, row=3, padx=15, pady=15, sticky="nsew")

# Start the program running
def main():
    global main_window
    main_window = Tk()
    main_window.title("Julie's Party Hire Store")  # Set window title
    main_window.configure(bg='#fff3e0')  # Set background color to light cream
    main_window.geometry("800x230")  # Set initial window size
    
    setup_buttons()
    
    # Set initial grid row and column weights to make the layout responsive
    for i in range(5):
        main_window.grid_columnconfigure(i, weight=1)
    for i in range(8, 20):  # Adjust this range based on expected number of rows
        main_window.grid_rowconfigure(i, weight=1)
    
    main_window.mainloop()

if __name__ == "__main__":
    main()
