# UC-09 Generate Match Suggestions — Sequence Diagram

**Drawn by:** Hamda Albahri (g00100284)

**Use case:** UC-09 Generate Match Suggestions

**Primary actor:** Finder (secondary: Email / Notification Service)

**Requirements:** FR-04, FR-09

**Diagram:** UC-09_Generate_Match_Suggestions.png (editable: UC-09_Generate_Match_Suggestions.drawio)

## Participants

| Lifeline | Class | Object type |
| --- | --- | --- |
| Finder | Actor (stick figure), 1st column | The person who submitted the found report |
| :MatchingService | MatchingService | Control object (2nd column) |
| found: FoundItemReport | FoundItemReport | Entity object |
| lost: LostItemReport | LostItemReport | Entity object; one lifeline stands for every lost report |
| m: MatchSuggestion | MatchSuggestion | Entity object, created during the interaction |
| n: Notification | Notification | Entity object, created during the interaction |
| Email / Notification Service | Actor (stick figure), last column | External system; outside the system |

## Messages

| No. | From → To | Message | Class operation |
| --- | --- | --- | --- |
| 1 | Finder → :MatchingService | findMatchesForFound(found) | MatchingService.findMatchesForFound(found: FoundItemReport): List\<MatchSuggestion\> |
| 2 | MatchingService → lost | *[for each lost item report] getPublicDetails() | LostItemReport.getPublicDetails(): String |
| 3 | lost → MatchingService (dashed) | return details | return message |
| 4 | MatchingService → MatchingService (self-call) | calculateScore(lost, found) | MatchingService.calculateScore(...): double (private) |
| 5 | MatchingService → m (creates m) | [score \>= minimumScore] MatchSuggestion(lost, found, score) | creation (constructor) |
| 6 | MatchingService → MatchingService (self-call) | rankMatches(matches) | MatchingService.rankMatches(...): List\<MatchSuggestion\> (private) |
| 7 | MatchingService → found | [matches not empty] updateStatus(MATCHED) | FoundItemReport.updateStatus(newStatus: ReportStatus): void |
| 8 | MatchingService → MatchingService (self-call) | *[for each ranked suggestion] notifyOwner(match) | MatchingService.notifyOwner(match: MatchSuggestion): Notification |
| 9 | MatchingService → lost | getContactEmail() | LostItemReport.getContactEmail(): String (added in Lab 6) |
| 10 | lost → MatchingService (dashed) | return email | return message |
| 11 | MatchingService → n (creates n) | Notification(match, email) | creation (constructor) |
| 12 | MatchingService → n | send() | Notification.send(): boolean |
| 13 | n → n (self-call) | buildMessage(found) | Notification.buildMessage(found: FoundItemReport): String (private) |
| 14 | n → Email / Notification Service | sendEmail(email, message) | external system (not a class operation) |
| 15 | Email / Notification Service → n (dashed) | return deliveryStatus | return message (DFD flow "Delivery Status") |
| 16 | n → n (self-call) | recordDeliveryStatus(delivered) | Notification.recordDeliveryStatus(delivered: boolean): void |
| 17 | n → MatchingService (dashed) | return sent | return message |
| 18 | MatchingService → Finder (dashed) | return suggestions | return List\<MatchSuggestion\> |

**Iteration and guards:** iteration on messages 2 and 8; guards on messages 5 and 7.

## Operations this diagram needs that were not in the Lab 5 class diagram

| Class | Operation | Reason |
| --- | --- | --- |
| LostItemReport | + getContactEmail(): String | Message 9: the notification copies the owner's contact email from the lost report into recipientEmail (Lab 5 decision, D2). |
