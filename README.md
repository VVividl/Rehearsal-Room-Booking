<img width="1281" height="239" alt="rehearsal-room-booking" src="https://github.com/user-attachments/assets/96b9e0ed-1a48-4e7c-b13c-6f08887207c6" />

# Rehearsal Room Booking

A project which contains laboratory works for the discipline "Software Engineering Components (Part 2: Software Requirements Modeling and Analysis)", Igor Sikorsky Kyiv Polytechnic Institute, 2026.

![](https://img.shields.io/badge/DIAGRAMS-MERMAID-482B42?style=for-the-badge)
![](https://img.shields.io/badge/DOCS-MARKDOWN-black?style=for-the-badge)
![](https://img.shields.io/badge/STATUS-IN_PROGRESS-23342C?style=for-the-badge)

---

## About the Project

**Rehearsal Room Booking** is a service for reserving studio spaces and renting musical equipment. The Client selects a rehearsal room for a specific time interval, adds optional inventory (Equipment), and finalizes the reservation (Booking). The system ensures fair pricing by locking rates at the time of booking, prevents time-slot overlaps, and tracks reservation statuses.

## Roles and Entities

| Entity / Role | Purpose / Action |
| :--- | :--- |
| **Client** | Registers, browses room availability, creates and manages bookings. |
| **Room** | A rehearsal space featuring a defined capacity and a base hourly rate. |
| **Booking** | Represents the reservation of a room for a specific time frame, including total price and status. |
| **Equipment** | Additional musical backline (instruments, microphones, amplifiers) available for an hourly fee. |
| **BookingEquipment** | An associative entity that tracks rented equipment for a specific booking and locks in the hourly rate at the time of order. |

## How it Works

1. The **Client** browses the catalog of available rehearsal rooms (`Room`) and checks for free time slots.
2. The Client selects the desired room, date, and time boundaries (`start_datetime`, `end_datetime`).
3. If necessary, the Client adds optional gear from the `Equipment` catalog to their session.
4. The system calculates the `total_price` and saves the current rental rates into the `BookingEquipment` records to prevent future price changes from affecting this order.
5. A `Booking` is created with a `PENDING` status, which updates to `CONFIRMED` once approved or paid.
