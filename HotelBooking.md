**Hotel Booking System**:
   - **Question**: Design an object-oriented system for managing hotel room bookings. The system should handle different room types, customer bookings, and availability checks.
   - **Answer Guide**:
     - **Classes**:
       - `Hotel`: Represents the hotel, including details like its name, location, and a list of available rooms.
       - `Room`: Represents different room types (e.g., single, double, suite) with attributes like room number, type, and price.
       - `Customer`: Stores customer details (name, contact information).
       - `Booking`: Represents a booking with attributes like booking ID, customer, room, and check-in/check-out dates.
     - **Key Methods**:
       - `Hotel`: Methods to check room availability and book rooms.
       - `Room`: Methods for managing room details.
       - `Booking`: Methods to create, cancel, or modify a booking.
     - **Relationships**: 
       - A `Hotel` contains multiple `Room` objects.
       - A `Customer` can make a `Booking` for a specific `Room`.

Here's a possible solution:

```python
class Room:
    def __init__(self, roomNumber, roomType, price):
        self.roomNumber = roomNumber
        self.roomType = roomType
        self.price = price
        self.isAvailable = True

class Customer:
    def __init__(self, name, contactInfo):
        self.name = name
        self.contactInfo = contactInfo

class Booking:
    def __init__(self, bookingID, customer, room, checkInDate, checkOutDate):
        self.bookingID = bookingID
        self.customer = customer
        self.room = room
        self.checkInDate = checkInDate
        self.checkOutDate = checkOutDate

class Hotel:
    def __init__(self, name, location):
        self.name = name
        self.location = location
        self.rooms = []

    def addRoom(self, room):
        self.rooms.append(room)

    def checkAvailability(self, roomType):
        for room in self.rooms:
            if room.roomType == roomType and room.isAvailable:
                return room
        return None

    def bookRoom(self, customer, roomType, checkInDate, checkOutDate):
        room = self.checkAvailability(roomType)
        if room:
            booking = Booking(len(self.rooms) + 1, customer, room, checkInDate, checkOutDate)
            room.isAvailable = False
            return booking
        else:
            return "No rooms available"

# Example usage
hotel = Hotel("Sunrise Hotel", "New York")
room1 = Room(101, "Single", 100)
room2 = Room(102, "Double", 150)
hotel.addRoom(room1)
hotel.addRoom(room2)

customer = Customer("Alice", "alice@example.com")
booking = hotel.bookRoom(customer, "Single", "2024-10-20", "2024-10-25")
print(vars(booking)) if isinstance(booking, Booking) else print(booking)
```

This system models the hotel booking process, handling room availability and customer reservations.