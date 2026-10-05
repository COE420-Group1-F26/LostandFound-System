# Level-0 DFD — Processes, Data Stores and Data Flows

The Level-0 DFD breaks process 0 from the Context Diagram into six processes. It uses the same four external entities and the same 26 entity data flows as the Context Diagram, so the two diagrams are balanced.

## Processes

| No. | Process Name | Use Cases Covered | Functional Requirements Covered |
| --- | --- | --- | --- |
| P1 | Authenticate Users | UC-01 | FR-01 |
| P2 | Manage Lost Item Reports | UC-02 | FR-02 |
| P3 | Manage Found Item Reports | UC-06, UC-07, UC-08, UC-10 | FR-06 to FR-08, FR-10 |
| P4 | Search Found Items | UC-03 | FR-03 |
| P5 | Match Items and Notify Owners | UC-04, UC-09 | FR-04, FR-09 |
| P6 | Process and Decide Claims | UC-05, UC-11, UC-12, UC-13, UC-14, UC-15 | FR-05, FR-11 to FR-15 |

## Data stores

| No. | Data Store | What it holds | Written by | Read by |
| --- | --- | --- | --- | --- |
| D1 | User Accounts | Registered AUS accounts: email, password hash, AUS ID | P1 | P1 |
| D2 | Lost Item Reports | Every lost item report with its category, description, location, date, photo, private identifying details and the owner's contact email | P2 | P5 |
| D3 | Found Item Reports | Every found item record with its details, photo, status (listed, claimed, released, withdrawn) | P3, P6 | P3, P4, P5, P6 |
| D4 | Match Suggestions | Possible lost/found pairs produced by the matching process, with a match score | P5 | P5 |
| D5 | Claims | Claim requests, their verification details, decisions, dispute outcomes and decision history | P6 | P6 |

## Data flows

| Flow Name | From | To |
| --- | --- | --- |
| Login Credentials | Item Owner | P1 Authenticate Users |
| Login Credentials | Finder | P1 Authenticate Users |
| Login Credentials | Campus Security Officer | P1 Authenticate Users |
| Login Result | P1 Authenticate Users | Item Owner |
| Login Result | P1 Authenticate Users | Finder |
| Login Result | P1 Authenticate Users | Campus Security Officer |
| Account Record | D1 User Accounts | P1 Authenticate Users |
| New Account Record | P1 Authenticate Users | D1 User Accounts |
| Lost Item Report | Item Owner | P2 Manage Lost Item Reports |
| Lost Report Confirmation | P2 Manage Lost Item Reports | Item Owner |
| Lost Report Record | P2 Manage Lost Item Reports | D2 Lost Item Reports |
| Found Item Report | Finder | P3 Manage Found Item Reports |
| Found Report Confirmation | P3 Manage Found Item Reports | Finder |
| Item Status | P3 Manage Found Item Reports | Finder |
| Report Update Request | Finder | P3 Manage Found Item Reports |
| Status Request | Finder | P3 Manage Found Item Reports |
| Found Item Record | P3 Manage Found Item Reports | D3 Found Item Reports |
| Stored Found Report | D3 Found Item Reports | P3 Manage Found Item Reports |
| Search Request | Item Owner | P4 Search Found Items |
| Search Results | P4 Search Found Items | Item Owner |
| Found Item Listings | D3 Found Item Reports | P4 Search Found Items |
| Delivery Status | Email / Notification Service | P5 Match Items and Notify Owners |
| Match Details | P5 Match Items and Notify Owners | Item Owner |
| Match Notification Request | P5 Match Items and Notify Owners | Email / Notification Service |
| Match Suggestions | P5 Match Items and Notify Owners | Finder |
| Match View Request | Item Owner | P5 Match Items and Notify Owners |
| Found Item Data | D3 Found Item Reports | P5 Match Items and Notify Owners |
| Lost Report Data | D2 Lost Item Reports | P5 Match Items and Notify Owners |
| Match Suggestion Record | P5 Match Items and Notify Owners | D4 Match Suggestions |
| Stored Matches | D4 Match Suggestions | P5 Match Items and Notify Owners |
| Claim Decision | Campus Security Officer | P6 Process and Decide Claims |
| Claim Details | P6 Process and Decide Claims | Campus Security Officer |
| Claim History Report | P6 Process and Decide Claims | Campus Security Officer |
| Claim Query | Campus Security Officer | P6 Process and Decide Claims |
| Claim Request | Item Owner | P6 Process and Decide Claims |
| Claim Status | P6 Process and Decide Claims | Item Owner |
| Claim Record | P6 Process and Decide Claims | D5 Claims |
| Item Details | D3 Found Item Reports | P6 Process and Decide Claims |
| Item Status Update | P6 Process and Decide Claims | D3 Found Item Reports |
| Stored Claims | D5 Claims | P6 Process and Decide Claims |

Totals: 6 processes, 5 data stores, 40 data flows (26 with external entities, 14 with data stores).
