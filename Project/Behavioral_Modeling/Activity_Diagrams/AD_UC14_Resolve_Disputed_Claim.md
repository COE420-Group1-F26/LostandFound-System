# UC-14 Resolve Disputed Claim — Activity Diagram

**Drawn by:** Hamda Albahri

**Use case:** UC-14 Resolve Disputed Claim (same use case as the Lab 6 sequence diagram UC-14_Resolve_Disputed_Claim)

**Requirements:** FR-16 (with FR-11 to FR-15)

**Files:** AD_UC14_Resolve_Disputed_Claim.pdf, AD_UC14_Resolve_Disputed_Claim.png (editable: AD_UC14_Resolve_Disputed_Claim.drawio)

**Swimlanes:** Campus Security Officer, DisputeCase, Claim, FoundItemReport

**Decisions:** claim pending or already decided (6), more claims (9), two or more pending claims or not (10).

**Parallel activities:** marking the found item CLAIMED happens at the same time as rejecting the other claims (fork 14 / join 17).

## Workflow

The officer views the pending claims and opens the dispute for one found item. DisputeCase goes through the claims one by one: each claim still in PENDING_VERIFICATION is added to the comparison, and claims that were already decided are skipped. If fewer than two pending claims remain, there is no dispute to resolve. Otherwise the officer compares the identifying details side by side (FR-16) and selects the claim that verifies ownership. That claim is approved and an APPROVED decision is recorded (FR-12, FR-15); then, at the same time, the found item is marked CLAIMED and every other claim is rejected with a REJECTED decision (FR-13, FR-15). Finally the dispute is marked resolved and the officer sees the result.

## Nodes and control flows

| No. | Swimlane | Node | Label | Next (guard) | Matches (Lab 6 sequence / Lab 5 class) |
| --- | --- | --- | --- | --- | --- |
| 1 | Campus Security Officer | Initial | — | → 2 | — |
| 2 | Campus Security Officer | Action | View pending claims | → 3 | viewPendingClaims() · Seq 1–2 |
| 3 | Campus Security Officer | Action | Open the dispute for the found item | → 4 | compareClaims() · Seq 3 |
| 4 | DisputeCase | Merge | — | → 5 | — |
| 5 | Claim | Action | Check claim status | → 6 | getStatus() · Seq 4–5 |
| 6 | Claim | Decision | — | [PENDING_VERIFICATION] → 7; [already decided] → 8 | — |
| 7 | DisputeCase | Action | Add claim to the comparison | → 8 | note in Seq |
| 8 | DisputeCase | Merge | — | → 9 | — |
| 9 | DisputeCase | Decision | — | [more claims] → 4; [all claims checked] → 10 | loop end |
| 10 | DisputeCase | Decision | — | [two or more pending claims] → 11; [fewer than two pending claims] → 20 | — |
| 11 | Campus Security Officer | Action | Compare identifying details side by side | → 12 | Seq 6 · FR-16 |
| 12 | Campus Security Officer | Action | Select the claim that verifies ownership | → 13 | resolve() · Seq 7 |
| 13 | Claim | Action | Approve winning claim and record APPROVED decision | → 14 | approve(), VerificationRecord() · Seq 8–9 |
| 14 | Claim | Fork | — | → 15, 16 | — |
| 15 | FoundItemReport | Action | Mark found item as CLAIMED | → 17 | updateStatus(CLAIMED) · Seq 10 |
| 16 | Claim | Action | Reject every other claim and record REJECTED decisions | → 17 | reject(), VerificationRecord() · Seq 12–13 |
| 17 | DisputeCase | Join | — | → 18 | — |
| 18 | DisputeCase | Action | Mark dispute as resolved | → 19 | isResolved() · Seq 15 |
| 19 | Campus Security Officer | Action | See the dispute result | → 21 | Seq 16 |
| 20 | Campus Security Officer | Action | See that there is no dispute to resolve | → 21 | — |
| 21 | Campus Security Officer | Merge | — | → 22 | — |
| 22 | Campus Security Officer | Final | — | — | — |
