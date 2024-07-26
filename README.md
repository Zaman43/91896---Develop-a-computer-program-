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
    Label(main_window, font=("Helvetica 10 bold"), text="Row", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=0, row=7, sticky="nsew")
    Label(main_window, font=("Helvetica 10 bold"), text="Customer Full Name", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=1, row=7, sticky="nsew")
    Label(main_window, font=("Helvetica 10 bold"), text="Receipt Number", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=2, row=7, sticky="nsew")
    Label(main_window, font=("Helvetica 10 bold"), text="Item Hired", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=3, row=7, sticky="nsew")
    Label(main_window, font=("Helvetica 10 bold"), text="Quantity", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=4, row=7, sticky="nsew")
    
    # Add each item in the list into its own row
    for index, hire_detail in enumerate(hire_details):
        row_number = index + 8  # Start from row 8 onwards
        Label(main_window, text=index, bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=0, row=row_number, sticky="nsew")
        Label(main_window, text=hire_detail[0], bg="light gray", borderwidth=1, relief="solid").grid(column=1, row=row_number, sticky="nsew")
        Label(main_window, text=hire_detail[1], bg="light gray", borderwidth=1, relief="solid").grid(column=2, row=row_number, sticky="nsew")
        Label(main_window, text=hire_detail[2], bg="light gray", borderwidth=1, relief="solid").grid(column=3, row=row_number, sticky="nsew")
        Label(main_window, text=hire_detail[3], bg="light gray", borderwidth=1, relief="solid").grid(column=4, row=row_number, sticky="nsew")

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
    
    main_window.configure(bg='light blue')  # Set background color to light blue
    
    Label(main_window, text="Customer Full Name", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=0, row=0, padx=10, pady=10, sticky="nsew")
    Label(main_window, text="Receipt Number", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=0, row=1, padx=10, pady=10, sticky="nsew")
    Button(main_window, text="Quit", command=quit, bg="#4f7cac", fg="white").grid(column=2, row=1, padx=10, pady=10, sticky="nsew")
    Button(main_window, text="Append Details", command=append_item, bg="#4f7cac", fg="white").grid(column=3, row=1, padx=10, pady=10, sticky="nsew")
    Button(main_window, text="Print Details", command=print_item_details, bg="#4f7cac", fg="white").grid(column=4, row=1, padx=10, pady=10, sticky="nsew")
    Label(main_window, text="Item Hired", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=0, row=2, padx=10, pady=10, sticky="nsew")
    Label(main_window, text="Quantity", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=0, row=3, padx=10, pady=10, sticky="nsew")
    Label(main_window, text="Row #", bg="#2e6f9e", fg="white", borderwidth=1, relief="solid").grid(column=2, row=3, padx=10, pady=10, sticky="nsew")
    Button(main_window, text="Delete", command=delete_row, bg="#4f7cac", fg="white").grid(column=4, row=3, padx=10, pady=10, sticky="nsew")
    
    entry_full_name = Entry(main_window, highlightbackground='light blue', highlightcolor='light blue')
    entry_full_name.grid(column=1, row=0, padx=10, pady=10, sticky="nsew")
    
    entry_receipt_number = Entry(main_window, highlightbackground='light blue', highlightcolor='light blue')
    entry_receipt_number.grid(column=1, row=1, padx=10, pady=10, sticky="nsew")
    
    entry_item_hired = Entry(main_window, highlightbackground='light blue', highlightcolor='light blue')
    entry_item_hired.grid(column=1, row=2, padx=10, pady=10, sticky="nsew")
    
    entry_quantity = Entry(main_window, highlightbackground='light blue', highlightcolor='light blue')
    entry_quantity.grid(column=1, row=3, padx=10, pady=10, sticky="nsew")
    
    delete_item = Entry(main_window, highlightbackground='light blue', highlightcolor='light blue')
    delete_item.grid(column=3, row=3, padx=10, pady=10, sticky="nsew")

# Start the program running
def main():
    global main_window
    main_window = Tk()
    main_window.title("Julie's Party Hire Store")  # Set window title
    main_window.configure(bg='light blue')  # Set background color to light blue
    
    setup_buttons()
    
    main_window.mainloop()  # Start the Tkinter event loop

# Entry point of the program
if __name__ == "__main__":
    main()
