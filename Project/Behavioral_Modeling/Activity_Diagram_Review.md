# Activity Diagram Review and Validation

**Reviewed by:** Hamda Albahri

**Diagrams reviewed:** AD_UC03_Search_Found_Items, AD_UC09_Generate_Match_Suggestions and AD_UC14_Resolve_Disputed_Claim in Project/Behavioral_Modeling/Activity_Diagrams.

| No. | Review question | UC-03 | UC-09 | UC-14 | Evidence |
| --- | --- | --- | --- | --- | --- |
| 1 | Does the diagram correspond to one of the use cases selected in Lab 6? | Yes | Yes | Yes | The diagrams model UC-03, UC-09 and UC-14, the same three use cases as Selected_Use_Cases.md and the three Lab 6 sequence diagrams (Selected_Workflows.md). |
| 2 | Is the workflow consistent with the relevant functional requirements and use case description? | Yes | Yes | Yes | UC-03: keyword, category, location and date filters, newest first (FR-03). UC-09: scores lost reports, ranks the suggestions and emails the owners (FR-04, FR-09). UC-14: compares the competing claims, approves one and rejects the others, recording each decision (FR-11 to FR-13, FR-15, FR-16). |
| 3 | Does it have a clearly identifiable initial node and final node? | Yes | Yes | Yes | Each diagram has one initial node at the top of the actor’s lane and one final node after the last merge. |
| 4 | Are the actions named clearly and arranged in a logical order? | Yes | Yes | Yes | Every action is a verb phrase (for example "Check report against filters", "Calculate match score", "Approve winning claim") and follows the order of the use case. |
| 5 | Are control-flow arrows used correctly? | Yes | Yes | Yes | Each arrow goes from one node to the next in the direction of the flow; loops return to a merge node, not to an action. |
| 6 | Are decision branches labeled with meaningful guard conditions? | Yes | Yes | Yes | UC-03: [status = WITHDRAWN] / [status != WITHDRAWN], [matches] / [does not match], [more reports] / [no more reports], [results found] / [no results]. UC-09: [score >= minimumScore] / [score < minimumScore], [more lost reports], [suggestions found] / [no suggestions], [more suggestions] / [all owners notified]. UC-14: [PENDING_VERIFICATION] / [already decided], [more claims] / [all claims checked], [two or more pending claims] / [fewer than two pending claims]. |
| 7 | Do alternative paths lead to appropriate outcomes? | Yes | Yes | Yes | UC-03: no results shows a "no matching items" message. UC-09: no suggestions tells the finder no matches were found yet. UC-14: fewer than two pending claims shows that there is no dispute to resolve. Each alternative ends at the final node. |
| 8 | Are merge nodes used appropriately when alternative paths rejoin? | Yes | Yes | Yes | A merge is used at every loop entry and wherever the alternative paths of a decision rejoin, including just before the final node. |
| 9 | Are fork and join nodes used correctly for parallel activities? | Yes | Yes | Yes | UC-03: the four filters can be entered in any order. UC-09: marking the item MATCHED is independent of emailing the owners. UC-14: marking the item CLAIMED is independent of rejecting the other claims. Each fork has a matching join. |
| 10 | Are swimlanes used appropriately to distinguish responsibilities? | Yes | Yes | Yes | UC-03: Item Owner, ItemSearch, FoundItemReport. UC-09: Finder, MatchingService, Notification, Email / Notification Service. UC-14: Campus Security Officer, DisputeCase, Claim, FoundItemReport. Each action is in the lane of the actor or class that performs it. |
| 11 | Are the activities consistent with the intended system behavior and the other project models? | Yes | Yes | Yes | Every system action matches an operation in the Lab 5 class diagram and a message in the Lab 6 sequence diagram (Activity_Consistency_Check.md, section B). |
| 12 | Are there no missing activities, unnecessary steps, contradictory paths or unreachable actions? | Yes | Yes | Yes | Every node is reachable from the initial node and leads to the final node; every message of the Lab 6 sequence diagram has a matching action, and no action does something the requirements do not allow. |

## Problems found and fixed

| Diagram | Problem | Fix | Fixed by |
| --- | --- | --- | --- |
| Selected_Workflows.md | The "Drawn by" column listed the task-division members, but UC-09 was drawn by Hamda Bahakeem and UC-14 by Hamda Albahri. | Replaced with the names of the members who drew each diagram. | Hamda Albahri |

## Confirmation

We confirm that all twelve review questions were checked for all three activity diagrams and that every problem found was fixed before submission.
