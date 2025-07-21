# Customer-Service-Manager
Features: Add customer details  ,Add service records for customers , View all customers , View all services for a customer
import json
import os

DATA_FILE = "data.json"

# Load existing data
def load_data():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, 'r') as f:
            return json.load(f)
    return {}

# Save data to file
def save_data(data):
    with open(DATA_FILE, 'w') as f:
        json.dump(data, f, indent=4)

# Add a new customer
def add_customer(data):
    cust_id = input("Enter Customer ID: ")
    if cust_id in data:
        print("Customer ID already exists.")
        return
    name = input("Enter Customer Name: ")
    phone = input("Enter Phone Number: ")
    data[cust_id] = {
        "name": name,
        "phone": phone,
        "services": []
    }
    print("Customer added!")

# Add a service for an existing customer
def add_service(data):
    cust_id = input("Enter Customer ID: ")
    if cust_id not in data:
        print("Customer not found.")
        return
    service = input("Enter Service Description: ")
    price = input("Enter Service Price: ₹ ")
    data[cust_id]["services"].append({
        "service": service,
        "price": price
    })
    print("Service added!")

# View all customers
def view_customers(data):
    if not data:
        print("No customers found.")
        return
    print("\n--- All Customers ---")
    for cid, info in data.items():
        print(f"ID: {cid}, Name: {info['name']}, Phone: {info['phone']}")

# View services of a customer
def view_services(data):
    cust_id = input("Enter Customer ID: ")
    if cust_id not in data:
        print("Customer not found.")
        return
    print(f"\n--- Services for {data[cust_id]['name']} ---")
    for idx, serv in enumerate(data[cust_id]["services"], 1):
        print(f"{idx}. {serv['service']} - ₹{serv['price']}")

# Main menu
def main():
    data = load_data()
    while True:
        print("\n📋 Customer & Service App Menu")
        print("1. Add Customer")
        print("2. Add Service")
        print("3. View All Customers")
        print("4. View Services for a Customer")
        print("5. Exit")

        choice = input("Enter choice: ")

        if choice == '1':
            add_customer(data)
        elif choice == '2':
            add_service(data)
        elif choice == '3':
            view_customers(data)
        elif choice == '4':
            view_services(data)
        elif choice == '5':
            save_data(data)
            print("Data saved. Exiting...")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
