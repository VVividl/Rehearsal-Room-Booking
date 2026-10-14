# AI Prompt Log: ADR

## Prompt Trail

1. "Я маю зв'язок Many-to-Many між сутностями Booking та Equipment. Я знаю, що потрібна проміжна таблиця, але які атрибути вона обов'язково повинна мати, окрім двох зовнішніх ключів (FK)? Які є best practices?"

2. "Якщо я просто зв'яжу їх через FK, виникне проблема, бо якщо адміністратор змінить ціну оренди гітари в таблиці Equipment наступного місяця, чи не перерахується вартість усіх старих бронювань? Як це вирішити на рівні бази даних?"

3. "Я додам поле `hourly_booking_rate` безпосередньо в асоціативну сутність, щоб робити 'знімок' ціни. Тепер мені треба задокументувати це рішення. Надай мені порожній шаблон формату MADR (Markdown Any Decision Record)."

4. "Я пишу розділ 'Considered Options' для ADR. Я хочу додати 'Збереження масиву equipment_ids у таблиці Booking' як одну з альтернатив, щоб показати, чому ми її відхилили. Як правильно і формально описати англійською, що це порушує 1-шу нормальну форму (1NF)?"

5. "Ось мій фінальний драфт ADR. Перевір, чи чітко все аргументовано."

6. "Допоможи перекласти цей фінальний варіант ADR повністю англійською мовою."

## AI Output Audit

* **Mistake / Deepening the Context (Handling Price History):** 
  * *What happened:* In response to the first prompt, the AI only suggested standard attributes like `quantity` and `created_at` for the bridge table. It completely missed the domain-specific financial risk of price changes.
  * *Correction:* I had to independently identify the historical pricing issue and challenge the AI (Prompt 2). Only then did it acknowledge the problem. I ultimately made the decision to use a snapshot field (`hourly_booking_rate`) and instructed the AI to follow my architectural choice (Prompt 3).
* **Refining Technical Vocabulary and Translation:**
  * *What happened:* When the AI initially tried to explain why saving an array of IDs is bad, it used informal language ("it's hard to search" or "it's not good for SQL"). 
  * *Correction:* I had to explicitly ask the AI to formalize the explanation (Prompt 4) to ensure it mentioned the violation of the First Normal Form (1NF) and atomic values. I also supervised the final translation (Prompt 6) to guarantee the ADR met academic engineering standards.
