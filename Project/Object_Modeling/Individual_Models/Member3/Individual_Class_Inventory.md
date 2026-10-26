# Individual Class Inventory — Member 3

**Contributor:** Hamda Bahakeem (g00100146)

**Component modeled:** Claims — submitting a claim, checking it, approving or rejecting it, handling disputes, and releasing the item

**Level-0 processes:** P6 Process and Decide Claims

**Data stores:** D5 Claims (my part also reads D3 Found Item Reports to see the item details)

**Requirements covered:** FR-05, FR-11 to FR-16

## Classes

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Contributor |
| --- | --- | --- | --- | --- |
| C20 | Claim | When an owner says "this found item is mine". It keeps their AUS ID, the private details they give to prove it, and the claim status. Saved in D5. | UC-05, UC-11, UC-12, UC-13 | Hamda Bahakeem |
| C21 | ClaimStatus (enumeration) | The only statuses a claim can have: PENDING_VERIFICATION, APPROVED, REJECTED, RELEASED. | UC-05, UC-12, UC-13 | Hamda Bahakeem |
| C22 | VerificationRecord | One decision by a security officer on a claim (approved or rejected), with who decided, notes, and the date and time. All of these together are the claim history. | UC-12, UC-13, UC-15 | Hamda Bahakeem |
| C23 | DisputeCase | Used when two or more people claim the same item. The officer compares them, approves one and rejects the others. | UC-14 | Hamda Bahakeem |
| C24 | ReleaseRecord | Made when an approved claim is ready for pickup, and finished when the owner actually gets the item. | UC-12 | Hamda Bahakeem |

## Why each class is a class (not just an attribute)

| Class | Justification |
| --- | --- |
| Claim | A claim has its own info (AUS ID, details, date, status) and goes through its own steps from submitted to decided. One item can have more than one claim, so it can't just be a field inside FoundItemReport. |
| ClaimStatus | FR-05, FR-12 and FR-13 say exactly which statuses are allowed ("Pending Verification", "Approved", "Rejected"), so an enumeration makes sure nothing else is used. It's different from Member 2's ReportStatus because that one is about the item, this one is about the claim. |
| VerificationRecord | FR-15 needs a history of every approve/reject, who did it and when. A claim can get more than one decision, so one field on Claim isn't enough. |
| DisputeCase | UC-14 and FR-16 need all the claims for the same item grouped together and decided once. The dispute has its own dates and result that don't belong to just one claim. |
| ReleaseRecord | FR-14 says an item can only be released after the claim is approved. The release has its own info (when it was ready, when it was picked up, who picked it up) and only approved claims have it, so I kept it separate from Claim. |

## Object types

- **Entity objects:** Claim, VerificationRecord, DisputeCase, ReleaseRecord (all saved in D5 Claims).
- **Enumeration:** ClaimStatus.
- **Control objects:** none. The officer makes decisions through the methods in Claim and DisputeCase, so an extra control class wouldn't add anything.
- **Boundary objects:** none. The claim form and officer screens are part of the UI design.

## Inheritance

I didn't use inheritance. ClaimStatus is an enumeration (just a list of values), not a parent class. My other classes are different kinds of records and none of them "is a" type of another, so a parent class wouldn't make sense.

## Relationships

- ItemOwner submits Claim (1 to 0..*): one owner can make many claims, but each claim is from one owner (FR-05).
- Claim is for FoundItemReport (0..* to 1): many people can claim the same item, but each claim is for one item (FR-05, FR-11).
- Claim has VerificationRecord, **composition** (1 to 0..*): the decisions belong to that claim and make up its history (FR-15).
- CampusSecurityOfficer records VerificationRecord (1 to 0..*): every decision shows which officer made it (FR-15).
- DisputeCase groups Claim (0..1 to 2..*): a dispute needs at least 2 claims, and a claim can only be in one dispute (UC-14, FR-16).
- CampusSecurityOfficer resolves DisputeCase (0..1 to 0..*): an open dispute has no officer yet, a solved one has one (FR-16).
- Claim ends with ReleaseRecord (1 to 0..1): only approved claims get a release record (FR-14).
- Claim **uses** ClaimStatus (dependency): it's the type of the status attribute.

## Classes of other members used for reference only

| Class | Owner |
| --- | --- |
| ItemOwner | Member 1 |
| CampusSecurityOfficer | Member 1 |
| FoundItemReport | Member 2 |

## Open design questions (to settle in Exercise 3)

1. Member 1 has approveClaim() and rejectClaim() in CampusSecurityOfficer, and I have approve() and reject() in Claim. We should keep only one, or have the officer's methods just call the Claim ones.
2. ReleaseRecord doesn't have its own use case, it's there for UC-12 and FR-14. We need to decide if it stays a class or becomes a few attributes in Claim.
3. When a claim is approved or the item is picked up, the item's status (Member 2's ReportStatus) should change to CLAIMED and then RETURNED. We need to agree which class does this.
4. The task division suggested a DISPUTED status, but no requirement mentions it, so I left it out. A disputed claim just stays PENDING_VERIFICATION until the dispute is solved.
