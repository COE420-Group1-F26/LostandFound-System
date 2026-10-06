# Work Breakdown Structure (WBS)

Durations are in working days (Monday-Friday). The duration of each task is the rounded Expected Time (E) calculated from the three-point estimate.

## WBS Activities

| ID | Task Name | Description | Predecessor(s) | Duration (Days) |
| --- | --- | --- | --- | --- |
| T1 | Refine System Requirements | Review and finalize requirements and use cases for the Lost and Found Property Claim System. | --- | 2 |
| T2 | Design Database | Design the database structure for users, lost/found reports, matches, and claims. | T1 | 3 |
| T3 | Design User Interface | Design interfaces for reporting items, searching, claims, and the security dashboard. | T1 | 3 |
| T4 | Implement Authentication | Implement AUS user login and security officer authentication. | T2 | 2 |
| T5 | Implement Lost and Found Reporting | Implement creation and management of lost and found item reports, including photos. | T2, T3 | 4 |
| T6 | Implement Search and Matching | Implement found-item search and automatic matching of lost and found reports. | T5 | 4 |
| T7 | Implement Notifications | Implement match notifications for relevant users. | T6 | 2 |
| T8 | Implement Claim Verification | Implement claim submission, security review, approval/rejection, and claim history. | T4, T5 | 4 |
| T9 | Integrate and Test System | Integrate all modules and perform functional and integration testing. | T7, T8 | 4 |
| T10 | Prepare Deployment | Prepare the completed system for deployment and perform final verification. | T9 | 2 |

## Time Estimates

Expected Time is calculated using:

**E = (O + 4M + P) / 6**

| ID | Optimistic (O) | Most Likely (M) | Pessimistic (P) | E (Exact) | E (Rounded) |
| --- | --- | --- | --- | --- | --- |
| T1 | 1 | 2 | 3 | 2.00 | 2 |
| T2 | 2 | 3 | 4 | 3.00 | 3 |
| T3 | 2 | 3 | 4 | 3.00 | 3 |
| T4 | 1 | 2 | 3 | 2.00 | 2 |
| T5 | 3 | 4 | 5 | 4.00 | 4 |
| T6 | 3 | 4 | 5 | 4.00 | 4 |
| T7 | 1 | 2 | 3 | 2.00 | 2 |
| T8 | 3 | 4 | 5 | 4.00 | 4 |
| T9 | 3 | 4 | 5 | 4.00 | 4 |
| T10 | 1 | 2 | 3 | 2.00 | 2 |
