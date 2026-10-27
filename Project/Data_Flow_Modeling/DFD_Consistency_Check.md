# DFD Consistency and Balancing Check

## 1. External entities vs UML Use Case actors

| Context Diagram Entity | UML Actor (Lab 3) | Shown in Level-0? |
| --- | --- | --- |
| Item Owner | Item Owner | Yes |
| Finder | Finder | Yes |
| Campus Security Officer | Campus Security Officer | Yes |
| Email / Notification Service | Email / Notification Service | Yes |

All four actors in the Lab 3 use case diagram appear as external entities, and there are no other entities. Stakeholders who never exchange data with the system (university administration, IT, instructors) are not entities.

## 2. Balancing: Context Diagram flows vs Level-0 flows

| Context Flow | Entity | Direction | Level-0 Process it enters or leaves | Balanced? |
| --- | --- | --- | --- | --- |
| Login Credentials | Item Owner | Into the system | P1 Authenticate Users | Yes |
| Lost Item Report | Item Owner | Into the system | P2 Manage Lost Item Reports | Yes |
| Search Request | Item Owner | Into the system | P4 Search Found Items | Yes |
| Match View Request | Item Owner | Into the system | P5 Match Items and Notify Owners | Yes |
| Claim Request | Item Owner | Into the system | P6 Process and Decide Claims | Yes |
| Login Result | Item Owner | Out of the system | P1 Authenticate Users | Yes |
| Lost Report Confirmation | Item Owner | Out of the system | P2 Manage Lost Item Reports | Yes |
| Search Results | Item Owner | Out of the system | P4 Search Found Items | Yes |
| Match Details | Item Owner | Out of the system | P5 Match Items and Notify Owners | Yes |
| Claim Status | Item Owner | Out of the system | P6 Process and Decide Claims | Yes |
| Login Credentials | Finder | Into the system | P1 Authenticate Users | Yes |
| Found Item Report | Finder | Into the system | P3 Manage Found Item Reports | Yes |
| Report Update Request | Finder | Into the system | P3 Manage Found Item Reports | Yes |
| Status Request | Finder | Into the system | P3 Manage Found Item Reports | Yes |
| Login Result | Finder | Out of the system | P1 Authenticate Users | Yes |
| Found Report Confirmation | Finder | Out of the system | P3 Manage Found Item Reports | Yes |
| Match Suggestions | Finder | Out of the system | P5 Match Items and Notify Owners | Yes |
| Item Status | Finder | Out of the system | P3 Manage Found Item Reports | Yes |
| Login Credentials | Campus Security Officer | Into the system | P1 Authenticate Users | Yes |
| Claim Query | Campus Security Officer | Into the system | P6 Process and Decide Claims | Yes |
| Claim Decision | Campus Security Officer | Into the system | P6 Process and Decide Claims | Yes |
| Login Result | Campus Security Officer | Out of the system | P1 Authenticate Users | Yes |
| Claim Details | Campus Security Officer | Out of the system | P6 Process and Decide Claims | Yes |
| Claim History Report | Campus Security Officer | Out of the system | P6 Process and Decide Claims | Yes |
| Match Notification Request | Email / Notification Service | Out of the system | P5 Match Items and Notify Owners | Yes |
| Delivery Status | Email / Notification Service | Into the system | P5 Match Items and Notify Owners | Yes |

All 26 Context Diagram flows appear in the Level-0 DFD on the same entity, with the same name and direction. The Level-0 DFD adds only internal flows between processes and data stores (14 flows), which do not appear at context level.

## 3. Level-0 requirements

| Check | Result |
| --- | --- |
| At least 5 meaningful processes | Yes — 6 processes (P1–P6), each covering at least one use case |
| At least 2 data stores | Yes — 5 data stores (D1–D5) |
| Every process has an input and an output | Yes |
| No entity-to-store or store-to-store flows | Yes — every data store is connected only to processes |
| Every data store is written by one process and read by one process | Yes |
| Processes named with a verb and noun, flows named with nouns | Yes |

## 4. Core Processes Matrix vs Level-0

| Process | In matrix? | Inputs match the DFD? | Outputs match the DFD? |
| --- | --- | --- | --- |
| P1 Authenticate Users | Yes | Yes | Yes |
| P2 Manage Lost Item Reports | Yes | Yes | Yes |
| P3 Manage Found Item Reports | Yes | Yes | Yes |
| P4 Search Found Items | Yes | Yes | Yes |
| P5 Match Items and Notify Owners | Yes | Yes | Yes |
| P6 Process and Decide Claims | Yes | Yes | Yes |

## 5. Processes vs Functional Requirements and Use Cases

| Process | Use Cases | Functional Requirements |
| --- | --- | --- |
| P1 Authenticate Users | UC-01 | FR-01 |
| P2 Manage Lost Item Reports | UC-02 | FR-02 |
| P3 Manage Found Item Reports | UC-06, UC-07, UC-08, UC-10 | FR-06 to FR-08, FR-10 |
| P4 Search Found Items | UC-03 | FR-03 |
| P5 Match Items and Notify Owners | UC-04, UC-09 | FR-04, FR-09 |
| P6 Process and Decide Claims | UC-05, UC-11, UC-12, UC-13, UC-14, UC-15 | FR-05, FR-11 to FR-16 |

Every one of the 15 use cases (UC-01 to UC-15) is covered by exactly one process.

## 6. Changes needed to earlier labs (Member 3 applies these after the oral)

| File | Change Needed | Reason |
| --- | --- | --- |
| Project/Requirements/Use_Cases.md | List Item Owner, Finder and Campus Security Officer as the actors of UC-01 Log In (skip if already done) | The final use case diagram and both DFDs connect all three to Log In / P1 |
| Project/Requirements/Use_Cases.md | Add Email / Notification Service as an actor of UC-09 Generate Match Suggestions (skip if already done) | The final use case diagram connects it to UC-09, and P5 sends it the Match Notification Request |
| Project/Scope.md | Make sure User Login is listed in the in-scope features (skip if already done) | P1 Authenticate Users is a major process in the Level-0 DFD |
| Project/Requirements/Functional_Requirements.md | Add a functional requirement for resolving disputed claims, or state which existing FR covers UC-14 | UC-14 Resolve Disputed Claim has no matching FR; FR-14 covers release approval, not disputes |
