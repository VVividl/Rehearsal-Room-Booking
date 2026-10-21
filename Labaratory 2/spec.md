## 6. Actors and Goals

To define the system boundaries and interaction workflows, there is two primary actors (one human and one external service):

### 1) Client (Human Actor)
A musician or band representative registered in the system.

**Primary Goals:** 
- Browse available rehearsal rooms and time slots.
- Create a booking for a specific room and time.
- Add additional equipment to the booking.
- Cancel an existing booking.

### 2) Payment Gateway (External System Actor)
An external API responsible for processing financial transactions.

**Primary Goals:** 
- Process the payment for a `PENDING` booking.
- Send a callback to the system to update the `booking_status` to `CONFIRMED`.
- Process refunds for `CANCELLED` bookings.

---

## 7. Requirements and Scenarios

To ensure clear communication between stakeholders and developers, we define our requirements using three distinct formats: User Stories (agile), EARS (formal system requirements), and Gherkin (testable behavior).

### 1) User Stories

- **US1 (Book a Room):** As a Client, I want to book an available rehearsal room for a specific time slot, so that my band has a place to practice.
- **US2 (Rent Equipment):** As a Client, I want to add extra equipment to my booking, so that I don't have to transport my own gear.
- **US3 (Payment Processing):** As a Payment Gateway, I want to notify the system of a successful transaction, so that the client's booking status changes to `CONFIRMED`.
- **US4 (Late Cancellation):** As a Client, I want to cancel my booking in case of some circumstences, so that the room becomes available for others. *(Note: This story is intentionally left without a Gherkin test scenario to demonstrate traceability gaps).*

### 2) EARS Requirements

- **REQ-01:** WHEN a Client selects an available room and time slot, the system SHALL create a new booking record with a `PENDING` status.
- **REQ-02:** WHILE a booking is in `PENDING` status, WHEN a Client adds an equipment item, the system SHALL capture the current `hourly_equip_rate` and add it to the booking's `total_price`.
- **REQ-03:** WHEN the Payment Gateway sends a success confirmation, the system SHALL update the `booking_status` to `CONFIRMED`.
- **REQ-04:** The system SHALL prevent double-booking of any room for overlapping time intervals.
- **REQ-05:** WHEN a Client cancels a `CONFIRMED` booking less than 24 hours before `start_datetime`, the system SHALL update the status to `CANCELLED` but retain the payment.

### 3) Gherkin Scenarios (Behavior-Driven Development)

**Scenario 1: Successful room booking**
- **Given** the Client is logged in
- **And** Room 1 is available on "2026-11-01" from 18:00 to 20:00
- **When** the Client creates a booking for Room 1 for this time slot
- **Then** the system should create a booking with status `PENDING`
- **And** the room should become unavailable for other clients during this time

**Scenario 2: Capturing equipment price at the time of booking**
- **Given** a Client has a `PENDING` booking
- **And** the catalog price for "Drum Set Cymbals" is 150 UAH/hour
- **When** the Client adds the "Drum Set Cymbals" to the booking
- **Then** the system should record the equipment rate as 150 UAH in `BookingEquipment`
- **And** recalculate the `total_price` of the booking

**Scenario 3: Payment confirmation by external API**
- **Given** a booking is in `PENDING` status
- **When** the Payment Gateway returns a "200 OK" success status for the transaction
- **Then** the system should change the booking status to `CONFIRMED`

