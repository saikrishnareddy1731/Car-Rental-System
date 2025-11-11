# 🚗 Car Rental System - Full UML Design with Relation Types Explained

This project models a **Car Rental System** using **UML Class Diagram** in **Mermaid syntax** to represent real-world entities like Users, Vehicles, Stores, Reservations, Bills, and Payments.  
It demonstrates **OOP principles** (Inheritance, Composition, Aggregation, Association) and ensures **system stability, modularity, and scalability**.

---

## 🧾 Requirements

1. **System Stability**  
   - Each component is modular and independently testable.  
   - Uses abstraction and interfaces (`VehicleInventoryManagement`) for flexibility.  
   - Strong composition ensures lifecycle control (e.g., Store → Location).  

2. **Object Descriptions**
   - **User** – Represents a customer with a valid driving license.  
   - **Store** – Represents a branch managing vehicle inventories.  
   - **Vehicle** – Abstract class; base for `Car` and `Bike`.  
   - **Reservation** – Connects a User and a Vehicle.  
   - **Bill** – Generated after reservation completion.  
   - **Payment** – Final transaction linked to a Bill.  

---

## 🧱 UML Class Diagram (Mermaid Syntax)

```mermaid
classDiagram
    title Car Rental System - Full UML Design with Relation Types Explained

    %% ================= ENUMS =================
    class VehicleType {
      <<enumeration>>
      CAR
      BIKE
    }

    class Status {
      <<enumeration>>
      ACTIVE
      INACTIVE
    }

    class ReservationStatus {
      <<enumeration>>
      SCHEDULED
      COMPLETED
      CLOSED
    }

    class PaymentMode {
      <<enumeration>>
      CASH
      CARD
      UPI
    }

    %% ================= CORE CLASSES =================
    class VehicleRentalSystem {
      - List~User~ users
      - List~Store~ stores
      + List~Store~ getStores()
    }

    class User {
      - int userId
      - String drivingLicense
    }

    class Location {
      - String address
      - String city
      - String state
      - int zipcode
    }

    class Store {
      - int storeId
      - Location location
      - VehicleInventoryManagement inventory
      + List~Vehicle~ searchVehicle(VehicleType type)
    }

    %% ================= VEHICLE & INVENTORY =================
    class Vehicle {
      <<abstract>>
      - int id
      - String vehicleNo
      - VehicleType type
      - int odometer
      - Status status
    }

    class Car {
      - String fuelType
      - int seats
    }

    class Bike {
      - String brand
    }

    class VehicleInventoryManagement {
      <<interface>>
      + addVehicle(v: Vehicle)
      + removeVehicle(v: Vehicle)
      + search(type: VehicleType): List~Vehicle~
    }

    class CarInventoryManagement {
      - List~Car~ cars
      + addVehicle(v: Car)
      + removeVehicle(v: Car)
      + search(): List~Car~
    }

    class BikeInventoryManagement {
      - List~Bike~ bikes
      + addVehicle(v: Bike)
      + removeVehicle(v: Bike)
      + search(): List~Bike~
    }

    %% ================= RESERVATION, BILL, PAYMENT =================
    class Reservation {
      - int reservationId
      - User user
      - Vehicle vehicle
      - Date bookedFrom
      - Date bookedTo
      - Location pickupLocation
      - ReservationStatus status
    }

    class Bill {
      - int billId
      - double totalAmount
      - Reservation reservation
      + generateBill()
    }

    class Payment {
      - int paymentId
      - double amount
      - PaymentMode mode
      - Bill bill
      + makePayment()
    }

    %% ================= RELATIONSHIPS =================

    %% Composition (strong ownership)
    VehicleRentalSystem "1" *-- "*" Store : composition\n(System owns stores)
    Store *-- Location : composition\n(Store has one location)
    Reservation *-- Bill : composition\n(Reservation creates Bill)
    Bill *-- Payment : composition\n(Bill generates Payment)

    %% Aggregation (weak ownership)
    VehicleRentalSystem "1" o-- "*" User : aggregation\n(System manages users)
    Store o-- VehicleInventoryManagement : aggregation\n(Store uses inventory)
    Reservation o-- Vehicle : aggregation\n(Reservation uses a Vehicle)
    Reservation o-- Location : aggregation\n(Reservation pickup at location)

    %% Association (simple link)
    User "1" -- "*" Reservation : association\n(User makes multiple reservations)

    %% Inheritance / Interface implementation
    Vehicle <|-- Car : inheritance\n(Car is a Vehicle)
    Vehicle <|-- Bike : inheritance\n(Bike is a Vehicle)
    VehicleInventoryManagement <|.. CarInventoryManagement : implements / inheritance
    VehicleInventoryManagement <|.. BikeInventoryManagement : implements / inheritance

    %% Enum Aggregations
    Vehicle o-- VehicleType : aggregation\n(Vehicle uses a type)
    Vehicle o-- Status : aggregation\n(Vehicle has status)
    Reservation o-- ReservationStatus : aggregation\n(Reservation status)
    Payment o-- PaymentMode : aggregation\n(Payment mode)
