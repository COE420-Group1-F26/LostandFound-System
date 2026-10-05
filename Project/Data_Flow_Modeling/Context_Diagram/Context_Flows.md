# Context Diagram — Entities and Data Flows

The whole Lost and Found Property Claim System is shown as one process (0). Each external entity is an actor from the Lab 3 use case diagram.

## External entities

| External Entity | UML Actor (Lab 3) | Why it is outside the system |
| --- | --- | --- |
| Item Owner | Item Owner | A student or staff member who lost an item. They give the system reports, searches and claims and receive results, but are not part of the software. |
| Finder | Finder | A student or staff member who hands in a found item. They supply found item reports and receive confirmations and status, from outside the system. |
| Campus Security Officer | Campus Security Officer | University security staff who review and decide claims. They use the system but are not part of it. |
| Email / Notification Service | Email / Notification Service | An external email service. The system sends it notification requests; it delivers the emails and reports back. |

## Data flows

| Flow Name | From | To | Use Cases | Description |
| --- | --- | --- | --- | --- |
| Login Credentials | Item Owner | System | UC-01 | AUS email and password (and registration details for a first-time user) |
| Lost Item Report | Item Owner | System | UC-02 | Category, description, location lost, date lost, optional photo, optional private identifying details; sent from the owner's account, so it carries their contact email |
| Search Request | Item Owner | System | UC-03 | Keyword and filters: category, location found, date range |
| Match View Request | Item Owner | System | UC-04 | Request to open a match notification and view the matched item |
| Claim Request | Item Owner | System | UC-05 | AUS ID and identifying details that are not shown in the public listing |
| Login Result | System | Item Owner | UC-01 | Access granted or denied, with a reason if denied |
| Lost Report Confirmation | System | Item Owner | UC-02 | Confirmation and reference number of the saved lost item report |
| Search Results | System | Item Owner | UC-03 | Matching found items, newest first |
| Match Details | System | Item Owner | UC-04 | Details and photo of the possible match |
| Claim Status | System | Item Owner | UC-05, UC-12, UC-13, UC-14 | Claim status: pending verification, approved, rejected or disputed |
| Login Credentials | Finder | System | UC-01 | AUS email and password |
| Found Item Report | Finder | System | UC-06, UC-07 | Category, description, location found, date found, optional photo |
| Report Update Request | Finder | System | UC-08 | Edit to, or withdrawal of, an existing found item report |
| Status Request | Finder | System | UC-10 | Request for the current status of a handed-in item |
| Login Result | System | Finder | UC-01 | Access granted or denied, with a reason if denied |
| Found Report Confirmation | System | Finder | UC-06, UC-07, UC-08 | Confirmation that the found item report was saved, changed or withdrawn |
| Match Suggestions | System | Finder | UC-09 | Lost item reports that may match the found item |
| Item Status | System | Finder | UC-10 | Current status of the handed-in item |
| Login Credentials | Campus Security Officer | System | UC-01 | Security account email and password |
| Claim Query | Campus Security Officer | System | UC-11, UC-15 | Request to see pending claims or the claim history |
| Claim Decision | Campus Security Officer | System | UC-12, UC-13, UC-14 | Approve or reject a claim, or the outcome of a disputed claim, with notes |
| Login Result | System | Campus Security Officer | UC-01 | Access granted or denied, with a reason if denied |
| Claim Details | System | Campus Security Officer | UC-11, UC-12, UC-13, UC-14 | Pending claims with the claimant's AUS ID, the details given and the item's private details for checking |
| Claim History Report | System | Campus Security Officer | UC-15 | Record of past claims, decisions and who released each item |
| Match Notification Request | System | Email / Notification Service | UC-04, UC-09 | Owner's contact email (taken from the lost item report) and the match summary to send as an email |
| Delivery Status | Email / Notification Service | System | UC-04, UC-09 | Whether the notification was delivered or failed |

Totals: 26 data flows (13 into the system, 13 out of the system). No internal processes or data stores appear on this diagram.
