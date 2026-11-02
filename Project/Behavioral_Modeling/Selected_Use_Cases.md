# Selected Use Cases for Sequence Modeling

Three major use cases were selected from Project/Requirements/Use_Cases.md. Each one comes from a different part of the system (search, matching and notification, claims), each is supported by the classes in the Lab 5 class diagram, and each includes repeated and conditional steps.

| Use Case ID | Use Case Name | Primary Actor | Purpose | Reason for Selection |
| --- | --- | --- | --- | --- |
| UC-03 | Search Found Items | Item Owner | The owner searches and filters the found items to check whether their item has been handed in. | The main way an owner finds their item (FR-03). The search checks every found report, skips withdrawn ones and orders the results, so it shows iteration and a guard. Diagram drawn by Member 1. |
| UC-09 | Generate Match Suggestions | Finder (secondary: Email / Notification Service) | The system compares a newly submitted found report with the lost reports, stores ranked match suggestions and emails the owner of each matching lost report. | The core automatic feature of the system (FR-04, FR-09). It involves the matching control object, four entity classes and the external email service, with iterations over the lost reports and the suggestions and a minimum-score guard. Diagram drawn by Member 2. |
| UC-14 | Resolve Disputed Claim | Campus Security Officer | The officer compares the competing claims for the same found item, approves the rightful owner’s claim and rejects all the others. | Covers the claim process end to end: it includes UC-11, UC-12 and UC-13 (FR-11 to FR-13, FR-16), records the claim history (FR-15) and changes the item status. Diagram drawn by Member 3. |
