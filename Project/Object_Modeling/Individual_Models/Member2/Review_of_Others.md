# Review of the Other Individual Models — by Member 2

**Reviewer:** Hamda Albahri (g00100284)

**Models reviewed:** Member 1 (Mahra Alhammadi) — users, lost reports and search; Member 3 (Hamda Bahakeem) — claims

## 1. Duplicate or overlapping classes

| Class A (owner) | Class B (owner) | What overlaps | My suggestion |
| --- | --- | --- | --- |
| ItemPhoto in LostItemReport (Member 1) | ItemPhoto in FoundItemReport (Member 2) | Both use composition with ItemPhoto, but one part class cannot belong to two different wholes. | Add an abstract parent class ItemReport that owns the photos (composition 1 to 0..3), with a note that a lost report allows only 1 photo (FR-02) and a found report up to 3 (FR-07). This is a real is-a relationship (see section 5). If the team does not want a parent class, split it into LostItemPhoto and FoundItemPhoto. |
| CampusSecurityOfficer approveClaim() / rejectClaim() (Member 1) | Claim approve() / reject() (Member 3) | Approving and rejecting a claim is written in two places. | Keep the logic in Claim, because it changes the claim status and creates the VerificationRecord. Remove the two methods from CampusSecurityOfficer and give the officer the methods that are really missing (see section 3). |
| ClaimStatus.RELEASED (Member 3) | ReportStatus.RETURNED (Member 2) and ReleaseRecord.isReleased() (Member 3) | Three places say the same thing: the item was handed back. No requirement mentions a "Released" claim status (FR-05, FR-12, FR-13 only give Pending Verification, Approved and Rejected). | Remove RELEASED from ClaimStatus. The claim stays APPROVED, the ReleaseRecord says when it was picked up, and the item becomes RETURNED. |
| LostItemReport.status: ReportStatus (Member 1) | FoundItemReport.status: ReportStatus (Member 2) | ReportStatus comes from FR-10, which is about found items. A lost report never becomes CLAIMED or RETURNED, and FR-02 does not give lost reports a status. | Remove status from LostItemReport, or create a small separate enumeration if the team adds a requirement for it. |

## 2. Name or responsibility differences

| Class | Problem | My suggestion |
| --- | --- | --- |
| LostItemReport / FoundItemReport | The same data has different names: lastSeenLocation vs foundLocation, dateLost vs dateTimeFound. MatchingService.calculateScore() compares exactly these fields (FR-09), so they should match. | If we add ItemReport, put category, description, location and date in the parent with one name each. |
| LostItemReport.itemName (Member 1) | FR-02 does not ask for an item name (only category, description, location, date, photo and private details). FoundItemReport has no item name either, so matching cannot use it. | Remove itemName, or add it to both reports and to FR-02 and FR-06. |
| Notification.recipientEmail (Member 2) — raised by Member 3 | Member 3 said this repeats User.email. | Keep it. FR-04 sends the email to the owner of the lost report, and D2 stores the owner's contact email in the lost report (LostItemReport.contactEmail), which may differ from the login email. recipientEmail is copied from contactEmail when the notification is created. |
| Claim.claimantAusId (Member 3) | Claim is already linked to ItemOwner, which has ausId. | Keep it, for the same reason as above. FR-05 says the claimant types in their AUS ID, and the officer compares the typed value (FR-11, FR-12). Mention this in the integration notes. |
| VerificationRecord.decision: ClaimStatus (Member 3) | The type allows PENDING_VERIFICATION and RELEASED, which are not decisions. | Add a note that decision is only APPROVED or REJECTED, or change it to approved: boolean. |

## 3. Missing classes

| Missing Class | Why it is needed (use case or requirement) |
| --- | --- |
| No missing class | Every use case from UC-01 to UC-15 is supported by at least one class. |
| Missing methods: list all pending claims and all claim history | UC-11 / FR-11 (view all pending claims) and UC-15 / FR-15 (view claim history) work across all claims, but every method in Member 3's model works on one claim. Suggest adding + viewPendingClaims(): List<Claim> and + viewClaimHistory(): List<VerificationRecord> to CampusSecurityOfficer, in place of approveClaim() and rejectClaim(). |
| Missing link: who changes the found item's status | The DFD has "Item Status Update" from P6 to D3. I agree with Member 3: Claim.approve() calls FoundItemReport.updateStatus(CLAIMED), and ReleaseRecord.confirmHandover() calls FoundItemReport.updateStatus(RETURNED). This also makes FR-08 work, because my edit() and withdraw() refuse once the item is CLAIMED. |

