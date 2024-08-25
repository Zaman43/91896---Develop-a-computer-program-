from tkinter import * # imports tkinter
from tkinter import messagebox
import random
import os 
from tkinter import ttk #for combobox!

# Global variables
main_window = None
entry_full_name = None
entry_item_hired = None
entry_quantity = None
delete_item = None
hire_details = []
counters = {'total_entries': 0}

def validate_name(name):
    return all(char.isalpha() or char.isspace() for char in name)


def validate_item(item):
    return all(char.isalpha() or char.isspace() for char in item)

# Quit subroutine
def quit():
    main_window.destroy()

# Print details of all the items and save to file
def print_item_details():
    clear_labels()
    
    # headings will be bold and colored
    headings = ["Row", "Receipt Number", "Customer Full Name", "Item Hired", "Quantity"]
    colors = ["#ff6f61", "#ff8a65", "#ffab40", "#ffd54f", "#f6e979"]
    
    for col, (heading, color) in enumerate(zip(headings, colors)): 
        Label(main_window, font=("Times New Roman", 10, "bold"), text=heading, bg=color, fg="black", borderwidth=2, relief="solid", anchor="center").grid(column=col, row=7, sticky="nsew")
    
    # Add each item in the list into its own row
    for index, hire_detail in enumerate(hire_details):
        row_number = index + 8  
        Label(main_window, text=index, bg="#fce4ec", borderwidth=2, relief="solid", anchor="center").grid(column=0, row=row_number, sticky="nsew")
        for col, detail in enumerate(hire_detail):
            Label(main_window, text=detail, bg="#f8bbd0", borderwidth=2, relief="solid", anchor="center").grid(column=col+1, row=row_number, sticky="nsew")
    


    # Save the details to a file
    if hire_details:
        with open("hire_details.txt", "a") as file:
            file.write(f"{'Receipt Number':<15} {'Customer Full Name':<30} {'Item Hired':<20} {'Quantity':<10}\n") # headings in the actaul saved data file
            file.write("="*75 + "\n")
            for detail in hire_details:
                file.write(f"{detail[0]:<15} {detail[1]:<30} {detail[2]:<20} {detail[3]:<10}\n")
        messagebox.showinfo("Success!", "Customer details have been saved successfully!")
    else:
        messagebox.showwarning("There are no details to save!")

# Clear labels 
def clear_labels():
    for widget in main_window.grid_slaves():
        if int(widget.grid_info()["row"]) >= 8:
            widget.grid_forget()

# Add next hired item to the list
def append_item():
    global hire_details, counters
    if len(entry_full_name.get()) != 0: #check if name is entered
        try:

            
            

            if not validate_name(entry_full_name.get()):
             messagebox.showerror("Invalid Entry", "Customer name must not have numbers or symbols") #validate name

            item_hired = entry_item_hired.get()

            if not validate_item(item_hired):  #check to see if it is only letters no integers or symbols
                messagebox.showerror("Invalid Entry", "Item name cannot have any numbers or symbols.")
                return  #exit the function 


            if not item_hired: #checks if the item is empty
                messagebox.showerror("Invalid Entry", "item hired cannot be left blank")
                return #exit function

            quantity = int(entry_quantity.get())
            if 1 <= quantity <= 500:
                receipt_number = random.randint(100000, 999999)  # Generate a random receipt number
                hire_details.append([str(receipt_number), entry_full_name.get(), entry_item_hired.get(), str(quantity)])
                entry_full_name.delete(0, 'end')
                entry_item_hired.delete(0, 'end')
                entry_quantity.delete(0, 'end') 
                counters['total_entries'] += 1

            else:
                messagebox.showerror("Invalid Entry", "Quantity must be between 1 and 500.")
        except ValueError:
            messagebox.showerror("Invalid Entry", "Only integers are accepted for quantity, cannot be left blank.")
    else:
        messagebox.showerror("Invalid", "The customer's full name cannot be empty.")

# Delete a row from the list
def delete_row():
    global hire_details, counters
    try:
        index_to_delete = int(delete_item.get()) #make the input a integer
        if 0 <= index_to_delete < counters['total_entries']: #check if it is in valid range
            del hire_details[index_to_delete] #delete a specific row
            counters['total_entries'] -= 1
            clear_labels() #clears the labels
            print_item_details()#print the updated list
        else:
            messagebox.showerror("Error", "Row number out of range.")
    except ValueError:
        messagebox.showerror("Error", "Row number must be a valid integer.")
    delete_item.delete(0, 'end') #clear the entry box 

# Create the buttons and labels
def setup_buttons():
    global main_window, entry_full_name, entry_item_hired, entry_quantity, delete_item

    #Label geometry on the grid
    labels = [
        ("Customer Full Name", 0, 0),
        ("Item Hired", 0, 1),
        ("Quantity", 0, 2),
        ("Row #", 2, 2)
    ]
    
    for text, col, row in labels:

        #adds the labels and their styles
        Label(main_window, text=text, font=('Helvetica', 11),fg="black", bg="#ffb74d", relief="raised", anchor="center", borderwidth=1).grid(column=col, row=row, padx=12, pady= 12, sticky="nsew")

    #makes the buttons in the gui adding them using the grid and also how it looks
    Button(main_window, text="Quit", command=quit, font=('Times New Roman', 11), relief="raised", fg="black", bg="#d32f2f").grid(column=2, padx=11, pady=11, row=0, sticky="nsew") 
    Button(main_window, text="Submit", command=append_item, font=('Times New Roman', 11), relief="raised", fg="black", bg="#f57c00").grid(column=3, padx=11, pady=11, row=0, sticky="nsew")
    Button(main_window, text="Print Details", command=print_item_details, font=('Times New Roman', 11), relief="raised", fg="black", bg="#f57c00").grid(column=4, padx=11, pady=11, row=0, sticky="nsew")
    Button(main_window, text="Delete", command=delete_row, font=('Times New Roman', 11), relief="raised", fg="black", bg="#d32f2f").grid(column=4, padx=11, pady=11, row=2, sticky="nsew")

    
    #this is the creation of the entry widgets for fullname,item hired,quantity, and delete
    entry_full_name = Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Verdana", 10))
    entry_full_name.grid(column=1, row=0, padx=10, pady=10,)
 
    item = ["Party Hat","Candy","Glow Stick","Toy"] #list of the items
    entry_item_hired = ttk.Combobox(main_window, values=item, font=('Helvetica', 11))
    entry_item_hired.grid(column=1, row=1, padx=10, pady=10)


    Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Helvetica", 10))
    entry_item_hired.grid(column=1, row=1, padx=10, pady=10,)
    
    entry_quantity = Entry(main_window, highlightbackground='#fff3e0', highlightcolor='#fff3e0', font=("Helvetica", 10))
    entry_quantity.grid(column=1, row=2, padx=10, pady=10,)
   
    delete_item = Entry(main_window, highlightbackground='#d32f2f', highlightcolor='#d32f2f', font=("Helvetica", 10))
    delete_item.grid(column=3, row=2, padx=10, pady=10,)

# Start the program running
def main():
    global main_window
    main_window = Tk()
    main_window.title("Julie's Party Hire Store")#names the main window
    main_window.configure(bg='#fff3e0')#gives BG colour to main window
    main_window.geometry("800x230")#the size of the main window
    
    setup_buttons() #this will setup the buttons and functions in this program/gui

    for i in range(5):
        main_window.grid_columnconfigure(i, weight=2)
    for i in range(8, 20):
        main_window.grid_rowconfigure(i, weight=2)

    main_window.mainloop()

if __name__ == "__main__":
    main()
