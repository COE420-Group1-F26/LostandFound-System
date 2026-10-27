# Review of the Other Individual Models — by Member 1

**Reviewer:** Mahra Alhammadi (g00098713)

**Models reviewed:** Member 2 — Hamda Albahri (g00100284): found items, matching and notifications. Member 3 — Hamda Bahakeem (g00100146): claims.

Each point was checked against Project/Requirements/Functional_Requirements.md, Use_Cases.md and the Level-0 DFD data stores.

## 1. Duplicate or overlapping classes

| Class A (owner) | Class B (owner) | What overlaps | My suggestion |
| --- | --- | --- | --- |
| ItemPhoto in FoundItemReport (Member 2) | ItemPhoto in LostItemReport (Member 1) | Both models make ItemPhoto a composition part, but one class can only be the part of one composition. | Add an abstract parent class ItemReport that owns ItemPhoto (composition, 1 to 0..3), with a note that a lost report has at most 1 photo (FR-02) and a found report up to 3 (FR-07). |
| FoundItemReport (Member 2) | LostItemReport (Member 1) | Both hold category, description, location, date, photos and a status, and both are submitted, edited and withdrawn. | Same parent class ItemReport for the shared data and operations. Both are item reports, so this is a real "is a" relationship. |
| Claim.approve() / reject() (Member 3) | CampusSecurityOfficer.approveClaim() / rejectClaim() (Member 1) | Approving and rejecting a claim appears in two classes. | Keep it only in Claim: Member 3's operations already take the officer as a parameter and create the VerificationRecord. Remove the two operations from CampusSecurityOfficer. |

## 2. Name or responsibility differences

| Class | Problem | My suggestion |
| --- | --- | --- |
| FoundItemReport.edit() (Member 2) and LostItemReport.update() (Member 1) | Same behaviour, different names. | Use edit() for both. |
| Notification.recipientEmail (Member 2) | The email address is already stored in User and reached through the "is sent to" association. | Remove it. |
| Claim.claimantAusId (Member 3) | Looks like a copy of User.ausId. | Keep it: FR-05 asks the claimant to enter their AUS ID so the officer can verify it, so it is the value typed in, not the account field. Mention this in a note. |
| Claim.identifyingDetails (Member 3) and LostItemReport.privateDetails (Member 1) | Similar names. | Keep both: one is written in the lost report, the other is given when claiming. |

## 3. Missing classes or operations

| Missing | Why it is needed (use case or requirement) |
| --- | --- |
| An operation that lists all pending claims | UC-11 and FR-11: the officer views all pending claims. No class has an operation for this. Suggest CampusSecurityOfficer.viewPendingClaims(): List\<Claim\>. |
| An operation that shows the full claim history | UC-15 and FR-15: Claim.getHistory() only returns the history of one claim. Suggest CampusSecurityOfficer.viewClaimHistory(): List\<VerificationRecord\>. |
| Who updates the item status after a claim | The DFD flow "Item Status Update" goes from P6 to D3. Agree with Member 3: Claim.approve() sets the found item to CLAIMED and ReleaseRecord.confirmHandover() sets it to RETURNED, shown with a UML note. |
| No missing classes | Every use case from UC-01 to UC-15 is covered by at least one class. |

## 4. Attributes and methods (consistency, data types, visibility)

| Class | Problem | My suggestion |
| --- | --- | --- |
| Member 2 and Member 3 diagrams | Visibility, data types, parameters and return types are shown everywhere. | No change. |
| MatchingService (Member 2) | notifyOwner() returns a Notification and findMatchesForLost() / findMatchesForFound() take report parameters, but there is no line to those classes. | Add dashed dependency arrows from MatchingService to Notification (and to the report classes if the diagram stays readable). |
| VerificationRecord.decision: ClaimStatus (Member 3) | Uses ClaimStatus as a type, but no dependency is drawn. | Add a dashed dependency from VerificationRecord to ClaimStatus. |
| Withdraw operations | LostItemReport.withdraw() returns void (Member 1) and FoundItemReport.withdraw() returns boolean (Member 2). FR-08 says withdrawing can fail. | Use withdraw(): boolean on the shared parent class. |

## 5. Inheritance

| Relationship | Is it a real is-a relationship? | My suggestion |
| --- | --- | --- |
| None in Member 2's or Member 3's model | Correct: ReportStatus and ClaimStatus are enumerations, not parent classes. | Add ItemReport as the parent of LostItemReport and FoundItemReport (section 1). |
| ItemOwner and Finder → User (my model, raised by Member 3) | FR-02, FR-03 and FR-05 say "a logged-in user", so the same AUS account can lose one item and find another. With two subclasses, one account can only be one of them. | Merge ItemOwner and Finder into one class for AUS students and staff, and show "owner" and "finder" as role names on the associations (role names are in the lecture slides). |

## 6. Associations and multiplicities

| Association | Problem | My suggestion |
| --- | --- | --- |
| Notification is sent to ItemOwner (Member 2) | I first thought the finder should also receive it (UC-09). | Keep it as it is: FR-04 only emails the owner of the lost report, and the finder sees matches through the found report, as Member 2 explains in her inventory. |
| FoundItemReport has ItemPhoto, 1 to 0..3 (Member 2) | Correct for FR-07. | Move it to ItemReport after integration. |
| DisputeCase groups Claim, 0..1 to 2..* (Member 3) | Correct: a dispute needs at least two claims (UC-14, FR-16). | No change. |
| Claim is for FoundItemReport, 0..* to 1; Claim has VerificationRecord, 1 to 0..* (Member 3) | Correct. | No change. |
| MatchSuggestion to LostItemReport and FoundItemReport, 0..* to 1 (Member 2) | Correct: each suggestion pairs one lost and one found report. | No change. |

## 7. Unnecessary or redundant classes

| Class | Why it can be removed or merged |
| --- | --- |
| ReleaseRecord (Member 3) | It could be two date attributes in Claim, but it has its own data (ready date, release date, who collected the item) and operation, and only approved claims have one (FR-14). I suggest keeping it. |
| Member 2's classes | All needed; each traces to FR-04 or FR-06 to FR-10. |
