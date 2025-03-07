# Practiceimport os
from datetime import datetime

# Donation file to store donation data
DONATION_FILE = "donations.txt"

def display_menu():
    """Display the menu options for the user."""
    print("\n--- Donation Management System ---")
    print("1. Add a new donation")
    print("2. View all donations")
    print("3. Delete a donation")
    print("4. Generate Donation Report")
    print("5. Exit")
    choice = input("Enter your choice (1/2/3/4/5): ")
    return choice

def add_donation():
    """Add a new donation."""
    donor_name = input("Enter the donor's name: ")
    try:
        amount = float(input("Enter the donation amount: "))
    except ValueError:
        print("Invalid amount! Please enter a valid number.")
        return
    
    donation_date = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    with open(DONATION_FILE, "a") as file:
        file.write(f"{donor_name},{amount},{donation_date}\n")
    
    print(f"\nThank you, {donor_name}, for your donation of ${amount:.2f}!")

def view_donations():
    """View all donations."""
    print("\n--- View All Donations ---")
    
    if not os.path.exists(DONATION_FILE):
        print("No donations found.")
        return
    
    with open(DONATION_FILE, "r") as file:
        donations = file.readlines()
    
    if not donations:
        print("No donations found.")
    else:
        for donation in donations:
            donor_name, amount, donation_date = donation.strip().split(',')
            print(f"Donor: {donor_name}, Amount: ${float(amount):.2f}, Date: {donation_date}")

def delete_donation():
    """Delete a specific donation."""
    print("\n--- Delete a Donation ---")
    
    if not os.path.exists(DONATION_FILE):
        print("No donations found.")
        return
    
    with open(DONATION_FILE, "r") as file:
        donations = file.readlines()
    
    if not donations:
        print("No donations found.")
        return
    
    # Show donations with their index
    print("Donations available to delete:")
    for index, donation in enumerate(donations, start=1):
        donor_name, amount, donation_date = donation.strip().split(',')
        print(f"{index}. Donor: {donor_name}, Amount: ${float(amount):.2f}, Date: {donation_date}")
    
    try:
        entry_index = int(input("\nEnter the number of the donation you want to delete: "))
        
        if entry_index < 1 or entry_index > len(donations):
            print("Invalid donation number!")
            return
        
        # Remove the donation from the list
        donations.pop(entry_index - 1)
        
        # Write the updated donations back to the file
        with open(DONATION_FILE, "w") as file:
            file.writelines(donations)
        
        print("\nDonation deleted successfully!")
    
    except ValueError:
        print("Invalid input! Please enter a number.")

def generate_report():
    """Generate a donation report."""
    print("\n--- Donation Report ---")
    
    if not os.path.exists(DONATION_FILE):
        print("No donations found.")
        return
    
    with open(DONATION_FILE, "r") as file:
        donations = file.readlines()
    
    if not donations:
        print("No donations found.")
        return
    
    total_donations = 0
    donor_count = len(donations)
    
    for donation in donations:
        _, amount, _ = donation.strip().split(',')
        total_donations += float(amount)
    
    print(f"Total Donations: ${total_donations:.2f}")
    print(f"Number of Donors: {donor_count}")
    print(f"Average Donation: ${total_donations / donor_count:.2f}" if donor_count > 0 else "Average Donation: $0.00")

def main():
    """Main function to run the app."""
    while True:
        choice = display_menu()

        if choice == "1":
            add_donation()
        elif choice == "2":
            view_donations()
        elif choice == "3":
            delete_donation()
        elif choice == "4":
            generate_report()
        elif choice == "5":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
