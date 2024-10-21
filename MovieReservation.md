### **Movie Ticket Booking System**

#### Problem:
Design a movie ticket booking system where users can browse available movies, check showtimes, book tickets, and cancel bookings. The system should also manage seating arrangements and allow users to select their seats for each showtime.

#### Requirements:
1. **Users**:
   - Can browse available movies and showtimes.
   - Can book and cancel tickets.
   - Can select seats based on availability.

2. **Movies**:
   - Each movie has a title, duration, genre, and multiple showtimes.
   - Each showtime has a seating arrangement (e.g., rows and columns of seats).

3. **Booking**:
   - When booking a ticket, the user must select a movie, a showtime, and seats.
   - Once booked, the seats should no longer be available for other users.

4. **Cancellation**:
   - A user can cancel a booking, which will make the seats available for others.

5. **Seating Management**:
   - Each showtime has a seating layout (e.g., a 10x10 grid) indicating which seats are available or booked.

Here’s an object-oriented solution for the **Movie Ticket Booking System**:

### 1. **Classes Overview:**
   - **Movie**: Represents a movie with title, duration, and genre.
   - **Showtime**: Represents a showtime for a specific movie, with a seating arrangement.
   - **Seat**: Represents an individual seat in a theater for a particular showtime.
   - **User**: Represents a user who can browse, book, and cancel tickets.
   - **Booking**: Represents a booking made by a user for a movie showtime.

### 2. **Class Definitions:**

```python
class Seat:
    def __init__(self, row, number):
        self.row = row
        self.number = number
        self.isBooked = False

    def book(self):
        if not self.isBooked:
            self.isBooked = True
            return True
        return False

    def cancel(self):
        if self.isBooked:
            self.isBooked = False
            return True
        return False


class Showtime:
    def __init__(self, movieTitle, time, numRows, numSeatsPerRow):
        self.movieTitle = movieTitle
        self.time = time
        self.seats = [[Seat(row, number) for number in range(1, numSeatsPerRow + 1)] for row in range(1, numRows + 1)]

    def displayAvailableSeats(self):
        availableSeats = []
        for row in self.seats:
            for seat in row:
                if not seat.isBooked:
                    availableSeats.append((seat.row, seat.number))
        return availableSeats

    def bookSeats(self, seatList):
        for row, number in seatList:
            seat = self.seats[row-1][number-1]
            if not seat.book():
                return False  # If any seat is already booked, return False
        return True

    def cancelSeats(self, seatList):
        for row, number in seatList:
            seat = self.seats[row-1][number-1]
            seat.cancel()


class Movie:
    def __init__(self, title, duration, genre):
        self.title = title
        self.duration = duration
        self.genre = genre
        self.showtimes = []

    def addShowtime(self, showtime):
        self.showtimes.append(showtime)

    def getShowtimes(self):
        return [(showtime.time, showtime.displayAvailableSeats()) for showtime in self.showtimes]


class Booking:
    def __init__(self, user, showtime, seatList):
        self.user = user
        self.showtime = showtime
        self.seatList = seatList
        self.isConfirmed = False

    def confirm(self):
        if self.showtime.bookSeats(self.seatList):
            self.isConfirmed = True
            return True
        return False

    def cancel(self):
        if self.isConfirmed:
            self.showtime.cancelSeats(self.seatList)
            self.isConfirmed = False
            return True
        return False


class User:
    def __init__(self, name):
        self.name = name
        self.bookings = []

    def browseMovies(self, movies):
        for movie in movies:
            print(f"{movie.title} ({movie.genre}, {movie.duration} mins)")
            for time, availableSeats in movie.getShowtimes():
                print(f"Showtime: {time}, Available Seats: {availableSeats}")

    def bookTickets(self, movie, showtime, seatList):
        booking = Booking(self, showtime, seatList)
        if booking.confirm():
            self.bookings.append(booking)
            print(f"Booking confirmed for {movie.title} at {showtime.time}, Seats: {seatList}")
        else:
            print("Booking failed. Some seats are already booked.")

    def cancelBooking(self, booking):
        if booking.cancel():
            self.bookings.remove(booking)
            print("Booking cancelled.")
        else:
            print("Cancellation failed. Booking was not confirmed.")


# Example usage:
if __name__ == "__main__":
    # Create movies
    movie1 = Movie("Inception", 148, "Sci-Fi")
    movie2 = Movie("The Godfather", 175, "Crime")

    # Create showtimes
    showtime1 = Showtime("Inception", "18:00", 5, 10)  # 5 rows, 10 seats per row
    showtime2 = Showtime("Inception", "21:00", 5, 10)

    # Add showtimes to movies
    movie1.addShowtime(showtime1)
    movie1.addShowtime(showtime2)

    # Create a user
    user = User("Alice")

    # Browse movies and showtimes
    user.browseMovies([movie1, movie2])

    # Book tickets
    user.bookTickets(movie1, showtime1, [(1, 1), (1, 2)])  # Booking seats (1,1) and (1,2)
    
    # Try to book the same seats again (this should fail)
    user.bookTickets(movie1, showtime1, [(1, 1)])

    # Cancel the booking
    if user.bookings:
        user.cancelBooking(user.bookings[0])

    # Browse again to see updated seat availability
    user.browseMovies([movie1])
```

### **Explanation:**

1. **Seat Class**: Each seat has a row, number, and booking status (whether it's booked or not).
2. **Showtime Class**: Represents a movie showing at a particular time, with a seating arrangement (rows and seats).
   - `displayAvailableSeats`: Lists all unbooked seats.
   - `bookSeats`: Books multiple seats at once, checking availability.
   - `cancelSeats`: Cancels bookings for the specified seats.
3. **Movie Class**: Represents a movie with showtimes and metadata (title, genre, duration).
4. **Booking Class**: Handles booking logic. When a user confirms a booking, it locks the seats.
5. **User Class**: Manages user interactions like browsing movies, booking tickets, and canceling bookings.

### **Example Usage**:
- **Browse Movies**: The user can see the available movies and showtimes, along with available seats.
- **Book Tickets**: The user selects a movie, showtime, and seats. If available, the seats are booked.
- **Cancel Bookings**: Users can cancel their bookings, making the seats available again.

This approach covers core OOP principles like encapsulation, inheritance, and modularity.