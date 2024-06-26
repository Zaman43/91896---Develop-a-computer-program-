from tkinter import *
from tkinter import messagebox  # Import messagebox for error handling

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
    
    # Create the column headings
    Label(main_window, font=("Helvetica 10 bold"), text="Row", bg="beige").grid(column=0, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Customer Full Name", bg="beige").grid(column=1, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Receipt Number", bg="beige").grid(column=2, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Item Hired", bg="beige").grid(column=3, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Quantity", bg="beige").grid(column=4, row=7)
    
    # Add each item in the list into its own row
    for index, hire_detail in enumerate(hire_details):
        row_number = index + 8  # Start from row 8 onwards
        Label(main_window, text=index, bg="beige").grid(column=0, row=row_number)
        Label(main_window, text=hire_detail[0], bg="beige").grid(column=1, row=row_number)
        Label(main_window, text=hire_detail[1], bg="beige").grid(column=2, row=row_number)
        Label(main_window, text=hire_detail[2], bg="beige").grid(column=3, row=row_number)
        Label(main_window, text=hire_detail[3], bg="beige").grid(column=4, row=row_number)

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
    
    main_window.configure(bg='beige')  # Set background color to beige
    
    Label(main_window, text="Customer Full Name").grid(column=0, row=0, padx=10, pady=10)
    Label(main_window, text="Receipt Number").grid(column=0, row=1, padx=10, pady=10)
    Button(main_window, text="Quit", command=quit).grid(column=2, row=1, padx=10, pady=10)
    Button(main_window, text="Append Details", command=append_item).grid(column=3, row=1, padx=10, pady=10)
    Button(main_window, text="Print Details", command=print_item_details).grid(column=4, row=1, padx=10, pady=10)
    Label(main_window, text="Item Hired").grid(column=0, row=2, padx=10, pady=10)
    Label(main_window, text="Quantity").grid(column=0, row=3, padx=10, pady=10)
    Label(main_window, text="Row #").grid(column=2, row=3, padx=10, pady=10)
    Button(main_window, text="Delete", command=delete_row).grid(column=4, row=3, padx=10, pady=10)
    
    entry_full_name = Entry(main_window, highlightbackground='beige', highlightcolor='beige')
    entry_full_name.grid(column=1, row=0, padx=10, pady=10)
    
    entry_receipt_number = Entry(main_window, highlightbackground='beige', highlightcolor='beige')
    entry_receipt_number.grid(column=1, row=1, padx=10, pady=10)
    
    entry_item_hired = Entry(main_window, highlightbackground='beige', highlightcolor='beige')
    entry_item_hired.grid(column=1, row=2, padx=10, pady=10)
    
    entry_quantity = Entry(main_window, highlightbackground='beige', highlightcolor='beige')
    entry_quantity.grid(column=1, row=3, padx=10, pady=10)
    
    delete_item = Entry(main_window, highlightbackground='beige', highlightcolor='beige')
    delete_item.grid(column=3, row=3, padx=10, pady=10)

# Start the program running
def main():
    global main_window
    main_window = Tk()
    main_window.configure(bg='beige')  # Set background color to beige
    
    setup_buttons()
    
    main_window.mainloop()  # Start the Tkinter event loop

# Entry point of the program
if __name__ == "__main__":
    main()
