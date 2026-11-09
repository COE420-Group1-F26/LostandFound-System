# UC-09 Generate Match Suggestions — Activity Diagram

**Drawn by:** Hamda Bahakeem (g00100146)

**Use case:** UC-09 Generate Match Suggestions (same use case as the Lab 6 sequence diagram UC-09_Generate_Match_Suggestions)

**Requirements:** FR-04, FR-09

**Files:** AD_UC09_Generate_Match_Suggestions.pdf, AD_UC09_Generate_Match_Suggestions.png (editable: AD_UC09_Generate_Match_Suggestions.drawio)

**Swimlanes:** Finder, MatchingService, Notification, Email / Notification Service

**Decisions:** score reaches the minimum or not (7), more lost reports (10), suggestions found or not (11), more suggestions to notify (20).

**Parallel activities:** marking the found item MATCHED happens at the same time as notifying the owners (fork 13 / join 21).

**Notes in the diagram:** a match suggestion is only made when the score reaches minimumScore (FR-09); marking the item MATCHED and emailing the owners happen at the same time, and each email goes to the owner of the lost report (FR-04).

## Nodes and control flows

| No. | Swimlane | Node | Label | Next (guard) | Matches (Lab 6 sequence / Lab 5 class) |
| --- | --- | --- | --- | --- | --- |
| 1 | Finder | Initial | — | → 2 | — |
| 2 | Finder | Action | Submit found item report | → 3 | UC-06 · triggers Seq 1 |
| 3 | MatchingService | Action | Start matching for the found report | → 4 | findMatchesForFound() · Seq 1 |
| 4 | MatchingService | Merge | — | → 5 | — |
| 5 | MatchingService | Action | Read next lost item report | → 6 | getPublicDetails() · Seq 2–3 |
| 6 | MatchingService | Action | Calculate match score | → 7 | calculateScore() · Seq 4 |
| 7 | MatchingService | Decision | — | [score \>= minimumScore] → 8; [score \< minimumScore] → 9 | Seq 5 guard |
| 8 | MatchingService | Action | Create match suggestion | → 9 | MatchSuggestion() · Seq 5 |
| 9 | MatchingService | Merge | — | → 10 | — |
| 10 | MatchingService | Decision | — | [more lost reports] → 4; [no more lost reports] → 11 | loop end |
| 11 | MatchingService | Decision | — | [suggestions found] → 12; [no suggestions] → 23 | Seq 7 guard |
| 12 | MatchingService | Action | Rank suggestions | → 13 | rankMatches() · Seq 6 |
| 13 | MatchingService | Fork | — | → 14, 15 | — |
| 14 | MatchingService | Action | Mark found item as MATCHED | → 21 | updateStatus(MATCHED) · Seq 7 |
| 15 | MatchingService | Merge | — | → 16 | — |
| 16 | MatchingService | Action | Get owner's contact email | → 17 | getContactEmail() · Seq 9–10 |
| 17 | Notification | Action | Build notification email | → 18 | Notification(), buildMessage() · Seq 11–13 |
| 18 | Email / Notification Service | Action | Send email to owner | → 19 | sendEmail() · Seq 14 |
| 19 | Notification | Action | Record delivery status | → 20 | recordDeliveryStatus() · Seq 15–16 |
| 20 | MatchingService | Decision | — | [more suggestions] → 15; [all owners notified] → 21 | Seq 8 loop end |
| 21 | MatchingService | Join | — | → 22 | — |
| 22 | Finder | Action | View match suggestions | → 24 | Seq 18 |
| 23 | Finder | Action | See that no matches were found yet | → 24 | Seq 18 (empty list) |
| 24 | Finder | Merge | — | → 25 | — |
| 25 | Finder | Final | — | — | — |
