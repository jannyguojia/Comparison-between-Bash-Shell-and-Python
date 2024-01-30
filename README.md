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

