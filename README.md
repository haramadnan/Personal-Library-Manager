📚 Personal Library Manager

A simple command-line application for managing your personal book collection. This program allows users to add, remove, search, and track books, along with viewing reading statistics.

🔧 Features

Add books with details (title, author, year, genre, read/unread status).

Remove books by title.

Search books by title or author.

Display all books in the library.

View statistics on total books and reading progress.

Saves data using JSON for persistence.



---

🛠 Implementation Steps

1. Setup and Data Handling

Imported required modules (json for data storage and os to check file existence).

Defined a data_file (library.txt) to store books.


import json
import os

data_file = "library.txt"

Implemented load_library() to read data from the file and save_library() to store book data persistently.


def load_library():
    if os.path.exists(data_file):
        with open(data_file, "r") as file:
            return json.load(file)
    return []

def save_library(library):
    with open(data_file, "w") as file:
        json.dump(library, file)


---

2. Adding a Book

Function add_book(library) prompts the user to enter book details and appends them to the library.

Saves the updated list.


def add_book(library):
    title = input("Enter the title of the book: ")
    author = input("Enter the author of the book: ")
    year = input("Enter the year of the book: ")
    genre = input("Enter the genre of the book: ")
    read = input("Have you read the book? (yes/no): ").lower() == "yes"

    new_book = {
        'title': title,
        'author': author,
        'year': year,
        'genre': genre,
        'read': read,
    }

    library.append(new_book)
    save_library(library)
    print(f"Book '{title}' added successfully.")


---

3. Removing a Book

remove_book(library) checks if a book exists and removes it by filtering the list.

Saves updated data if the book is found.


def remove_book(library):
    title = input("Enter the title of the book to remove: ").lower()
    initial_length = len(library)
    library = [book for book in library if book['title'].lower() != title]
    
    if len(library) < initial_length:
        save_library(library)
        print(f"Book '{title}' removed successfully.")
    else:
        print(f"Book '{title}' not found in the library.")


---

4. Searching for a Book

search_library(library) allows users to search by title or author.

Displays all matching results.


def search_library(library):
    search_by = input("Search by title or author: ").lower()
    search_term = input(f"Enter the {search_by}: ").lower()

    results = [book for book in library if search_term in book[search_by].lower()]
    
    if results:
        for book in results:
            status = "Read" if book['read'] else "Unread"
            print(f"{book['title']} by {book['author']} - {book['year']} - {book['genre']} - {status}")
    else:
        print(f"No books found matching '{search_term}' in the {search_by} field.")


---

5. Displaying Books and Statistics

display_all_books(library): Lists all books with their details.

display_statistics(library): Calculates and displays reading progress.


def display_statistics(library):
    total_books = len(library)
    read_books = len([book for book in library if book['read']])
    percentage_read = (read_books / total_books) * 100 if total_books > 0 else 0

    print(f"Total books: {total_books}")
    print(f"Percentage read: {percentage_read:.2f}%")


---

6. Main Menu & User Interaction

main() provides an interactive menu.

Calls appropriate functions based on user input.


def main():
    library = load_library()
    while True:
        print("\nWelcome to Your Personal Library Manager!")
        print("1. Add a book")
        print("2. Remove a book")
        print("3. Search for a book")
        print("4. Display all books")
        print("5. Display statistics")
        print("6. Exit")

        choice = input("Enter your choice: ")
        if choice == '1':
            add_book(library)
        elif choice == '2':
            remove_book(library)
        elif choice == '3':
            search_library(library)
        elif choice == '4':
            display_all_books(library)
        elif choice == '5':
            display_statistics(library)
        elif choice == '6':
            print("Exiting Personal Library Manager!")
            break
        else:
            print("Invalid choice. Please try again.")




