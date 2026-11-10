# Activity Diagram Consistency Check

**Checked by:** Hamda Albahri

Each activity diagram was compared with Project/Requirements/Use_Cases.md and Functional_Requirements.md (Lab 3), the class diagram in Project/Object_Modeling/Class_Diagram (Lab 5), and its sequence diagram in Project/Behavioral_Modeling/Sequence_Diagrams (Lab 6).

## A. Lab 3 — Requirements and use cases

| Diagram | Use case behavior reflected? | Requirements reflected | Evidence |
| --- | --- | --- | --- |
| AD UC-03 | Yes | FR-03 | The owner searches and filters found items (UC-03): keyword, category, location and date range, results newest first. |
| AD UC-09 | Yes | FR-04, FR-09 | The system compares the found report with the lost reports, ranks the matches and emails the owner of each lost report; the finder sees the suggestions (UC-09). |
| AD UC-14 | Yes | FR-16 (with FR-11 to FR-15) | The officer reviews the competing claims, compares the identifying details, approves one claim and rejects the others, and every decision is recorded (UC-14, which includes UC-11, UC-12 and UC-13). |

## B. Lab 5 and Lab 6 — Each action against its sequence message and class operation

| Diagram | Action | Lab 6 message / Lab 5 operation | Matches? |
| --- | --- | --- | --- |
| UC-03 | 3. Enter keyword | FR-03 | Yes |
| UC-03 | 4. Select category | FR-03 | Yes |
| UC-03 | 5. Select location found | FR-03 | Yes |
| UC-03 | 6. Select date range | FR-03 | Yes |
| UC-03 | 8. Submit search | Seq 1–2 | Yes |
| UC-03 | 9. Save search filters | setFilters() · Seq 1 | Yes |
| UC-03 | 11. Get next found item report | execute() loop · Seq 3 | Yes |
| UC-03 | 13. Check report against filters | matches() · Seq 5–6 | Yes |
| UC-03 | 15. Add report to results | note in Seq | Yes |
| UC-03 | 18. Sort results newest first | sortNewestFirst() · Seq 7 | Yes |
| UC-03 | 20. View matching found items | Seq 8 | Yes |
| UC-03 | 21. See "no matching items" message | Seq 8 (empty list) | Yes |
| UC-09 | 2. Submit found item report | UC-06 · triggers Seq 1 | Yes |
| UC-09 | 3. Start matching for the found report | findMatchesForFound() · Seq 1 | Yes |
| UC-09 | 5. Read next lost item report | getPublicDetails() · Seq 2–3 | Yes |
| UC-09 | 6. Calculate match score | calculateScore() · Seq 4 | Yes |
| UC-09 | 8. Create match suggestion | MatchSuggestion() · Seq 5 | Yes |
| UC-09 | 12. Rank suggestions | rankMatches() · Seq 6 | Yes |
| UC-09 | 14. Mark found item as MATCHED | updateStatus(MATCHED) · Seq 7 | Yes |
| UC-09 | 16. Get owner’s contact email | getContactEmail() · Seq 9–10 | Yes |
| UC-09 | 17. Build notification email | Notification(), buildMessage() · Seq 11–13 | Yes |
| UC-09 | 18. Send email to owner | sendEmail() · Seq 14 | Yes |
| UC-09 | 19. Record delivery status | recordDeliveryStatus() · Seq 15–16 | Yes |
| UC-09 | 22. View match suggestions | Seq 18 | Yes |
| UC-09 | 23. See that no matches were found yet | Seq 18 (empty list) | Yes |
| UC-14 | 2. View pending claims | viewPendingClaims() · Seq 1–2 | Yes |
| UC-14 | 3. Open the dispute for the found item | compareClaims() · Seq 3 | Yes |
| UC-14 | 5. Check claim status | getStatus() · Seq 4–5 | Yes |
| UC-14 | 7. Add claim to the comparison | note in Seq | Yes |
| UC-14 | 11. Compare identifying details side by side | Seq 6 · FR-16 | Yes |
| UC-14 | 12. Select the claim that verifies ownership | resolve() · Seq 7 | Yes |
| UC-14 | 13. Approve winning claim and record APPROVED decision | approve(), VerificationRecord() · Seq 8–9 | Yes |
| UC-14 | 15. Mark found item as CLAIMED | updateStatus(CLAIMED) · Seq 10 | Yes |
| UC-14 | 16. Reject every other claim and record REJECTED decisions | reject(), VerificationRecord() · Seq 12–13 | Yes |
| UC-14 | 18. Mark dispute as resolved | isResolved() · Seq 15 | Yes |
| UC-14 | 19. See the dispute result | Seq 16 | Yes |
| UC-14 | 20. See that there is no dispute to resolve | — | Yes |

Actions performed by actors (for example "Enter keyword" or "Compare identifying details side by side") are user steps; they lead to the first message the actor sends in the sequence diagram.

## C. Lab 6 — Same use case, no contradictions

| Diagram | Same use case as its sequence diagram? | Any contradiction with the sequence diagram? | Evidence |
| --- | --- | --- | --- |
| AD UC-03 vs UC-03_Search_Found_Items | Yes | No | Same loop over the found reports, same withdrawn guard, same sort step. The alternative "no results" path is the empty result list returned by message 8. |
| AD UC-09 vs UC-09_Generate_Match_Suggestions | Yes | No | Same loop and minimum-score guard. The sequence diagram sends updateStatus(MATCHED) before the notifications; the activity diagram shows the two as parallel, which does not contradict it because they are independent. |
| AD UC-14 vs UC-14_Resolve_Disputed_Claim | Yes | No | Same order: view pending claims, compare, approve the winner, reject the others, mark resolved. Marking the item CLAIMED and rejecting the other claims are shown as parallel because they are independent. The "fewer than two pending claims" path is an alternative outcome that the sequence diagram does not model. |

## Confirmation

We confirm that the consistency checks with Lab 3 (requirements and use cases), Lab 5 (class diagram) and Lab 6 (sequence diagrams) were performed and verified for all three activity diagrams.