## 4. Attributes and methods (consistency, data types, visibility)

| Class | Problem | My suggestion |
| --- | --- | --- |
| Finder.submitFoundReport(): boolean (Member 1) | No parameter, unlike ItemOwner.reportLostItem(report: LostItemReport). | Change to submitFoundReport(report: FoundItemReport): boolean. |
| LostItemReport.update() and withdraw() (Member 1) | No requirement allows editing or withdrawing a lost report (FR-02 is submit only; FR-08 is only about found reports). withdraw() also returns void, while FoundItemReport.withdraw() returns boolean. | Remove them, or add a requirement. If kept, use edit() and withdraw(): boolean to match FoundItemReport. |
| DisputeCase.resolve(approved: Claim, officer: CampusSecurityOfficer): void (Member 3) | FR-16 says the resolution must be recorded in the claim history, but the diagram does not show how the losing claims are rejected. | Add a note: resolve() calls approve() on the winning claim and reject() on every other claim, so each one gets a VerificationRecord. |
| Both models | Visibility, data types, parameters and return types are shown on every attribute and method. | No change. |

## 5. Inheritance

| Relationship | Is it a real is-a relationship? | My suggestion |
| --- | --- | --- |
| ItemOwner, Finder, CampusSecurityOfficer → User (Member 1) | Yes. All three log in the same way (UC-01, FR-01). | Keep it. Add a note that one student can be both an owner and a finder, so these are roles of an account. |
| Proposed: LostItemReport, FoundItemReport → ItemReport (abstract) | Yes. Both are reports of an item with a category, description, location, date and photos, and MatchingService compares them through these shared fields (FR-09). | Add it during integration. It fixes the ItemPhoto composition problem and the naming differences in section 2. |
| Member 3 has no inheritance | Correct. ClaimStatus is an enumeration, and Claim, VerificationRecord, DisputeCase and ReleaseRecord are different kinds of records. | No change. |

## 6. Associations and multiplicities

| Association | Problem | My suggestion |
| --- | --- | --- |
| ItemOwner runs ItemSearch, 1 to 0..* (Member 1) | A search is not stored in any data store, so a lasting association does not fit. | Change it to a dependency (ItemOwner uses ItemSearch), as Member 3 also suggested. |
| Claim is for FoundItemReport, 0..* to 1 (Member 3) | Correct. One item can have many claims, and a claim is for one item (FR-05, FR-16). It matches my FoundItemReport class. | No change. |
| DisputeCase groups Claim, 0..1 to 2..* (Member 3) | Correct. A dispute needs at least two claims for the same item (UC-14). | Add a note that all grouped claims must be for the same FoundItemReport. |
| Claim has VerificationRecord, composition 1 to 0..* (Member 3) | Correct. The decisions belong to their claim (FR-15). | No change. |
| Claim ends with ReleaseRecord, 1 to 0..1 (Member 3) | Correct. Only an approved claim gets a release record (FR-14). | No change. |
| CampusSecurityOfficer records VerificationRecord, 1 to 0..* (Member 3) | Correct. FR-15 needs the officer who made each decision. | No change. |

## 7. Unnecessary or redundant classes

| Class | Why it can be removed or merged |
| --- | --- |
| ReleaseRecord (Member 3) | Member 3 asked whether to keep it. I suggest keeping it: FR-14 has a "ready for release" step, and the handover is the moment my FoundItemReport becomes RETURNED (FR-10), so it gives that status change a clear place. |
| ItemSearch (Member 1) | It has only one method, but it groups the search filters from FR-03, so keeping it is fine. |
| Other classes | None are redundant. Each class in both models traces to at least one use case or functional requirement. |
