Microlearning Program in Data Structure And Algorithms with cpp
# Library-management-system code

#include <iostream>
#include <string>

using namespace std;

struct Book {
    int id;
    string title;
    string author;
    bool available;
    Book* next;

    Book(int bookId, const string& bookTitle, const string& bookAuthor)
        : id(bookId), title(bookTitle), author(bookAuthor), available(true), next(nullptr) {}
};

class Library {
private:
    Book* head;

public:
    Library() : head(nullptr) {}

    void addBook() {
        int bookId;
        string bookTitle, bookAuthor;

        cout << "Enter book ID: ";
        cin >> bookId;
        cin.ignore();
        cout << "Enter book title: ";
        getline(cin, bookTitle);
        cout << "Enter book author: ";
        getline(cin, bookAuthor);

        Book* newBook = new Book(bookId, bookTitle, bookAuthor);

        if (head == nullptr) {
            head = newBook;
        } else {
            Book* current = head;
            while (current->next != nullptr) {
                current = current->next;
            }
            current->next = newBook;
        }

        cout << "Book added successfully." << endl;
    }

    Book* binarySearch(int id) {
        Book* left = head;
        Book* right = nullptr;

        while (left != right) {
            Book* mid = getMiddle(left, right);
            if (mid->id < id) {
                left = mid->next;
            } else if (mid->id > id) {
                right = mid;
            } else {
                return mid;
            }
        }

        return nullptr;
    }

    Book* getMiddle(Book* left, Book* right) {
        Book* slow = left;
        Book* fast = left->next;

        while (fast != right) {
            fast = fast->next;
            if (fast != right) {
                slow = slow->next;
                fast = fast->next;
            }
        }

        return slow;
    }

    void searchBook() {
        int searchId;
        cout << "Enter book ID to search: ";
        cin >> searchId;

        Book* foundBook = binarySearch(searchId);

        if (foundBook != nullptr) {
            cout << "Book found!" << endl;
            cout << "Book ID: " << foundBook->id << endl;
            cout << "Title: " << foundBook->title << endl;
            cout << "Author: " << foundBook->author << endl;
            cout << "Status: " << (foundBook->available ? "Available" : "Issued") << endl;
        } else {
            cout << "Book not found." << endl;
        }
    }

    void issueBook() {
        int issueId;
        cout << "Enter book ID to issue: ";
        cin >> issueId;

        Book* foundBook = binarySearch(issueId);

        if (foundBook != nullptr) {
            if (foundBook->available) {
                foundBook->available = false;
                cout << "Book issued successfully." << endl;
            } else {
                cout << "Book is already issued." << endl;
            }
        } else {
            cout << "Book not found." << endl;
        }
    }

    void returnBook() {
        int returnId;
        cout << "Enter book ID to return: ";
        cin >> returnId;

        Book* foundBook = binarySearch(returnId);

        if (foundBook != nullptr) {
            if (!foundBook->available) {
                foundBook->available = true;
                cout << "Book returned successfully." << endl;
            } else {
                cout << "Book is already available." << endl;
            }
        } else {
            cout << "Book not found." << endl;
        }
    }

    void mergeSort(Book** headRef) {
        Book* headNode = *headRef;
        Book* left;
        Book* right;

        if (headNode == nullptr || headNode->next == nullptr) {
            return;
        }

        splitList(headNode, &left, &right);

        mergeSort(&left);
        mergeSort(&right);

        *headRef = mergeLists(left, right);
    }

    void splitList(Book* source, Book** frontRef, Book** backRef) {
        Book* slow = source;
        Book* fast = source->next;

        while (fast != nullptr) {
            fast = fast->next;
            if (fast != nullptr) {
                slow = slow->next;
                fast = fast->next;
            }
        }

        *frontRef = source;
        *backRef = slow->next;
        slow->next = nullptr;
    }

    Book* mergeLists(Book* left, Book* right) {
        Book dummy(0, "", ""); // Dummy node to hold the merged result
        Book* tail = &dummy;

        while (left != nullptr && right != nullptr) {
            if (left->title <= right->title) {
                tail->next = left;
                left = left->next;
            } else {
                tail->next = right;
                right = right->next;
            }
            tail = tail->next;
        }

        // Attach the remaining nodes
        if (left != nullptr) {
            tail->next = left;
        } else {
            tail->next = right;
        }

        return dummy.next;
    }

    void listBooks() {
        mergeSort(&head);

        cout << "List of books:" << endl;
        cout << "-------------------------------------------" << endl;
        cout << "ID\tTitle\t\tAuthor\t\tStatus" << endl;
        cout << "-------------------------------------------" << endl;

        Book* current = head;
        while (current != nullptr) {
            cout << current->id << "\t" << current->title << "\t\t" << current->author << "\t\t"
                 << (current->available ? "Available" : "Issued") << endl;
            current = current->next;
        }

        cout << "-------------------------------------------" << endl;
    }

    void deleteBook() {
        int deleteId;
        cout << "Enter book ID to delete: ";
        cin >> deleteId;

        if (head == nullptr) {
            cout << "No books in the library." << endl;
            return;
        }

        Book* current = head;
        Book* prev = nullptr;

        while (current != nullptr) {
            if (current->id == deleteId) {
                if (prev == nullptr) {
                    head = current->next;
                } else {
                    prev->next = current->next;
                }
                delete current;
                cout << "Book deleted successfully." << endl;
                return;
            }

            prev = current;
            current = current->next;
        }

        cout << "Book not found." << endl;
    }

    ~Library() {
        Book* current = head;
        while (current != nullptr) {
            Book* temp = current;
            current = current->next;
            delete temp;
        }
    }
};

int main() {
    Library library;

    int choice;
    do {
        cout << "|| Library Management System || " << endl;
        cout<< "    Made by Anand prem kumar" << endl;
        cout << "1. Add New Book" << endl;
        cout << "2. Search for a Book" << endl;
        cout << "3. Issue a Book" << endl;
        cout << "4. Return a Book" << endl;
        cout << "5. List All Books" << endl;
        cout << "6. Delete a Book" << endl;
        cout << "0. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 0:
                cout << "Exiting the program. Goodbye!" << endl;
                break;
            case 1:
                library.addBook();
                break;
            case 2:
                library.searchBook();
                break;
            case 3:
                library.issueBook();
                break;
            case 4:
                library.returnBook();
                break;
            case 5:
                library.listBooks();
                break;
            case 6:
                library.deleteBook();
                break;
            default:
                cout << "Invalid choice. Please try again." << endl;
                break;
        }

        cout << endl;
    } while (choice != 0);

    return 0;
}

