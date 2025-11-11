
```mermaid
classDiagram
    class VehicleRentalSystem
    class Store
    class Location
    class Reservation
    class Bill
    class Payment
    class User
    class Vehicle
    class Car
    class Bike
    class VehicleInventoryManagement
    class CarInventoryManagement
    class BikeInventoryManagement
    class VehicleType
    class Status
    class ReservationStatus
    class PaymentMode

    %% Composition (strong ownership)
    VehicleRentalSystem *-- Store
    Store *-- Location
    Reservation *-- Bill
    Bill *-- Payment

    %% Aggregation (shared ownership)
    VehicleRentalSystem o-- User
    Store o-- VehicleInventoryManagement
    Reservation o-- Vehicle
    Reservation o-- Location

    %% Association (general link)
    User --> Reservation

    %% Inheritance
    Vehicle <|-- Car
    Vehicle <|-- Bike
    VehicleInventoryManagement <|.. CarInventoryManagement
    VehicleInventoryManagement <|.. BikeInventoryManagement

    %% Enum aggregation
    Vehicle o-- VehicleType
    Vehicle o-- Status
    Reservation o-- ReservationStatus
    Payment o-- PaymentMode
```

## Relationship Explanations

- **Composition:**  
  - `VehicleRentalSystem *-- Store`: System owns Stores  
  - `Store *-- Location`: Store has Location  
  - `Reservation *-- Bill`: Reservation creates Bill  
  - `Bill *-- Payment`: Bill generates Payment  

- **Aggregation:**  
  - `VehicleRentalSystem o-- User`: System manages Users  
  - `Store o-- VehicleInventoryManagement`: Store uses Inventory  
  - `Reservation o-- Vehicle`: Reservation uses Vehicle  
  - `Reservation o-- Location`: Pickup Location  

- **Association:**  
  - `User --> Reservation`: User makes Reservations

- **Inheritance & Strategy Pattern:**  
  - `Vehicle <|-- Car`, `Vehicle <|-- Bike`: Car/Bike inherit Vehicle  
  - `VehicleInventoryManagement <|.. CarInventoryManagement`, `VehicleInventoryManagement <|.. BikeInventoryManagement`: Strategy Pattern

- **Enum aggregation:**  
  - `Vehicle o-- VehicleType`, `Vehicle o-- Status`,  
  - `Reservation o-- ReservationStatus`, `Payment o-- PaymentMode`
