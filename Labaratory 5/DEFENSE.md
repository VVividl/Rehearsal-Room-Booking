# Task 5: Architecture (Component)

**Intent and criteria:**
The goal of this task is to define the high-level architecture of the Rehearsal Room Booking System using a UML Component Diagram. It maps out internal layers, clear system boundaries, external integrations, and provided/required interfaces to connect all previous models into one cohesive system.

**Top-3 discrepancies (spec ↔ artifact) + fix commits:**
1. **Swapped repository labels:** During the initial diagram render, PlantUML misplaced the `saves booking` label onto `RoomRepository` and `checks availability` onto `BookingRepository`. *(Fix commit: corrected arrow labels so `BookingService` checks availability via `RoomRepository` and saves records via `BookingRepository`).*
2. **Unclear system boundary:** In the first draft, external services (`Payment Gateway` and `Notification Provider`) were not visually separated from internal backend modules. *(Fix commit: wrapped all internal layers inside the `Rehearsal Room Booking System` `<<system>>` boundary and moved external integrations into the `External Systems` cloud).*
3. **Missing interface explicitness:** Initially, components were linked with plain arrows instead of showing provided and required contracts. *(Fix commit: added explicit `IBookingAPI`, `IDatabaseConnection`, `IPaymentAPI`, and `INotificationAPI` interfaces using lollipop and `<<uses>>` dependency notation).*

**Key decision — what alternatives were considered and why this one was chosen (ADR):**
* **Decision:** Adopted a multi-tier (layered) architecture with a dedicated Data Access Layer (`RoomRepository`, `BookingRepository`) and interface-based external integrations.
* **Alternatives:** A 2-tier architecture (where `WebClient` connects directly to the database) or a Microservices architecture.
* **Reasoning:** A 2-tier setup violates separation of concerns and creates security risks. Microservices would add unnecessary operational complexity for a booking system of this scale. A layered architecture with clear interfaces (`IPaymentAPI`, `INotificationAPI`) keeps the system modular—allowing us to replace external payment or SMS providers later without rewriting `BookingService`.

**Verification — consistency with the entire chain of models (how I checked it):**
* **Lab 1 (ER Model):** `RelationalDatabase` stores the `Rooms`, `Bookings`, and `Equipment` tables, accessed strictly through `RoomRepository` and `BookingRepository` via `IDatabaseConnection`.
* **Lab 2 (Use Cases & Gherkin):** `BookingService` executes the core logic for `UC-02 Create Booking Request`, while `Payment Gateway` supports `UC-05 Confirm Transaction`.
* **Lab 3 (Sequence Diagram):** `WebClient` (`BookingUI`), `BookingService`, `RoomRepository`, and `BookingRepository` match the exact participants and interaction flow from the sequence diagrams.
* **Lab 4 (Activity Diagram):** The external `Notification Provider` connected via `INotificationAPI` (`Async Event`) directly corresponds to the `Notification Service` swimlane and the asynchronous confirmation signal.
