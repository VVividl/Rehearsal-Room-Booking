# DEFENSE - Task 2: Requirements and Use Cases

**Intent and criteria:**
The main goal of this assignment is to formalize the behavior of the Rehearsal Room Booking system using EARS requirements, User Stories, and Gherkin scenarios. It also aims to visualize actor interactions through a Use Case diagram with complex traceability links.

**Top-3 discrepancies (spec ↔ artifact) + fix commits:**
1. **External system in UML:** The ER model (Lab 1) did not have a `Payment Gateway` entity. However, for a complete Use Case model (Lab 2), I had to introduce this external actor because this system does not process payments locally.
2. **Equipment optionality:** In the database, `BookingEquipment` is a separate table. In the UML diagram, I represented this as a behavioral `<<extend>>` relationship (Rent Additional Equipment) to show that this action is optional for the client.
3. **Uncovered requirement:** Requirement REQ-05 (Cancel Booking) exists in EARS and UML (UC-06), but I intentionally left it without a Gherkin scenario. This was done to demonstrate a traceability gap in the matrix.

**Key decision - what alternatives were considered and why this one was chosen (ADR):**
**Decision:** Using the `<<extend>>` relationship for equipment rental (UC-03) instead of `<<include>>` or an independent use case.
**Alternatives:** 
* Making rental an independent Use Case (incorrect, because a client cannot rent drum cymbals without booking a room first).
* Using `<<include>>` (incorrect, because it would force every client to rent extra equipment).
**Reasoning:** `<<extend>>` perfectly reflects the business logic of a rehearsal studio. Equipment is added optionally, only if the client requests it, extending the basic booking process.

**Verification - consistency with the previous model (how I checked it):**
I cross-checked the database entities from Lab 1 with the use cases in Lab 2. The `Client` actor fully matches the `Client` table in the database. The `Rent Additional Equipment` use case correctly maps to the intermediate `BookingEquipment` table, and the basic booking process maintains its 1:M relationship with the `Room` and `Booking` tables.
