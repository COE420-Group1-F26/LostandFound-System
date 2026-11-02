# Sequence Diagram Consistency Check

**Checked by:** Mahra Alhammadi (g00098713)

**Diagrams checked:** UC-03 Search Found Items (Member 1), UC-09 Generate Match Suggestions (Member 2), UC-14 Resolve Disputed Claim (Member 3), in Project/Behavioral_Modeling/Sequence_Diagrams. Each diagram was compared with Project/Requirements/Use_Cases.md, Functional_Requirements.md, the Level-0 DFD and the updated class diagram (Project/Object_Modeling/Class_Diagram). The message labels in each .drawio file were checked against the message table in its notes file.

## A. Consistency with Lab 3 — Requirements and Use Cases

| Check | UC-03 | UC-09 | UC-14 | Evidence |
| --- | --- | --- | --- | --- |
| The diagram represents one of the approved use cases | Yes | Yes | Yes | The title frame of each diagram names UC-03, UC-09 and UC-14, which are in Use_Cases.md and Selected_Use_Cases.md. |
| The interaction supports the use case's goal | Yes | Yes | Yes | UC-03 returns the matching found items to the owner (message 8). UC-09 stores ranked match suggestions and emails the owner of each lost report (messages 5, 6, 14). UC-14 approves one claim and rejects every other claim for the item (messages 8, 12). |
| The sequence reflects the use case's description and flow | Yes | Yes | Yes | UC-03: keyword, category, location and date filters, results newest first (messages 1, 5, 7; FR-03). UC-09: compares the found report with the lost reports and emails the owner of the lost report; the finder gets the suggestions back (messages 2–5, 11–14, 18; FR-04, FR-09). UC-14: views pending claims, compares the competing claims, approves one and rejects the rest, as in its includes of UC-11, UC-12 and UC-13 (messages 1, 3–6, 8, 12; FR-16). |
| The messages do not introduce unsupported functionality | Yes | Yes | Yes | Every message traces to FR-03 (UC-03), FR-04 and FR-09 (UC-09), or FR-11 to FR-16 (UC-14). No diagram edits, deletes or pays for anything that the requirements do not allow. |

## B. Consistency with Lab 5 — UML Class Diagram

| Check | UC-03 | UC-09 | UC-14 | Evidence |
| --- | --- | --- | --- | --- |
| Every participating object corresponds to a class in the class diagram | Yes | Yes | Yes | UC-03: ItemSearch, FoundItemReport. UC-09: MatchingService, FoundItemReport, LostItemReport, MatchSuggestion, Notification. UC-14: CampusSecurityOfficer, DisputeCase, Claim, VerificationRecord, FoundItemReport. The other lifelines are the use case actors (Item Owner, Finder, Email / Notification Service, Campus Security Officer). |
| Message names correspond to class operations | Yes | Yes | Yes | Every call message is an operation of the receiving class, with matching parameters. The only other messages are returns, constructors (MatchSuggestion, Notification, VerificationRecord) and sendEmail() to the external Email / Notification Service. |
| Messages are sent between plausible participants | Yes | Yes | Yes | The actor is the first column and the control object (ItemSearch, MatchingService) the second, as in the lecture slides. No entity object sends a message to a control object; entities only return to it. In UC-14 every call follows an association or dependency in the class diagram (DisputeCase groups Claim, Claim has VerificationRecord, Claim is for FoundItemReport). |
| Required operations are in the class model, or the model was updated | Yes | Yes | Yes | setFilters(), sortNewestFirst() and ItemReport.matches() (UC-03) and LostItemReport.getContactEmail() (UC-09) were added, and CampusMember.startSearch() was removed. Recorded in Documents/Lab5_Changes.md. UC-14 needed no change. |

## C. Consistency with the Overall System

| Check | UC-03 | UC-09 | UC-14 | Evidence |
| --- | --- | --- | --- | --- |
| The interactions reflect the actual proposed system | Yes | Yes | Yes | They use the Lost and Found classes, statuses (WITHDRAWN, MATCHED, CLAIMED, PENDING_VERIFICATION) and DFD flows (Delivery Status in UC-09, Item Status Update in UC-14). |
| The actor is consistent with the selected use case | Yes | Yes | Yes | UC-03: Item Owner. UC-09: Finder, with the Email / Notification Service as secondary actor. UC-14: Campus Security Officer. All match Use_Cases.md. |
| The sequence of messages is logical | Yes | Yes | Yes | UC-03 sets the filters before running the search and sorts after the loop. UC-09 scores before ranking and ranks before notifying. UC-14 compares the claims before resolving, and approves the winner before rejecting the others. |
| The diagram is understandable and internally consistent | Yes | Yes | Yes | Messages are numbered in time order, loop frames mark the repeated messages, notes explain the rules, and the message numbers in each notes file match the diagram. |

## D. Notation required by Exercise 2

| Check | UC-03 | UC-09 | UC-14 | Evidence |
| --- | --- | --- | --- | --- |
| A lifeline for every participant | Yes | Yes | Yes | Actors and objects all have lifelines; created objects (MatchSuggestion, Notification, VerificationRecord) start at their creation message. |
| Messages in the correct order, named after the intended operation, with meaningful parameters | Yes | Yes | Yes | See the message tables in the three notes files. |
| Return messages where a value comes back | Yes | Yes | Yes | UC-03: 4, 6, 8. UC-09: 3, 10, 15, 17, 18. UC-14: 2, 5, 6, 11, 14, 16. |
| Activation bars | Yes | Yes | Yes | On every lifeline while it executes, with nested bars for self-calls. |
| At least one iteration (*) and one guard ([ ]) | Yes | Yes | Yes | UC-03: iteration on 3, guard on 5. UC-09: iteration on 2 and 8, guards on 5 and 7. UC-14: iteration on 4 and 12, guard on 12. |
| Made with a UML tool (draw.io) | Yes | Yes | Yes | The editable .drawio file of each diagram is in Sequence_Diagrams. |

## E. Mismatches found and fixed

| Diagram | Mismatch | Fix | Fixed by |
| --- | --- | --- | --- |
| UC-03, UC-09 | Four operations used in the messages were not in the Lab 5 class diagram, and CampusMember.startSearch() let an entity object access a control object. | The class diagram was updated (see Documents/Lab5_Changes.md). | Hamda Albahri (Member 2) |
| UC-14 | The PNG was a screenshot that included a screen tooltip and cut off the diagram frame. | The diagram was exported again from draw.io. | Hamda Bahakeem (Member 3) |

## Confirmation

We confirm that all of the consistency checks listed above (A, B and C, and the notation checks in D) were performed on all three sequence diagrams and verified, and that every mismatch found was fixed before submission.
