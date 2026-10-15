# AI Prompt Log: spec.md

## Prompt Trail
3. "Я накидала список сутностей та атрибутів для домену rehearsal room booking: Client (client_id...), Room (room_id...), Booking (booking_id...), Equipment (DrumSet_avail, Guitar_avail...). Оціни та підкажи, якщо щось не так та розпиши на англійській про кожен атрибут та сутність? Чи немає тут порушення першої нормальної форми (1NF)?"
4. "Я зрозуміла проблему з неявним зв'язком M:N. Якщо я створю асоціативну сутність BookingEquipment, які саме ключі та атрибути мені варто туди додати?"
5. "Я пишу spec.md і мені потрібно сформулювати функціональні вимоги та граничні випадки (Edge cases) англійською. Які сценарії овербукінгу чи зміни цін мені варто врахувати?"

## AI Output Audit
* **Mistake 1 (1NF Violation):** The AI initially didn't heavily critique my draft with boolean flags for equipment (`DrumSet_avail`, `Guitar_avail`). 
  * *Correction:* I realized this violates 1NF and makes the schema inflexible. I instructed the AI to help me refactor `Equipment` into a proper catalog with a `category` field.
* **Mistake 2 (Missing M:N Resolution):** The initial discussion lacked a clear architectural solution to link a booking to multiple equipment items properly.
  * *Correction:* I identified the need for an associative entity and prompted the AI to help structure the `BookingEquipment` table with a `quantity` and price snapshot field.
