## 6. Actors and Goals

To define the system boundaries and interaction workflows, there is two primary actors (one human and one external service):

## 1. Client (Human Actor)
A musician or band representative registered in the system.

**Primary Goals:** 
- Browse available rehearsal rooms and time slots.
- Create a booking for a specific room and time.
- Add additional equipment to the booking.
- Cancel an existing booking.

---

## 2. Payment Gateway (External System Actor)
An external API responsible for processing financial transactions.

**Primary Goals:** 
- Process the payment for a `PENDING` booking.
- Send a callback to the system to update the `booking_status` to `CONFIRMED`.
- Process refunds for `CANCELLED` bookings.
