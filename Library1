#include <iostream>
#include <string>
#include <vector>
# include<algorithm>
#include <stdexcept> // For exceptions
using namespace std;

// Enum for Book Genre
enum Genre { Fiction, Nonfiction, Periodical, Biography, Children };

// Helper function to convert Genre to string
string genreToString(Genre g) {
    switch (g) {
    case Fiction: return "Fiction";
    case Nonfiction: return "Nonfiction";
    case Periodical: return "Periodical";
    case Biography: return "Biography";
    case Children: return "Children";
    default: return "Unknown";
    }
}

// Class Book

class Book {
private:
    string ISBN;
    string Title;
    string Author;
    int CopyrightDate;
    bool CheckedOut;
    Genre genre;

public:
    // Constructor
    Book(string isbn, string title, string author, int copyrightDate, Genre g)
        : ISBN(isbn), Title(title), Author(author), CopyrightDate(copyrightDate), CheckedOut(false), genre(g) {}

    // Getters
    string getISBN() const { return ISBN; }
    string getTitle() const { return Title; }
    string getAuthor() const { return Author; }
    int getCopyrightDate() const { return CopyrightDate; }
    bool isCheckedOut() const { return CheckedOut; }
    Genre getGenre() const { return genre; }

    // Functions to check out and check in the book
    void checkOut() { CheckedOut = true; }
    void checkIn() { CheckedOut = false; }

    // Overloaded operators
    bool operator==(const Book& other) const { return ISBN == other.ISBN; }
    bool operator!=(const Book& other) const { return ISBN != other.ISBN; }

    // Overload << operator for display
    friend ostream& operator<<(ostream& os, const Book& b) {
        os << "Title: " << b.Title << "\n"
           << "Author: " << b.Author << "\n"
           << "ISBN: " << b.ISBN << "\n"
           << "Genre: " << genreToString(b.genre) << "\n";
        return os;
    }
};

// Class Patron

class Patron {
private:
    string UserName;
    string CardNumber;
    int OwedFees;

public:
    // Constructor
    Patron(string userName, string cardNumber, int owedFees = 0)
        : UserName(userName), CardNumber(cardNumber), OwedFees(owedFees) {}

    // Getters
    string getUserName() const { return UserName; }
    string getCardNumber() const { return CardNumber; }
    int getOwedFees() const { return OwedFees; }

    // Setter for fees
    void setOwedFees(int fees) { OwedFees = fees; }

    // Check if user owes fees
    bool owesFees() const { return OwedFees > 0; }
};

//Class Transaction

class Transaction {
private:
    Book book;
    Patron patron;
    string activity; // "Check Out" or "Check In"
    string date;

public:
    // Constructor
    Transaction(Book b, Patron p, string act, string d)
        : book(b), patron(p), activity(act), date(d) {}

    // Display transaction
    void displayTransaction() const {
        cout << "Transaction:\n";
        cout << book << "\n"
             << "Patron: " << patron.getUserName() << "\n"
             << "Activity: " << activity << "\n"
             << "Date: " << date << "\n";
    }
};

// Class Library

class Library {
private:
    vector<Book> books;
    vector<Patron> patrons;
    vector<Transaction> transactions;

public:
    // Function to add a book
    void addBook(const Book& b) {
        books.push_back(b);
    }

    // Function to add a patron
    void addPatron(const Patron& p) {
        patrons.push_back(p);
    }

    // Function to check out a book
    void checkOutBook(const string& isbn, const string& cardNumber, const string& date) {
        // Find the book
        auto bookIt =find_if(books.begin(), books.end(), [&isbn](const Book& b) { return b.getISBN() == isbn; });
        if (bookIt == books.end())
            throw runtime_error("Book not found in library!");

        // Find the patron
        auto patronIt = find_if(patrons.begin(), patrons.end(), [&cardNumber](const Patron& p) { return p.getCardNumber() == cardNumber; });
        if (patronIt == patrons.end())
            throw runtime_error("Patron not found in library!");

        // Check if the patron owes fees
        if (patronIt->owesFees())
            throw runtime_error("Patron owes fees and cannot check out books!");

        // Check out the book and create a transaction
        bookIt->checkOut();
        transactions.emplace_back(*bookIt, *patronIt, "Check Out", date);
    }

    // Function to get patrons who owe fees
    vector<string> patronsWithFees() const {
        vector<string> owingPatrons;
        for (const auto& patron : patrons) {
            if (patron.owesFees())
                owingPatrons.push_back(patron.getUserName());
        }
        return owingPatrons;
    }

    // Display all transactions
    void displayTransactions() const {
        for (const auto& t : transactions) {
            t.displayTransaction();
            cout << "----------------------\n";
			
		}
	}
};
	int main() {
    Library library;

    // Add some books
    library.addBook(Book("12345", "C++ Programming", "Bjarne Stroustrup", 1985, Nonfiction));
    library.addBook(Book("67890", "The Catcher in the Rye", "J.D. Salinger", 1951, Fiction));

    // Add some patrons
    library.addPatron(Patron("Alice", "A001"));         // Default owed fees = 0
    library.addPatron(Patron("Aline", "B002", 50));       // Aline owes fees

    // Test transactions
    try {
        library.checkOutBook("12345", "A001", "2024-11-27"); // Alice checks out a book
    } catch (const exception& e) {
        cerr << "Error: " << e.what() << "\n";
    }

    // Attempt to check out a book for Aline (who owes fees)
    try {
        library.checkOutBook("67890", "B002", "2024-11-27");
    } catch (const exception& e) {
        cerr << "Error: " << e.what() << "\n";
    }

    // Display patrons who owe fees
    vector<string> owingPatrons = library.patronsWithFees();
    cout << "Patrons owing fees:\n";
    for (const auto& name : owingPatrons) {
        cout << name << "\n";
    }

    // Display all transactions
    cout << "\nAll Transactions:\n";
    library.displayTransactions();

    return 0;
}
