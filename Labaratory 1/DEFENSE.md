# Task 1: Domain and Data Model (ER)

## 1. Intent and Criteria
**Intent:** To design a logical data model for a Rehearsal Room Booking service that captures room reservations and the rental of additional musical equipment.
**Criteria:** The model is normalized to 3NF, has no implicit Many-to-Many (M:N) relationships, ensures the preservation of historical financial data, and the ER diagram perfectly matches the `spec.md`.

## 2. Top-3 Discrepancies (spec ↔ artifact) + Commit Fixes
During the design process, the following architectural flaws were identified and fixed:

1. **1NF Violation (Flag Anti-pattern in Equipment):**
   * *Was:* The `Equipment` entity stored boolean flag columns for specific instruments (`DrumSet_avail`, `Guitar_avail`), making the structure inflexible.
   * *Fixed:* Created a normalized inventory catalog with a `category` attribute.
   * *Commit:* `[insert_commit_hash_or_link_here]`

2. **Implicit Many-to-Many Relationship:**
   * *Was:* The booking directly referenced equipment, which made it impossible to rent multiple units of different inventory for a single session.
   * *Fixed:* Introduced the associative entity `BookingEquipment` to correctly resolve the M:N relationship and added a `quantity` field.
   * *Commit:* `[insert_commit_hash_or_link_here]`

3. **Loss of Historical Financial Data (Business Logic):**
   * *Was:* The equipment rental cost was taken directly from the `Equipment` catalog. A price change in the catalog would lead to the recalculation of past orders.
   * *Fixed:* Added the `hourly_booking_rate` attribute to the `BookingEquipment` associative entity, capturing a price snapshot at the time of the transaction.
   * *Commit:* `[insert_commit_hash_or_link_here]`

## 3. Key Decision (ADR)
How exactly we resolved the relationship between the rehearsal session and the equipment catalog while preserving pricing business rules is described in the ADR:
[ADR 0001: Resolving Many-to-Many Relationship Between Bookings and Equipment](adr/0001-booking-equipment-relationship.md)

## 4. Verification: Consistency
* Since this is the first model in the chain (data), consistency was checked between the text specification (`spec.md`) and the diagram code (`Mermaid`).
* **How it was checked:** A step-by-step verification of cardinalities was performed. Verified that the optionality of equipment rental is reflected as `0..*` (zero or many) on the `BookingEquipment` side, and all foreign keys (`FK`) have corresponding `PK`s in the parent tables. Attribute names in the diagram match the specification character by character.
