# UC-14 Resolve Disputed Claim — Sequence Diagram

**Drawn by:** Hamda Bahakeem (g00100146)

**Use case:** UC-14 Resolve Disputed Claim

**Primary actor:** Campus Security Officer

**Requirements:** FR-16 (with FR-11 to FR-15)

**Diagram:** UC-14_Resolve_Disputed_Claim.png (editable: UC-14_Resolve_Disputed_Claim.drawio)

## Participants

| Lifeline | Class | Object type |
| --- | --- | --- |
| Campus Security Officer | Actor (stick figure) | The person; outside the system |
| officer: CampusSecurityOfficer | CampusSecurityOfficer | Entity object (the officer's account) |
| dispute: DisputeCase | DisputeCase | Entity object |
| winner: Claim | Claim | Entity object (the claim that is approved) |
| others: Claim | Claim | Entity object; one lifeline stands for every other claim |
| record: VerificationRecord | VerificationRecord | Entity object, created during the interaction |
| item: FoundItemReport | FoundItemReport | Entity object |

## Messages

| No. | From → To | Message | Class operation |
| --- | --- | --- | --- |
| 1 | Officer (actor) → officer | viewPendingClaims() | CampusSecurityOfficer.viewPendingClaims(): List\<Claim\> (UC-11) |
| 2 | officer → Officer (dashed) | return pendingClaims | return List\<Claim\> |
| 3 | Officer → dispute | compareClaims() | DisputeCase.compareClaims(): List\<Claim\> |
| 4 | dispute → others | *[for each claim in the dispute] getStatus() | Claim.getStatus(): ClaimStatus |
| 5 | others → dispute (dashed) | return status | return message |
| 6 | dispute → Officer (dashed) | return claims | return List\<Claim\> |
| 7 | Officer → dispute | resolve(winner, officer) | DisputeCase.resolve(approved: Claim, officer: CampusSecurityOfficer): void |
| 8 | dispute → winner | approve(officer, resolutionNotes) | Claim.approve(officer, notes): VerificationRecord (UC-12) |
| 9 | winner → record (creates record) | VerificationRecord(APPROVED, officer, notes) | creation (constructor) |
| 10 | winner → item | updateStatus(CLAIMED) | FoundItemReport.updateStatus(newStatus: ReportStatus): void |
| 11 | winner → dispute (dashed) | return record | return VerificationRecord |
| 12 | dispute → others | *[for each other claim] [claim != winner] reject(officer, reason) | Claim.reject(officer, reason): VerificationRecord (UC-13) |
| 13 | others → record (creates record) | VerificationRecord(REJECTED, officer, reason) | creation (constructor) |
| 14 | others → dispute (dashed) | return record | return VerificationRecord |
| 15 | dispute → dispute (self-call) | isResolved() | DisputeCase.isResolved(): boolean |
| 16 | dispute → Officer (dashed) | return resolved | return message |

**Iteration and guards:** iteration on messages 4 and 12; guard on message 12. Messages 4 to 5 and messages 12 to 14 are inside loop frames.

**Notes in the diagram:** only claims with status PENDING_VERIFICATION are compared side by side (FR-16); the found item becomes CLAIMED (DFD flow Item Status Update).

## Operations this diagram needs that were not in the Lab 5 class diagram

| Class | Operation | Reason |
| --- | --- | --- |
| — | None | Every message is an operation already in the Lab 5 class diagram, a return, or a constructor. |
