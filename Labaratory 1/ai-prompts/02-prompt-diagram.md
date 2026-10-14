# AI Prompt Log: ER Diagram

## Prompt Trail
1. "Я планую малювати ER-діаграму в Mermaid. Підкажи правильний декларативний синтаксис для позначення кардинальностей (наприклад, 1-до-багатьох, 0-до-багатьох) для моїх сутностей Booking та BookingEquipment."
2. "Дивлюся на згенерований зв'язок між Booking та BookingEquipment. Чому кардинальність з боку обладнання починається від нуля (0..*)? Чи не повинно бути хоча б однієї одиниці інвентарю в кожному бронюванні?"

## AI Output Audit
* **Mistake / Verification (Cardinality Check):** I had to critically evaluate why the AI suggested a `0..*` (zero to many) relationship between `Booking` and `BookingEquipment` instead of `1..*`.
  * *Correction/Validation:* The AI explained that equipment rental is optional (a band can bring their own gear). I validated that this aligns with the business logic and that the Mermaid syntax `||--o{` correctly represents this rule before adding it to README.md.
