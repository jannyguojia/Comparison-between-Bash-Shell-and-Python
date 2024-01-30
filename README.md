Comparison between Bash Shell and Python
Jia Guo
Different ways of execution:
Bash lacks native access to the operating system-level APIs (also known as C APIs). Therefore, if an automation script requires access to C APIs, programmers must use another programming language to create an executable file for implementation. Python addresses this issue by providing a user-friendly, concise language with access to C APIs, even offering a cross-platform way to access operating system-level APIs.
I can directly run the Python code I write in Visual Studio in the console once it's completed. However, for Shell scripts written in Visual Studio Code, I need to run them through the command-line interface by inputting commands. Moreover, if I transfer a Shell script file written in Windows to Linux, it may fail to run due to the encoding differences between Windows and Linux. To resolve this, while in the vim command mode on Linux, I can run the command ":set ff" to check the format, which may display as "doc". To make it runnable, I need to change it to Unix format by using the command ":set ff=unix". After making this change, the script can be executed normally.

Syntax and Structure Comparison:
In my contact list program, there are five operations: adding contacts, listing contacts, modifying, deleting, and exiting the program.
•	In the Python code, I first define a class named ContactList. Using "def," I define the class's initialization method, methods for handling contact information, and storage paths. Python utilizes if loop statements. 
•	In the contact list code written in Shell Bash, "declare -a contacts" declares an array named contacts to store contact information, where each contact consists of a name, phone, and email separated by ":". Functions are then defined, allowing for if loop statements within the method definition, starting with "if" and ending with "fi."
•	Then, in my Python code, I create an instance of the ContactList class, contact_list, by using “contact_list = ContactList(file_path) ” and passing a file path parameter. Within an infinite loop (while true), a simple menu is displayed for users to perform operations (implemented using if…elif…else). In Python, there's no direct case statement (initially attempted but resulted in system errors); instead, if, elif, and else structures are used to achieve similar multi-branch conditional checks. For comparison with Shell Bash, I provided a basic command-line interface, allowing users to add contacts and view the contact list. Contact information is stored in a text file (contacts.txt).
•	In the Shell Bash "Main Menu," echo outputs statements, and read captures user inputs. Upon user input, the script uses a case statement where ";;" marks the end of each condition block, and "esac" marks the end of the case statement. "done" is used to conclude the loop structure (while true; do). From the user selection perspective, Shell Bash's loop statements appear simpler.

Possible choices for future programming languages:
Bash Shell is highly powerful in system administration, suitable for automation tasks, batch processing, script writing, and file operations. It serves as a command-line Shell, enabling the execution of system commands, managing file systems, and running scripts through the Command-Line Interface (CLI). Bash is also used for scripting to automate specific tasks such as backups, data processing, and periodic jobs.
Python finds extensive applications in Web development. It is also utilized for scripting and automation tasks, similar to Shell scripting. However, Python is more versatile and applicable across a broader range of domains.
There isn't an extreme competition between Bash and Python since they represent two distinct types of programming languages—Bash being a command language and Python being a general-purpose programming language. We can choose either option based on specific requirements or even use both languages simultaneously.


class ContactList:
    def __init__(self, file_path):
        self.file_path = file_path
        self.contacts = self.load_contacts()

    def load_contacts(self):
        try:
            with open(self.file_path, 'r') as file:
                return file.readlines()
        except FileNotFoundError:
            return []

    def save_contacts(self):
        with open(self.file_path, 'w') as file:
            file.writelines(self.contacts)

    def add_contact(self, name, phone, email):
        contact_id = len(self.contacts) + 1
        new_contact = f"ID: {contact_id}, Name: {name}, Phone: {phone}, Email: {email}\n"
        self.contacts.append(new_contact)
        self.save_contacts()

    def list_contacts(self):
        for contact in self.contacts:
            print(contact.strip())  # Remove trailing newline character

    def find_contact_by_id(self, contact_id):
        for contact in self.contacts:
            if contact.startswith(f"ID: {contact_id},"):
                return contact
        return None

    def update_contact(self, contact_id, name, phone, email):
        contact = self.find_contact_by_id(contact_id)
        if contact:
            updated_contact = f"ID: {contact_id}, Name: {name}, Phone: {phone}, Email: {email}\n"
            index = self.contacts.index(contact)
            self.contacts[index] = updated_contact
            self.save_contacts()
            print(f"Contact with ID {contact_id} updated successfully!")
        else:
            print(f"Contact with ID {contact_id} not found.")

    def delete_contact(self, contact_id):
        contact = self.find_contact_by_id(contact_id)
        if contact:
            self.contacts.remove(contact)
            self.save_contacts()
            print(f"Contact with ID {contact_id} deleted successfully!")
        else:
            print(f"Contact with ID {contact_id} not found.")

