# 4. Business Process Specification (Activity Model)

## Process Overview: Booking Request Processing and Confirmation
This document details the business process for `UC-02 Create Booking Request`, building directly on the ER data model from Lab 1, the Use Case specifications from Lab 2, and the execution flows modeled in Lab 3.

---

### 1. Selected Notation & Architectural Rationale
* **Selected Notation:** UML Activity Diagram (implemented via PlantUML).
* **Rationale for Selection:** 
  * Unlike a State Machine, which focuses solely on the lifecycle and state transitions of a single entity (such as `BookingStatus`), an Activity Diagram naturally models coordinated workflows across multiple system participants.
  * It provides explicit syntax for concurrent execution branches (`fork` and `join` nodes), which are required to represent parallel operations such as database record creation alongside asynchronous event dispatching.
  * It maintains clear operational boundaries across domains using swimlanes (`Client`, `Booking System`, and `Notification Service`), preventing architectural ambiguity.

---

### 2. Participants and Operational Roles (Swimlanes)
* **Client (User Role):** 
  * Initiates the booking flow by submitting input parameters: room ID, rehearsal date, time interval, and optional equipment selection.
  * Receives immediate UI feedback: either an error alert or a successful booking confirmation.
* **Booking System (Application Backend):** 
  * Acts as the central controller handling domain logic.
  * Interacts with `RoomRepository` to verify timeslot availability.
  * Performs concurrent execution of database lock and pricing/equipment calculations.
  * Emits outbound notification signals upon finalizing records in `BookingRepository`.
* **Notification Service (External/Background Worker):** 
  * Listens for published booking confirmation events.
  * Asynchronously compiles and delivers transaction alerts (email and SMS) to the client without blocking the core booking pipeline.

---

### 3. Workflow Steps and Decision Logic

1. **Initiation:** The Client enters booking details into the UI and triggers the `submitBooking` action.
2. **Availability Check:** The Booking System queries the persistent storage layer (`RoomRepository`) to determine whether the requested room is available for the given timeslot.
3. **Branching (Decision Node):**
   * **Alternative Path `[Slot is occupied]`:** 
     * The system halts reservation processing immediately.
     * The UI outputs a `RoomNotAvailableError` alert.
     * The process terminates at an activity final node without persisting any state changes.
   * **Primary Path `[Slot is available]`:** 
     * The flow enters a `fork` node to initiate two parallel operations:
       * **Branch A (Persistence):** Locks the designated rehearsal room timeslot to avoid race conditions.
       * **Branch B (Calculation & Add-ons):** Calculates final charges and reserves selected auxiliary instruments (`BookingEquipment`).
     * A `join` node synchronizes both threads before the primary `Booking` entity is committed with the status `Confirmed`.

---

### 4. Inter-System Messaging and Signal Handling
* **Outbound Message (Send Signal):** Immediately following the commit, the Booking System publishes an asynchronous event: `Send Booking Confirmation Event`.
* **Inbound Handling (Accept Event):** The Notification Service intercepts this event (`Accept Notification Event`) and triggers external communication channels to deliver confirmation details to the Client.
* **Non-Blocking Execution:** The core booking pipeline does not stall waiting for delivery receipts; the Client receives on-screen UI confirmation immediately, while the external message is processed independently in the background.
