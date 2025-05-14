#include <iostream>
#include <vector>
#include <string>
using namespace std;

// Room class to manage room details
class Room {
public:
    int roomNumber;
    string type;
    float price;
    bool isAvailable;

    Room(int number, string roomType, float roomPrice) {
        roomNumber = number;
        type = roomType;
        price = roomPrice;
        isAvailable = true;
    }

    void display() {
        cout << "Room No: " << roomNumber 
             << ", Type: " << type 
             << ", Price: $" << price 
             << ", Status: " << (isAvailable ? "Available" : "Booked") << endl;
    }
};

// Booking class for customer bookings
class Booking {
public:
    string customerName;
    int roomNumber;
    int days;
    float totalAmount;

    Booking(string name, int roomNum, int stayDays, float rate) {
        customerName = name;
        roomNumber = roomNum;
        days = stayDays;
        totalAmount = days * rate;
    }

    void display() {
        cout << "\nCustomer: " << customerName
             << "\nRoom No: " << roomNumber
             << "\nDays: " << days
             << "\nTotal Amount: $" << totalAmount << endl;
    }
};

class Hotel {
    vector<Room> rooms;
    vector<Booking> bookings;

public:
    void addRoom(int number, string type, float price) {
        rooms.push_back(Room(number, type, price));
    }

    void showRooms() {
        cout << "\n--- Room List ---\n";
        for (Room &room : rooms) {
            room.display();
        }
    }

    void bookRoom() {
        string name;
        int days;
        showRooms();
        cout << "\nEnter Room Number to book: ";
        int number;
        cin >> number;

        for (Room &room : rooms) {
            if (room.roomNumber == number) {
                if (room.isAvailable) {
                    cout << "Enter customer name: ";
                    cin >> name;
                    cout << "Enter number of days: ";
                    cin >> days;

                    room.isAvailable = false;
                    Booking newBooking(name, room.roomNumber, days, room.price);
                    bookings.push_back(newBooking);
                    cout << "\nBooking successful!";
                    newBooking.display();
                    return;
                } else {
                    cout << "Room is already booked.\n";
                    return;
                }
            }
        }
        cout << "Invalid room number.\n";
    }

    void checkoutRoom() {
        cout << "Enter room number to checkout: ";
        int number;
        cin >> number;

        for (Room &room : rooms) {
            if (room.roomNumber == number) {
                if (!room.isAvailable) {
                    room.isAvailable = true;
                    cout << "Checkout successful. Room is now available.\n";
                    return;
                } else {
                    cout << "Room is already available.\n";
                    return;
                }
            }
        }
        cout << "Invalid room number.\n";
    }

    void showAllBookings() {
        cout << "\n--- Booking Records ---\n";
        for (Booking &b : bookings) {
            b.display();
            cout << "----------------------\n";
        }
    }
};

int main() {
    Hotel hotel;

    // Add sample rooms
    hotel.addRoom(101, "Single", 1000.0);
    hotel.addRoom(102, "Double", 1500.0);
    hotel.addRoom(103, "Suite", 2500.0);

    int choice;
    do {
        cout << "\n--- Hotel Management System ---\n";
        cout << "1. Show All Rooms\n";
        cout << "2. Book Room\n";
        cout << "3. Checkout Room\n";
        cout << "4. Show All Bookings\n";
        cout << "0. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            hotel.showRooms();
            break;
        case 2:
            hotel.bookRoom();
            break;
        case 3:
            hotel.checkoutRoom();
            break;
        case 4:
            hotel.showAllBookings();
            break;
        case 0:
            cout << "Thank you for using the Hotel Management System.\n";
            break;
        default:
            cout << "Invalid choice.\n";
        }
    } while (choice != 0);

    return 0;
}
