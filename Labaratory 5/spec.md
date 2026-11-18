# 5. Architecture Model (Component Diagram)

## System Overview: Rehearsal Room Booking System
This section defines the high-level architecture, internal component boundaries, and external integrations required to support the workflows modeled in previous assignments.

### 1. System Components & Layers
The system follows a multi-tier architecture to separate concerns:
*   **Presentation Layer (UI):** 
    *   `WebClient`: The frontend application representing the `BookingUI`. It consumes backend APIs to display room availability and capture client requests.
*   **Business Logic Layer (Backend/API):** 
    *   `BookingController`: Exposes RESTful endpoints (Provided Interface) for the frontend.
    *   `BookingService`: The core orchestrator that handles business rules, availability checks, and price calculations.
*   **Data Access Layer (Persistence):** 
    *   `RoomRepository` & `BookingRepository`: Data Access Objects (DAOs) that abstract database operations.
*   **Database:** 
    *   `RelationalDatabase`: The physical or logical database hosting the ER model (Rooms, Bookings, Equipment).

### 2. External Integrations (Outside System Boundary)
*   **Payment Gateway:** An external financial API required to process transactions (`UC-05 Confirm Transaction`).
*   **Notification Provider:** An external SMS/Email service required to dispatch asynchronous alerts to the client.

### 3. Provided and Required Interfaces
*   **Provided Interfaces:** 
    *   `IBookingAPI`: Provided by the Backend, required by the WebClient to submit bookings.
*   **Required Interfaces:** 
    *   `IPaymentAPI`: Required by the Backend, provided by the Payment Gateway.
    *   `INotificationAPI`: Required by the Backend, provided by the Notification Provider.
    *   `IDatabaseConnection`: Provided by the Database, required by the Repositories.
