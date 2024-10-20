

### OOP Question: Design a Library Management System

**Question:**

Design a library management system using object-oriented programming principles. The system should allow users to borrow and return books, as well as manage the library’s inventory of books. 

**Requirements:**
1. **Classes**:
   - **Book**: Represents a book with attributes such as title, author, ISBN, and availability status.
   - **User**: Represents a user with attributes like name, user ID, and a list of borrowed books.
   - **Library**: Represents the library itself, managing the collection of books and user interactions.

2. **Methods**:
   - In the `Book` class:
     - A method to check if the book is available.
     - A method to mark the book as borrowed or returned.
   
   - In the `User` class:
     - A method to borrow a book.
     - A method to return a book.
   
   - In the `Library` class:
     - A method to add a book to the inventory.
     - A method to search for a book by title or author.
     - A method to list all available books.

3. **Additional Considerations**:
   - Handle cases where a user tries to borrow a book that is not available.
   - Ensure that a user can only borrow a limited number of books (e.g., 3 at a time).

### How to Approach the Answer:

1. **Define the Classes**: Start by outlining the attributes and methods for each class.

2. **Implement the Classes**: Provide a simple implementation for each class and its methods.

3. **Explain the Design Choices**: Discuss why you chose certain attributes and methods, how they interact, and any design patterns you might use (like composition or inheritance).

4. **Handle Edge Cases**: Discuss how you would handle scenarios like a user trying to borrow more than the allowed number of books or attempting to borrow a book that is already checked out.

### Example Implementation

Here’s a basic implementation to illustrate the concept:

```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_available = True

    def check_availability(self):
        return self.is_available

    def borrow(self):
        if self.is_available:
            self.is_available = False
            return True
        return False

    def return_book(self):
        self.is_available = True


class User:
    def __init__(self, name, user_id):
        self.name = name
        self.user_id = user_id
        self.borrowed_books = []

    def borrow_book(self, book):
        if len(self.borrowed_books) < 3 and book.borrow():
            self.borrowed_books.append(book)
            return True
        return False

    def return_book(self, book):
        if book in self.borrowed_books:
            book.return_book()
            self.borrowed_books.remove(book)
            return True
        return False


class Library:
    def __init__(self):
        self.books = []

    def add_book(self, book):
        self.books.append(book)

    def search_by_title(self, title):
        return [book for book in self.books if book.title.lower() == title.lower()]

    def search_by_author(self, author):
        return [book for book in self.books if book.author.lower() == author.lower()]

    def list_available_books(self):
        return [book for book in self.books if book.check_availability()]
```

### Conclusion:

This example demonstrates the use of object-oriented principles such as encapsulation and separation of concerns. By designing the system this way, you can easily extend it in the future (for example, by adding features like overdue notifications or different user types). Be prepared to discuss potential improvements or features that could enhance the system during the interview.

