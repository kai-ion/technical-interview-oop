### **Parking Lot System**

**Question**: Design a parking lot system that manages different types of vehicles (e.g., cars, motorcycles, trucks). The parking lot has multiple levels, and each level has a fixed number of parking spaces for each vehicle type. How would you design the system to allow vehicles to park and leave, and track the availability of spaces?

**Key Points to Address**:
- **Classes**: Define classes for `ParkingLot`, `Level`, `ParkingSpace`, `Vehicle`, and specific vehicle types like `Car`, `Motorcycle`, and `Truck`.
- **Inheritance**: Use inheritance for the `Vehicle` class, where each vehicle type extends the base `Vehicle` class.
- **Parking Logic**: Implement methods to check availability and park a vehicle in the appropriate space.
- **Exit Logic**: Implement logic for a vehicle leaving and updating the available spaces.
- **Additional Features**: You could also add features like calculating parking fees based on the time parked.


Here’s a possible solution for the **Parking Lot System** using object-oriented programming principles. The solution breaks the system into several classes, with clear responsibilities and interactions.

```python
from abc import ABC, abstractmethod

# Enum to define vehicle types
class VehicleSize:
    MOTORCYCLE = 1
    COMPACT = 2
    LARGE = 3

# Abstract class for Vehicle
class Vehicle(ABC):
    def __init__(self, license_plate, vehicle_size):
        self.license_plate = license_plate
        self.vehicle_size = vehicle_size
        self.parking_spot = None

    def park_in_spot(self, parking_spot):
        self.parking_spot = parking_spot

    def leave_spot(self):
        self.parking_spot = None

    @abstractmethod
    def get_size(self):
        pass

# Vehicle Types
class Motorcycle(Vehicle):
    def __init__(self, license_plate):
        super().__init__(license_plate, VehicleSize.MOTORCYCLE)

    def get_size(self):
        return VehicleSize.MOTORCYCLE

class Car(Vehicle):
    def __init__(self, license_plate):
        super().__init__(license_plate, VehicleSize.COMPACT)

    def get_size(self):
        return VehicleSize.COMPACT

class Truck(Vehicle):
    def __init__(self, license_plate):
        super().__init__(license_plate, VehicleSize.LARGE)

    def get_size(self):
        return VehicleSize.LARGE

# Parking Spot class
class ParkingSpot:
    def __init__(self, spot_id, size):
        self.spot_id = spot_id
        self.size = size
        self.vehicle = None

    def is_available(self):
        return self.vehicle is None

    def can_fit_vehicle(self, vehicle):
        return self.is_available() and self.size >= vehicle.get_size()

    def park(self, vehicle):
        if self.can_fit_vehicle(vehicle):
            self.vehicle = vehicle
            vehicle.park_in_spot(self)
            return True
        return False

    def remove_vehicle(self):
        self.vehicle.leave_spot()
        self.vehicle = None

# Parking Level class
class ParkingLevel:
    def __init__(self, floor, num_motorcycle_spots, num_compact_spots, num_large_spots):
        self.floor = floor
        self.parking_spots = []

        # Initialize parking spots by type
        self.parking_spots += [ParkingSpot(i, VehicleSize.MOTORCYCLE) for i in range(num_motorcycle_spots)]
        self.parking_spots += [ParkingSpot(i, VehicleSize.COMPACT) for i in range(num_motorcycle_spots, num_motorcycle_spots + num_compact_spots)]
        self.parking_spots += [ParkingSpot(i, VehicleSize.LARGE) for i in range(num_motorcycle_spots + num_compact_spots, num_motorcycle_spots + num_compact_spots + num_large_spots)]

    def park_vehicle(self, vehicle):
        for spot in self.parking_spots:
            if spot.can_fit_vehicle(vehicle):
                return spot.park(vehicle)
        return False

    def leave_parking_spot(self, vehicle):
        if vehicle.parking_spot is not None:
            vehicle.parking_spot.remove_vehicle()

# Parking Lot class
class ParkingLot:
    def __init__(self, num_levels, spots_per_level):
        self.levels = [ParkingLevel(i, *spots_per_level) for i in range(num_levels)]

    def park_vehicle(self, vehicle):
        for level in self.levels:
            if level.park_vehicle(vehicle):
                print(f"{vehicle.__class__.__name__} with license plate {vehicle.license_plate} parked.")
                return True
        print(f"No available spot for {vehicle.__class__.__name__} with license plate {vehicle.license_plate}.")
        return False

    def leave_parking(self, vehicle):
        if vehicle.parking_spot is not None:
            vehicle.parking_spot.remove_vehicle()
            print(f"{vehicle.__class__.__name__} with license plate {vehicle.license_plate} left the parking lot.")
            return True
        print(f"{vehicle.__class__.__name__} with license plate {vehicle.license_plate} was not found in the parking lot.")
        return False
```

### Key Concepts:
1. **Abstraction and Inheritance**: 
   - `Vehicle` is an abstract class. `Motorcycle`, `Car`, and `Truck` inherit from `Vehicle`.
   - Each vehicle type has its own behavior (through the `get_size` method).
   
2. **ParkingSpot**: Represents a single spot in the parking lot. It checks whether the vehicle fits based on size.

3. **ParkingLevel**: Manages parking spots on a single level and handles parking vehicles at that level.

4. **ParkingLot**: This is the main class that handles multiple levels of parking. It tries to park the vehicle at different levels.

### Example Usage:

```python
# Create a parking lot with 2 levels, each level having 2 motorcycle spots, 2 compact spots, and 1 large spot
parking_lot = ParkingLot(2, [2, 2, 1])

# Create vehicles
motorcycle = Motorcycle("M123")
car = Car("C456")
truck = Truck("T789")

# Park vehicles
parking_lot.park_vehicle(motorcycle)  # Should succeed
parking_lot.park_vehicle(car)         # Should succeed
parking_lot.park_vehicle(truck)       # Should succeed

# Try to park another truck (should fail as there's only one large spot per level)
truck2 = Truck("T987")
parking_lot.park_vehicle(truck2)      # Should fail

# Remove a vehicle from the parking lot
parking_lot.leave_parking(truck)      # Should succeed
```

### Explanation:
- We defined a parking lot that supports different types of vehicles.
- The parking system handles checking if a vehicle can park, based on available spaces and vehicle size.
- Inheritance is used to differentiate between different vehicle types, allowing for flexibility and scalability.

This is a classic object-oriented design problem that tests your understanding of class relationships, encapsulation, inheritance, and real-world system design.