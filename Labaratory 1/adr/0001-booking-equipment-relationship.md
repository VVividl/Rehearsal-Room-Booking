# ADR 0001: Resolving Many-to-Many Relationship Between Bookings and Equipment

## Context and Problem Statement
In the Rehearsal Room Booking system, a single booking can include multiple additional equipment items (e.g., cymbals, amplifiers). At the same time, a specific equipment item from the catalog can be rented across many different bookings over time. 
Furthermore, the catalog price (`hourly_equip_rate`) of an equipment item may change in the future, but past and existing confirmed bookings must retain the price that was agreed upon at the time of reservation.

How should we model the relationship between `Booking` and `Equipment` to ensure data normalization and preserve historical pricing?

## Considered Options

* **Option 1:** Add an array or comma-separated list of `equipment_ids` directly into the `Booking` entity.
* **Option 2:** Use a standard direct Many-to-Many (M:N) relationship without additional attributes.
* **Option 3:** Introduce an associative entity `BookingEquipment` that acts as a bridge and stores transaction-specific attributes (`quantity`, `hourly_booking_rate`).

## Decision Outcome
Chosen option: **Option 3**, because it fully complies with the Third Normal Form (3NF) and satisfies all business requirements.

### Consequences
* **Good, because** it resolves the M:N relationship properly without implicit arrays, satisfying 1NF.
* **Good, because** it allows storing the `quantity` of the same item rented in a single booking.
* **Good, because** adding `hourly_booking_rate` to this associative entity captures a snapshot of the price at checkout, preventing historical data mutation if catalog prices change.
* **Bad, because** it requires one additional JOIN operation when querying the total list of equipment for a specific booking.
