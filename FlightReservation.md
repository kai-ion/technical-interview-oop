**Flight Reservation System**:
   - **Question**: Design a flight reservation system that allows users to search for flights, book tickets, and manage their reservations.
   
   - **Answer Guide**:
     - **Classes**:
       - `Flight`: Represents a flight, including attributes such as flight number, airline, departure and arrival locations, date/time, and seat availability.
       - `Passenger`: Represents a passenger with attributes like name, age, contact info, and a list of bookings.
       - `Booking`: Represents a booking made by a passenger, including booking ID, flight details, seat number, and status (confirmed, canceled).
       - `Seat`: Represents a seat on a flight, with attributes like seat number, class (economy, business), and availability status.
       - `ReservationSystem`: Manages available flights, bookings, and searches for flights based on criteria (date, destination, etc.).

   - **Key Operations**:
     - `ReservationSystem`: Methods for searching flights, booking tickets, and canceling reservations.
     - `Passenger`: Methods to view current bookings and manage reservations.
     - `Flight`: Methods to track available seats and display flight details.
     - `Seat`: Methods to reserve and cancel seat availability based on bookings. 

This system offers a rich opportunity to show how you can design relationships between classes, manage real-world constraints, and demonstrate your ability to design scalable and maintainable systems.

Here’s a solution for the **Flight Reservation System** using object-oriented principles in Python:

```python
# Class to represent a Seat in a flight
class Seat:
    def __init__(self, seatNumber, seatClass, isAvailable=True):
        self.seatNumber = seatNumber
        self.seatClass = seatClass  # 'Economy', 'Business', etc.
        self.isAvailable = isAvailable
    
    def reserveSeat(self):
        if self.isAvailable:
            self.isAvailable = False
            return True
        return False
    
    def cancelReservation(self):
        self.isAvailable = True


# Class to represent a Flight
class Flight:
    def __init__(self, flightNumber, airline, departure, arrival, dateTime, seats):
        self.flightNumber = flightNumber
        self.airline = airline
        self.departure = departure
        self.arrival = arrival
        self.dateTime = dateTime
        self.seats = seats  # List of Seat objects

    def getAvailableSeats(self):
        return [seat for seat in self.seats if seat.isAvailable]

    def reserveSeat(self, seatNumber):
        for seat in self.seats:
            if seat.seatNumber == seatNumber and seat.isAvailable:
                return seat.reserveSeat()
        return False

    def cancelSeatReservation(self, seatNumber):
        for seat in self.seats:
            if seat.seatNumber == seatNumber:
                seat.cancelReservation()
                return True
        return False


# Class to represent a Passenger
class Passenger:
    def __init__(self, name, age, contactInfo):
        self.name = name
        self.age = age
        self.contactInfo = contactInfo
        self.bookings = []

    def addBooking(self, booking):
        self.bookings.append(booking)

    def viewBookings(self):
        return self.bookings


# Class to represent a Booking
class Booking:
    def __init__(self, bookingID, passenger, flight, seatNumber):
        self.bookingID = bookingID
        self.passenger = passenger
        self.flight = flight
        self.seatNumber = seatNumber
        self.status = "Confirmed"

    def cancel(self):
        self.flight.cancelSeatReservation(self.seatNumber)
        self.status = "Cancelled"


# Reservation System to manage flights and bookings
class ReservationSystem:
    def __init__(self):
        self.flights = []
        self.bookings = []
        self.bookingIDCounter = 1

    def addFlight(self, flight):
        self.flights.append(flight)

    def searchFlights(self, departure, arrival):
        availableFlights = [flight for flight in self.flights if flight.departure == departure and flight.arrival == arrival]
        return availableFlights

    def bookFlight(self, passenger, flightNumber, seatNumber):
        flight = next((f for f in self.flights if f.flightNumber == flightNumber), None)
        if flight and flight.reserveSeat(seatNumber):
            bookingID = self.bookingIDCounter
            booking = Booking(bookingID, passenger, flight, seatNumber)
            self.bookings.append(booking)
            passenger.addBooking(booking)
            self.bookingIDCounter += 1
            return booking
        return None

    def cancelBooking(self, bookingID):
        booking = next((b for b in self.bookings if b.bookingID == bookingID), None)
        if booking and booking.status == "Confirmed":
            booking.cancel()
            return True
        return False


# Example usage:

# Create some seats
seats = [Seat(i, 'Economy') for i in range(1, 6)]

# Create a flight
flight1 = Flight(101, "Capital Air", "Seattle", "New York", "2024-10-22 10:00 AM", seats)

# Create a passenger
passenger1 = Passenger("Cai Lin", 29, "cai@example.com")

# Initialize the reservation system
system = ReservationSystem()

# Add the flight to the system
system.addFlight(flight1)

# Search for flights
availableFlights = system.searchFlights("Seattle", "New York")
print(f"Available Flights: {[flight.flightNumber for flight in availableFlights]}")

# Book a seat on the flight
booking = system.bookFlight(passenger1, 101, 2)
if booking:
    print(f"Booking successful! Booking ID: {booking.bookingID}")
else:
    print("Booking failed. Seat may not be available.")

# View passenger's bookings
print(f"Passenger's Bookings: {[(b.bookingID, b.flight.flightNumber, b.seatNumber) for b in passenger1.viewBookings()]}")

# Cancel the booking
if system.cancelBooking(booking.bookingID):
    print("Booking cancelled successfully.")
else:
    print("Failed to cancel booking.")
```

### Key Components:
- **Seat Class**: Handles seat attributes and reservation logic.
- **Flight Class**: Manages flight information and seat reservations.
- **Passenger Class**: Represents passengers and their bookings.
- **Booking Class**: Encapsulates booking details.
- **ReservationSystem Class**: Handles flight searches, bookings, and cancellations.

This setup demonstrates basic OOP principles with relationships between objects and proper abstraction in designing a flight reservation system.