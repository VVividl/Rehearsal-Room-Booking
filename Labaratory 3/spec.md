# 3. Sequence Diagram Specifications

## Use Case: UC-02 Create Booking Request
This section outlines the internal system interactions required to complete the booking process.

### Participants
The participants are mapped directly to the database entities from the Lab 1 ER model and the actors from Lab 2:
1. **Client** (Actor): The registered user who initiates the booking.
2. **BookingUI** (Boundary): The user interface (frontend) where the client enters their request.
3. **BookingService** (Control): The main backend controller that processes the business logic.
4. **RoomRepository** (Entity): The database access layer responsible for querying the `Room` table to verify availability.
5. **BookingRepository** (Entity): The database access layer that handles saving records to the `Booking` and `BookingEquipment` tables.

### Scenario 1: Successful Booking with Optional Equipment
1. The Client submits the booking details (roomId, date, time) through the BookingUI.
2. The BookingUI forwards the data to the BookingService.
3. The BookingService calls the RoomRepository to verify if the room is available for the selected timeslot.
4. The RoomRepository confirms availability (returns `true`).
5. **[OPTIONAL]** If the client requested additional gear, the BookingService saves the equipment data in the BookingRepository.
6. The BookingService saves the final booking record in the BookingRepository.
7. The BookingService sends a success response back to the BookingUI.
8. The BookingUI displays a confirmation message to the Client.

### Scenario 2: Room is Already Booked (Alternative Flow)
1. The Client submits the booking details through the BookingUI.
2. The BookingUI forwards the request to the BookingService.
3. The BookingService asks the RoomRepository to verify availability.
4. The RoomRepository reports that the room is occupied (returns `false`).
5. The BookingService immediately throws an error exception (`RoomNotAvailableError`) back to the BookingUI.
6. The BookingUI displays an error message, prompting the Client to select a different time.
