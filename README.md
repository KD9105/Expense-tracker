# Expense-tracker
A simple expense tracker application to manage and track personal enpenses.
## Features
import csv
import os
from datetime import datetime

FILE_NAME = "expenses.csv"


def initialize_file():
    """Create the CSV file if it doesn't exist."""
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, "w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Category", "Description", "Amount"])


def add_expense():
    """Add a new expense."""
    category = input("Enter category (Food/Travel/Shopping/Bills/Other): ")
    description = input("Enter description: ")

    try:
        amount = float(input("Enter amount: ₹"))
        if amount <= 0:
            print("Amount must be greater than 0.")
            return
    except ValueError:
        print("Please enter a valid amount.")
        return

    date = datetime.now().strftime("%Y-%m-%d")

    with open(FILE_NAME, "a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([date, category, description, amount])

    print("Expense added successfully!")


def view_expenses():
    """Display all recorded expenses."""
    with open(FILE_NAME, "r") as file:
        reader = csv.DictReader(file)
        expenses = list(reader)

    if not expenses:
        print("\nNo expenses found.")
        return

    print("\n" + "=" * 65)
    print(f"{'Date':<12}{'Category':<15}{'Description':<20}{'Amount':>10}")
    print("=" * 65)

    for expense in expenses:
        print(
            f"{expense['Date']:<12}"
            f"{expense['Category']:<15}"
            f"{expense['Description']:<20}"
            f"₹{float(expense['Amount']):>9.2f}"
        )

    print("=" * 65)


def total_expenses():
    """Calculate and display total expenses."""
    total = 0

    with open(FILE_NAME, "r") as file:
        reader = csv.DictReader(file)

        for expense in reader:
            total += float(expense["Amount"])

    print(f"\nTotal Expenses: ₹{total:.2f}")


def category_summary():
    """Show expenses grouped by category."""
    summary = {}

    with open(FILE_NAME, "r") as file:
        reader = csv.DictReader(file)

        for expense in reader:
            category = expense["Category"]
            amount = float(expense["Amount"])

            summary[category] = summary.get(category, 0) + amount

    if not summary:
        print("\nNo expenses found.")
        return

    print("\n===== Category Summary =====")

    for category, amount in summary.items():
        print(f"{category:<15} ₹{amount:.2f}")


def delete_expenses():
    """Delete all expenses after confirmation."""
    confirmation = input(
        "Are you sure you want to delete ALL expenses? (yes/no): "
    ).lower()

    if confirmation != "yes":
        print("Delete operation cancelled.")
        return

    with open(FILE_NAME, "w", newline="") as file:
        writer = csv.writer(file)
        writer.writerow(["Date", "Category", "Description", "Amount"])

    print("All expenses have been deleted.")


def main():
    initialize_file()

    while True:
        print("\n")
        print("========== EXPENSE TRACKER ==========")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Show Total Expenses")
        print("4. Category Summary")
        print("5. Delete All Expenses")
        print("6. Exit")
        print("=====================================")

        choice = input("Enter your choice (1-6): ")

        if choice == "1":
            add_expense()

        elif choice == "2":
            view_expenses()

        elif choice == "3":
            total_expenses()

        elif choice == "4":
            category_summary()

        elif choice == "5":
            delete_expenses()

        elif choice == "6":
            print("Thank you for using Expense Tracker!")
            break

        else:
            print("Invalid choice. Please select 1-6.")


if __name__ == "__main__":
    main()
