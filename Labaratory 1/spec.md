# Data Model Specification: Rehearsal Room Booking (spec.md)

## 1. Entities and Attributes Description

### Client
Represents a registered user of the rehearsal studio (a musician or band representative) who makes room and equipment reservations.
* `client_id` (PK) — Unique identifier for the client.
* `first_name` — Client's first name.
* `last_name` — Client's last name.
* `email` — Contact email address (unique).
* `phone_number` — Contact phone number for communication and confirmation.

### Room
Describes a specific rehearsal space with its technical capacity and base cost.
* `room_id` (PK) — Unique identifier for the room.
* `room_name` — Name or label of the room (e.g., "Loud Room", "Acoustic Stage").
* `capacity` — Maximum number of people allowed (band size).
* `hourly_room_rate` — Current base hourly rental rate for the room.

### Booking
Represents the reservation of a rehearsal room by a specific client for a defined time interval.
* `booking_id` (PK) — Unique identifier for the booking.
* `client_id` (FK -> Client.client_id) — ID of the client who created the booking.
* `room_id` (FK -> Room.room_id) — ID of the reserved room.
* `booking_status` — Lifecycle status of the reservation (e.g., PENDING, CONFIRMED, CANCELLED, COMPLETED).
* `start_datetime` — Date and exact start time of the rehearsal session.
* `end_datetime` — Date and exact end time of the rehearsal session.
* `total_price` — Calculated final cost of the reservation.

### Equipment
A catalog of additional musical gear and instruments available for temporary rent during a session.
* `equipment_id` (PK) — Unique identifier for the equipment item.
* `equipment_name` — Model name or instrument type (e.g., "Marshall JCM800", "Shure SM58").
* `category` — Type of backline (drums, amplifier, microphone, cables).
* `equipment_status` — Operational status of the item (AVAILABLE, MAINTENANCE).
* `hourly_equip_rate` — Current base hourly rental rate for the item.

### BookingEquipment
An associative entity resolving the Many-to-Many relationship between a rehearsal session and additional gear.
* `booking_equipment_id` (PK) — Unique identifier for the rental line item.
* `booking_id` (FK -> Booking.booking_id) — ID of the booking that includes the gear.
* `equipment_id` (FK -> Equipment.equipment_id) — ID of the rented instrument.
* `quantity` — Number of units rented for this specific item.
* `hourly_booking_rate` — Locked-in hourly rental rate at the time of order creation (preserves historical pricing).

---

## 2. Entity Relationships and Cardinalities

* **Client ↔ Booking (1:N):** A client can have 0 or many bookings. A single booking belongs strictly to one client.
* **Room ↔ Booking (1:N):** A room can be associated with 0 or many bookings for different time slots. A single booking is tied to exactly one room.
* **Booking ↔ BookingEquipment (1:N):** A booking may contain 0 or many additional equipment line items. Each equipment rental record belongs strictly to one booking.
* **Equipment ↔ BookingEquipment (1:N):** An item from the equipment catalog can be rented in 0 or many bookings. Each rental record points to one specific equipment type.
* **Booking ↔ Equipment (M:N):** The direct Many-to-Many relationship is fully resolved via the intermediate `BookingEquipment` entity.

---

## 3. Functional Requirements
- **FR-001**: The system MUST store bookings linked to a client, a room, and specific time boundaries (`start_datetime`, `end_datetime`).
- **FR-002**: The system MUST store the hourly rate of the equipment at the time of order creation (`hourly_booking_rate` in `BookingEquipment`), ensuring that price changes in the catalog do not affect the cost of already created bookings.
- **FR-003**: The system MUST allow creating a room booking without any additional equipment (the number of records in `BookingEquipment` can be 0).
- **FR-004**: The system MUST store the operational status of each equipment unit (`equipment_status`) to prevent adding broken inventory (`MAINTENANCE`) to new orders.
- **FR-005**: The `booking_id` + `equipment_id` pair in `BookingEquipment` MUST be unique: a specific catalog item within a single booking is recorded as one row specifying the `quantity`.
- **FR-006**: Time intervals (`start_datetime` and `end_datetime`) for the same room (`room_id`) in active bookings (statuses `CONFIRMED` / `PENDING`) MUST NOT overlap.
- **FR-007**: The system MUST calculate and record the final cost in the `total_price` field during order creation based on the room's duration and the added equipment items.

---

## 4. Edge Cases
- **Client cancels the booking at the last minute:** The booking receives `booking_status` = `CANCELLED`. If this occurs less than 24 hours before the start time, the system may retain the prepayment (business logic handled outside the DB, but the status is updated).
- **Equipment breaks right before the rehearsal:** The administrator changes the `equipment_status` to `MAINTENANCE`, removes or replaces the corresponding record in `BookingEquipment` for the current booking, and recalculates the `total_price`.
- **Double booking attempt (overbooking):** If two clients simultaneously attempt to book the same room for overlapping times, the system rejects the transaction for the second client (enforced via time boundary checks at the backend/DBMS level).

---

## 5. Acceptance Criteria
- **SC-001**: All primary (`PK`) and foreign (`FK`) keys must be of type `int`.
- **SC-002**: The model is in 3NF: no repeating groups (e.g., arrays of instruments in a single column), no non-cacheable derived data, and no transitive dependencies.
- **SC-003**: Each relationship has an explicit cardinality, documented in the specification and reflected on the diagram.
- **SC-004**: The `Booking` M:N `Equipment` relationship is resolved exclusively through the `BookingEquipment` associative entity. There are no direct M:N relationships in the ER diagram.
- **SC-005**: Entity and attribute names in the ER diagram match `spec.md` character by character (case-sensitive).
- **SC-006**: Cardinalities in the Mermaid ER diagram exactly match the "Relationships" section in the specification.