file_path = 'contacts.txt'

# Create a contact list object
contact_list = ContactList(file_path)

while True:
    print("\n1. Add Contact\n2. List Contacts\n3. Update Contact\n4. Delete Contact\n5. Exit")
    choice = input("Enter your choice: ")

    if choice == '1':
        name = input("Enter name: ")
        phone = input("Enter phone number: ")
        email = input("Enter email: ")
        contact_list.add_contact(name, phone, email)
        print("Contact added successfully!")

    elif choice == '2':
        print("\nListing all contacts:")
        contact_list.list_contacts()

    elif choice == '3':
        contact_id = int(input("Enter contact ID to update: "))
        name = input("Enter new name: ")
        phone = input("Enter new phone number: ")
        email = input("Enter new email: ")
        contact_list.update_contact(contact_id, name, phone, email)

    elif choice == '4':
        contact_id = int(input("Enter contact ID to delete: "))
        contact_list.delete_contact(contact_id)

    elif choice == '5':
        print("Exiting...")
        break

    else:
        print("Invalid choice. Please enter a valid option.")



#!/bin/bash

# Array to store contacts
declare -a contacts

# Function to add a contact
add_contact() {
    echo "Enter name: "
    read name

    echo "Enter phone number: "
    read phone

    echo "Enter email: "
    read email

    # Store the contact details in the array
    contacts+=("$name:$phone:$email")
    echo "Contact added successfully."
}

# Function to list all contacts
list_contacts() {
    echo "List of Contacts:"
    echo "-----------------"

    if [ ${#contacts[@]} -gt 0 ]; then
        for i in "${!contacts[@]}"; do
            contact="${contacts[$i]}"
            IFS=':' read -r -a contact_details <<< "$contact"
            echo "ID: $i | Name: ${contact_details[0]} | Phone: ${contact_details[1]} | Email: ${contact_details[2]}"
        done
    else
        echo "No contacts found."
    fi
}

# Function to find contact by ID
find_contact_by_id() {
    local id=$1
    if [ "$id" -ge 0 ] && [ "$id" -lt ${#contacts[@]} ]; then
        echo "${contacts[$id]}"
    else
        echo "Contact ID $id not found."
    fi
}

# Function to modify contact by ID
modify_contact_by_id() {
    local id=$1
    local contact_info=$2

    if [ "$id" -ge 0 ] && [ "$id" -lt ${#contacts[@]} ]; then
        contacts[$id]="$contact_info"
        echo "Contact with ID $id updated successfully!"
    else
        echo "Contact ID $id not found."
    fi
}

# Function to delete contact by ID
delete_contact_by_id() {
    local id=$1
    if [ "$id" -ge 0 ] && [ "$id" -lt ${#contacts[@]} ]; then
        unset 'contacts[id]'
        contacts=("${contacts[@]}")  # Remove empty elements after deletion
        echo "Contact with ID $id deleted successfully!"
    else
        echo "Contact ID $id not found."
    fi
}

# Main menu
while true; do
    echo "Contact List"
    echo "1. Add Contact"
    echo "2. List Contacts"
    echo "3. Modify Contact by ID"
    echo "4. Delete Contact by ID"
    echo "5. Exit"
    echo "Enter your choice: "
    read choice

    case $choice in
        1)
            add_contact
            ;;
        2)
            list_contacts
            ;;
        3)
            echo "Enter contact ID to modify: "
            read id
            echo "Enter modified contact details (name:phone:email): "
            read modify_contact_info
            modify_contact_by_id "$id" "$modify_contact_info"
            ;;
        4)
            echo "Enter contact ID to delete: "
            read id
            delete_contact_by_id "$id"
            ;;
        5)
            echo "Exiting."
            break
            ;;
        *)
            echo "Invalid option. Please choose again."
            ;;
    esac
done
