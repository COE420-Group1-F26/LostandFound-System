# Earlier Lab Changes for Lab 5

| File | Change Made | Caused By |
| --- | --- | --- |
| Project/Data_Flow_Modeling/Level_0_DFD/Level_0_Flows.md | P6 Process and Decide Claims now lists FR-05, FR-11 to FR-16. | FR-16 was added in Lab 4 but the P6 row still said FR-11 to FR-15. Found in the Lab 5 consistency check. |
| Project/Data_Flow_Modeling/DFD_Consistency_Check.md | Same change for P6 in section 5. | Same reason. |
| Project/Requirements/Use_Cases.md | UC-09 Generate Match Suggestions: the description now says the owner of the lost report gets the email and the finder sees the match through their found report (member table and consolidated table). | Lab 5 integration decision: the notification goes to the owner only (FR-04, Notification is sent to CampusMember as owner). |
| Project/Requirements/Use_Cases.md | UC-10 Check Item Status: added Withdrawn to the list of statuses (member table and consolidated table). | ReportStatus in the class model has WITHDRAWN (FR-08), and D3 Found Item Reports already lists it. |
| Project/Requirements/Functional_Requirements.md | FR-10: added Withdrawn to the list of statuses (member table and consolidated table). | Same reason. |

No change was needed for UC-14, because FR-16 (added in Lab 4) already covers resolving disputed claims. The actors Item Owner and Finder stay in the use case model; in the class model they are roles played by one CampusMember account.
