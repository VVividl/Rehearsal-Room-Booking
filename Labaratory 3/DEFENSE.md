# Task 3: Interaction (Sequence)

**Intent and criteria:**
The main goal of this task is to show how the system works inside for `UC-02 Create Booking Request`. It shows how the frontend, backend, and database communicate using `alt` and `opt` blocks.

**Top-3 differences (spec ↔ artifact) + fix commits:**
1. **Missing layers:** In Lab 2, we only had the "Client" actor. For this diagram, I added `BookingUI` and `BookingService` to separate the frontend from the backend. *(Fix commit: added UI and Controller lifelines).*
2. **Direct database connection:** At first, the `BookingUI` tried to talk to `RoomRepository` directly. This is bad architecture. *(Fix commit: routed all database requests through the `BookingService`).*
3. **Broken activation bars:** The `BookingService` activation bar ended inside the `alt` block by mistake. *(Fix commit: moved the `deactivate` commands outside the conditional block).*

**Key decision — what alternatives were considered and why this one was chosen (ADR):**
* **Decision:** The backend (`BookingService`) checks room availability again after the user clicks "Submit". 
* **Alternatives:** Trusting only the frontend (just showing free slots on the UI calendar and booking without a backend check).
* **Reasoning:** This prevents "race conditions". If two users try to book the same room at the exact same time, the strict backend check ensures only the first one succeeds (Scenario 1), and the second user gets an error (Scenario 2).

**Verification — consistency with the previous model (how I checked it):**
I checked the diagram participants against the ER model from Lab 1. `RoomRepository` and `BookingRepository` perfectly match the `Room`, `Booking`, and `BookingEquipment` tables. Also, the `alt` and `opt` blocks match the Gherkin scenarios I wrote in Lab 2.
