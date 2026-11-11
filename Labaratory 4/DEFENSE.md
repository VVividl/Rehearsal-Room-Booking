# Task 4: Process / Behavior

**Intent and criteria:**
The goal of this task is to model the business process of handling a booking request (`UC-02`) using an Activity Diagram. The main focus is on utilizing swimlanes, parallel branches (fork/join) for concurrent operations, and asynchronous message passing between participants.

**Top-3 discrepancies (spec ↔ artifact) + fix commits:**
1. **Message synchronization:** Initially, the specification assumed a standard synchronous call to the notification service. *Fix commit:* Changed this to asynchronous signals (Send Signal `>` / Accept Event `<`) on the diagram so the client's process is not blocked waiting for SMS delivery.
2. **Missing final states in alternative branches:** In the first draft, the room unavailability error simply dropped the process. *Fix commit:* Added an explicit termination node (`stop`) after displaying the "Room is occupied" message to avoid unreachable states and dead ends.
3. **Merging parallel processes:** The spec originally placed equipment reservation sequentially after locking the room. *Fix commit:* Moved these actions into parallel branches (`fork` / `fork again`) that synchronize via `join` (`end fork`) before creating the final booking record.

**Key decision — what alternatives were considered and why this one was chosen (ADR):**
* **Decision:** Selected the UML Activity Diagram format using PlantUML.
* **Alternatives:** State Machine or BPMN 2.0.
* **Reasoning:** A State Machine is perfect for tracking the lifecycle of a single entity (like a booking status) but does not natively support modeling parallel processes (fork/join) across different participants. BPMN requires external editors and is harder to render cleanly in GitHub Markdown. The Activity Diagram allowed me to clearly separate roles (Client, Booking System, Notification Service) and fulfill the mandatory message event requirement. Because the signal is asynchronous, the main process still finishes successfully for the Client even without a direct response from the Notification Service.

**Verification — consistency with the previous model (how I checked it):**
* **Role consistency:** The `Client` and `Booking System` participants perfectly match the actors and controllers from the Sequence Diagram in Lab 3.
* **Logic consistency:** The `if (Is slot available?)` branching is the direct equivalent of the `alt` fragment from Lab 3 and the alternative Gherkin scenario from Lab 2.
* **Transition completeness:** I verified the diagram graph to ensure every path logically ends with a terminal node (`stop`), confirming there are no transition "holes" or unreachable states.
