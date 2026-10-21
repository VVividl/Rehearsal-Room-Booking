# AI Prompt Log: Lab 2

## Prompt Trail
1. "Я накидала EARS-вимоги, User Stories та Gherkin для системи Rehearsal Room Booking, а також список Use Cases. Перевір їх на логічну узгодженість та підкажи, чи правильно актори співпадають з ER-моделлю з першої лаби?"
2. "Я планую малювати Use Case діаграму в PlantUML для імпорту в Draw.io. Підкажи правильний декларативний синтаксис для позначення стереотипів (наприклад, `<<include>>` та `<<extend>>`) і зв'язків узагальнення (generalization) для моїх акторів."
3. "Дивлюся на згенерований зв'язок між базовим бронюванням (Create Booking Request) та орендою обладнання (Rent Additional Equipment). Який саме стереотип краще застосувати, щоб правильно відобразити опціональність цієї дії?"
4. "Ось матриця трасування підкажи якщо щось не правильно та зроби аудит матриці, щоб переконатися, що M:N зв'язки валідні."

## AI Output Audit
- Mistake 1 (Actor Scope Creep): When reviewing the requirements, the AI initially suggested adding extra actors like `VIP Client` and `Studio Admin` to enrich the model.
  - Correction: I realized this violates the established system boundaries from Lab 1. I instructed the AI to strictly limit the actors to the ones I originally defined (`Client`, `Guest`, `Payment Gateway`) to maintain consistency.
- Mistake / Verification (Relationship Check): I had to critically evaluate why the AI suggested an `<<extend>>` relationship between 'Create Booking Request' and 'Rent Additional Equipment' instead of `<<include>>` in the PlantUML diagram.
  - Correction/Validation: The AI explained that equipment rental is an optional behavior (a band can bring their own gear and just rent the room). I validated that this aligns with the business logic and that the PlantUML syntax `.> : <<extend>>` correctly represents this rule.
- Mistake 3 (Traceability Matrix Simplification): When auditing the traceability matrix, the AI initially drafted trivial 1:1 mappings (e.g., mapping a requirement to only one use case), ignoring my complex structure.
  - Correction: I realized this fails the assignment's explicit requirement for Many-to-Many (M:N) relationships. I instructed the AI to restore my original M:N mappings and validate the intentional "uncovered gap" for the cancellation requirement.
