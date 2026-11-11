
**AI Prompt Log: Lab 4 - Process / Behavior (Activity Diagram)**

**Prompt Trail**
1. "Ось моя заготовка для `spec.md`: ролі (Client, Booking System, Notification Service) та базова логіка з розгалуженням, чи всі вимоги завдання враховані і що треба дописати? Також згенеруй PlantUML код для Activity діаграми. нехай буде тільки це"

**AI Output Audit**
- Mistake 1 (Missing Mandatory Elements): When analyzing the initial draft, the AI generated a standard sequential flowchart that lacked parallel execution and asynchronous messaging.
  - Correction: I identified that this did not fulfill the core assignment criteria. I guided the AI to incorporate a `fork/join` block for concurrent tasks (database locking and equipment calculation) and to use asynchronous signal notation (`Send Signal >` / `Accept Event <`) for the notification service.
- Mistake 2 (Unreachable State): The AI's first PlantUML draft left the alternative branch (when the room is occupied) hanging without a proper termination point.
  - Correction: I prompted the AI to add an explicit `stop` node after the error message to ensure the state graph was completely closed.
