# AI Prompt Log: Lab 3

## Prompt Trail
1. "Я обрала `UC-02 Create Booking Request`. Ось список учасників (Client, BookingUI, BookingService, RoomRepository, BookingRepository). Перевір чи все відповідває ER-моделі з лаби 1. Допоможи почати зі сценаріїв 'успішне бронювання' та 'відмова'."
2. "Напиши код PlantUML для Sequence-діаграми. Мені потрібно, щоб в одній діаграмі був фрагмент `alt` (вільно/зайнято) і `opt` (додаткове обладнання). Activation bars не повинні обриватися всередині гілки `alt`."
3. "Склади таблицю аудиту, щоб порівняти кроки з Gherkin-сценаріїв (Lab 2) із викликами на моїй Sequence-діаграмі (Lab 3). Перевір, чи є точний збіг."

## AI Output Audit
- Mistake 1 (Architectural Gap): When generating the text scenarios, the AI suggested a flow where the `BookingUI` directly queried the database.
  - Correction: I realized this violates the multi-tier architecture. I instructed the AI to route all database interactions strictly through the `BookingService` controller.
- Mistake 2 (Activation Bar Syntax): In the PlantUML code, the AI placed the `deactivate Service` command inside the `alt` block, which breaks the continuous lifeline.
  - Correction: I prompted the AI to move the deactivation commands outside the conditional fragment to ensure the activation bar remains continuous.
- Verification (Gherkin to Sequence Audit): I had the AI map the Gherkin steps to the sequence diagram.
  - Validation: I manually verified the generated audit table to ensure that every `Given`, `When`, and `Then` exactly matched a specific diagram step (e.g., `checkAvailability`, `saveBooking`), without any hallucinated extra actions.
