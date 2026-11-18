# AI Prompt Log: Lab 5

## Prompt Trail
1. "Ось моя специфікація модулів для `spec.md` (Presentation, Business Logic, Data Access, Database та зовнішні сервіси Payment Gateway і Notification Provider). Перевір, чи все узгоджується з Лабами 1–4, та згенеруй код PlantUML для Component-діаграми з чіткою межею системи та provided/required-інтерфейсами."
2. "Зроби діаграму більш читабельною у класичному шаровому стилі UML, щоб вона точно відповідала моєму `spec.md` без зайвих елементів."
3. "Перевір мій рендер діаграми на помилки та допоможи оформити `DEFENSE.md` англійською рівня B2."

## AI Output Audit
- Mistake 1 (Extra Unspecified Interfaces & Cluttered Layout): In one of the intermediate drafts, the AI added extra internal interfaces and complex port syntax that were not in my `spec.md` and caused messy rendering.
  - Correction: I decided to keep a clean vertical layered structure with a clear `<<system>>` boundary and `External Systems` cloud, strictly matching the four main interfaces defined in `spec.md` (`IBookingAPI`, `IDatabaseConnection`, `IPaymentAPI`, `INotificationAPI`).
- Mistake 2 (Swapped Repository Labels): When reviewing the generated PlantUML render, the `saves booking` label was placed on the arrow to `RoomRepository`, and `checks availability` on `BookingRepository`.
  - Correction: I noticed the mismatch during visual verification and fixed the arrow labels so `BookingService` correctly checks availability via `RoomRepository` and saves bookings via `BookingRepository`.
- Verification (End-to-End Traceability): I manually verified that every component and interface on the final diagram maps directly to previous labs—`RelationalDatabase` to Lab 1 (ER), `Payment Gateway` to Lab 2 (`UC-05`), `WebClient`/`Repositories` to Lab 3 (Sequence), and `Notification Provider` to Lab 4 (Activity).
