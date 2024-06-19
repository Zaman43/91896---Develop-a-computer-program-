# 91896---Develop-a-computer-program-
This project will look at developing a computer program that solves a certain problem.

from tkinter import *

# Quit subroutine
def quit():
    main_window.destroy()

# Print details of all the items
def print_item_details():
    name_count = 0
    # Create the column headings
    Label(main_window, font=("Helvetica 10 bold"), text="Row").grid(column=0, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Customer Full Name").grid(column=1, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Receipt Number").grid(column=2, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Item Hired").grid(column=3, row=7)
    Label(main_window, font=("Helvetica 10 bold"), text="Quantity").grid(column=4, row=7)
    # Add each item in the list into its own row
    while name_count < counters['total_entries']:
        Label(main_window, text=name_count).grid(column=0, row=name_count + 8)
        Label(main_window, text=(hire_details[name_count][0])).grid(column=1, row=name_count + 8)
        Label(main_window, text=(hire_details[name_count][1])).grid(column=2, row=name_count + 8)
        Label(main_window, text=(hire_details[name_count][2])).grid(column=3, row=name_count + 8)
        Label(main_window, text=(hire_details[name_count][3])).grid(column=4, row=name_count + 8)
        name_count += 1
        counters['name_count'] = name_count

# Add the next hired item to the list
def append_item():
    # Check full name is not blank and the quantity is between 1 and 500
    if len(entry_full_name.get()) != 0 and 1 <= int(entry_quantity.get()) <= 500:
        # Append each item to its own area of the list
        hire_details.append([entry_full_name.get(), entry_receipt_number.get(), entry_item_hired.get(), entry_quantity.get()])
        # Clear the boxes
        entry_full_name.delete(0, 'end')
        entry_receipt_number.delete(0, 'end')
        entry_item_hired.delete(0, 'end')
        entry_quantity.delete(0, 'end')
        counters['total_entries'] += 1

# Delete a row from the list
def delete_row():
    # Find which row is to be deleted and delete it
    del hire_details[int(delete_item.get())]
    counters['total_entries'] -= 1
    name_count = counters['name_count']
    delete_item.delete(0, 'end')
    # Clear the last item displayed on the GUI
    Label(main_window, text="       ").grid(column=0, row=name_count + 7)
    Label(main_window, text="       ").grid(column=1, row=name_count + 7)
    Label(main_window, text="       ").grid(column=2, row=name_count + 7)
    Label(main_window, text="       ").grid(column=3, row=name_count + 7)
    Label(main_window, text="       ").grid(column=4, row=name_count + 7)
    # Print all the items in the list
    print_item_details()

# Create the buttons and labels
def setup_buttons():
    Label(main_window, text="Customer Full Name").grid(column=0, row=0)
    Label(main_window, text="Receipt Number").grid(column=0, row=1)
    Button(main_window, text="Quit", command=quit).grid(column=2, row=1)
    Button(main_window, text="Append Details", command=append_item).grid(column=3, row=1)
    Button(main_window, text="Print Details", command=print_item_details).grid(column=4, row=1)
    Label(main_window, text="Item Hired").grid(column=0, row=2)
    Label(main_window, text="Quantity").grid(column=0, row=3)
    Label(main_window, text="Row #").grid(column=2, row=3)
    Button(main_window, text="Delete", command=delete_row).grid(column=4, row=3)

# Start the program running
def main():
    setup_buttons()
    main_window.mainloop()

counters = {'total_entries': 0, 'name_count': 0}
hire_details = []
main_window = Tk()
entry_full_name = Entry(main_window)
entry_full_name.grid(column=1, row=0)
entry_receipt_number = Entry(main_window)
entry_receipt_number.grid(column=1, row=1)
entry_item_hired = Entry(main_window)
entry_item_hired.grid(column=1, row=2)
entry_quantity = Entry(main_window)
entry_quantity.grid(column=1, row=3)
delete_item = Entry(main_window)
delete_item.grid(column=3, row=3)
main()
