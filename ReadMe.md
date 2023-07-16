 > # **Parking Lot**
## System Requirements

We will focus on the following set of requirements while designing the parking lot:
- [ ] The parking lot should have multiple floors where customers can park their cars.
- [ ] The parking lot should have multiple entry and exit points.
- [ ] Customers can collect a parking ticket from the entry points and can pay the parking fee at the exit points on their way out.
- [ ] Customers can pay the tickets at the automated exit panel or to the parking attendant.
- [ ] Customers can pay via both cash and credit cards.
- [ ] Customers should also be able to pay the parking fee at the customer's info portal on each floor. If the customer has paid at the info portal, they don't have to pay at the exit.
- [ ] The system should not allow more vehicles than the maximum capacity of the parking lot. If the parking is full, the system should be able to show a message at the entrance panel and on the parking display board on the ground floor.
- [ ] Each parking floor will have many parking spots. The system should support multiple types of parking spots such as Compact, Large, Handicapped, Motorcycle, etc.
- [ ] The Parking lot should have some parking spots specified for electric cars. These spots should have an electric panel through which customers can pay and charge their vehicles.
- [ ] The system should support parking for different types of vehicles like car, truck, van, motorcycle, etc.
- [ ] Each parking floor should have a display board showing any free parking spot for each spot type.
- [ ] The system should support a per-hour parking fee model. For example, customers have to pay $4 for the first hour, $3.5 for the second and third hours, and $2.5 for all the remaining hours.

```mermaid
classDiagram

class ParkingSpotType{
    <<enumeration>>
    Handicapped
    Compact
    Large
    Motorbike
    Electric
}

class VehicleType{
    <<enumeration>>
    Car
    Truck
    Electric
    Van
    Motorbike
}

class ParkingTicketStatus{
    <<enumeration>>
    Active
    Paid
    Lost
}

class AccountStatus{
    <<enumeration>>
    Active
    Closed
    Canceled
    Blacklisted
    None
}

class Location{
    <<dataType>>
    streetAddress: string
    city: string
    state: string
    zipcode: string
    country: string
}

class Person{
    <<dataType>>
    name: string
    address: string
    email: string
    phone: string
}


```

```mermaid
classDiagram

class HandicappedSpot{  }
class CompactSpot{  }
class LargeSpot{  }
class MotorbikeSpot{  }
class ElectricSpot{  }

class Car{ }
class Truck{ }
class Electric{ }
class Van{ }
class MotorBike{  }

class ParkingSpot{
    String: number
    Bool: free
    ParkingSlotType: type
    getIsFree() bool
}

class Vehicle {
    String: licenseNumber
    VehicleType: type
    assignTicket() void
}

Car --|> Vehicle : Extends
Truck --|> Vehicle : Extends
Electric --|> Vehicle : Extends
Van --|> Vehicle : Extends
MotorBike --|> Vehicle : Extends

HandicappedSpot --|> ParkingSpot : Extends
CompactSpot --|> ParkingSpot : Extends
LargeSpot --|> ParkingSpot : Extends
MotorbikeSpot --|> ParkingSpot : Extends
ElectricSpot --|> ParkingSpot : Extends

ParkingSpot --> Vehicle : Association

class Account{
   String: username
   String: password
   AccountStatus: status
}

class Admin{
   addParkingFloor() bool
}

class ParkingAttendant{
    processTicket() bool
}

Admin --|> Account : Extends
ParkingAttendant --|> Account : Extends

class Payment{
    Date: creationDate
    Double: amount
    PaymentStatus: status
}

class CreditCardTransaction{
    String: nameOnCard
}

class CashTransaction{
    Double: cashTendered 
}

CreditCardTransaction --|> Payment : Extends
CashTransaction --|> Payment : Extends

class ElectricPanel{
    Int: payedForMinutes
    DateTime: chargingStartTime
    cancelCharging() bool
}

ElectricPanel --> Payment : Association
ElectricPanel --* ElectricSpot : Composition

class ParkingTicket{
    String: ticketNumber
    DateTime: issuedAt
    DateTime: payedAt
    Double: payedAmount
    ParkingTicketStatus: status
}

class EntrancePanel{
   String: id
   printTicket() bool
}

class ExitPanel{
   String: id
   scanTicket() bool
   processPayment() bool
}
class ParkingAttendantPortal{
    String: id
    scanTicket() bool
    processPayment() bool
}

class CustomerInfoPortal{
    String: id
    scanTicket() bool
    processpayment() bool
}

ParkingAttendant --> ParkingTicket : Association
EntrancePanel --> ParkingTicket : Association
ExitPanel --> ParkingTicket : Association
ParkingAttendantPortal --> ParkingTicket : Association
CustomerInfoPortal --> ParkingTicket : Association
Vehicle --> ParkingTicket : Association


Payment -- ParkingTicket : Link

class ParkingLot{
   String: id
   Location: address 
   addParkingFloor() bool
   addEntrancePanel() bool
   getNewParkingTicket() parkingTicket
   isFull() bool
}


ParkingTicket --* ParkingLot : Composition
EntrancePanel --* ParkingLot : Composition
ExitPanel --* ParkingLot : Composition
ParkingAttendantPortal --* ParkingLot: Composition

class ParkingRate{
   Int: hourNumber
   Double: rate
}

class ParkingFloor{
    String: name
    updateDisplayBoard() void
    addParkingSlot() void
    assignVehicleToSlot() void
    freeSlot() void
}

ParkingRate --* ParkingLot : Composition
ParkingFloor --* ParkingLot : Composition

class ParkingDisplayBoard{
    String: id
    HandicappedSlot: handicappedSlot
    CompactSlot: compactFreeSpot
    LargeSlot: largeFreeSpot
    MotorbikeSlot: motorbikeFreeSpot
    ElectricSlot: electricFreeSpot
    showEmptySpotNumber() void
}

ParkingDisplayBoard --* ParkingFloor : Composition
ParkingSpot --* ParkingFloor : Composition
CustomerInfoPortal --* ParkingFloor : Composition
```