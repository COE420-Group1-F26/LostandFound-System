# Integration Notes

**Prepared by:** Mahra Alhammadi (g00098713), Member 1

This file records the significant changes made while consolidating the three individual class models into one team model. It is based on the three written reviews (Review_of_Others.md by each member). Member 2 draws the consolidated class diagram from section 7; Member 3 checks it in Class_Model_Consistency_Check.md.

**Exercise 3 steps covered:** review of all individual models (the three Review_of_Others.md files); duplicate classes (Class_Inventory.md section 2); names and responsibilities (section 2 below); missing classes (Class_Inventory.md section 4); attributes and methods (section 3); inheritance (section 4); associations and multiplicities (section 5); redundant classes (Class_Inventory.md section 3); conflicting decisions (section 6); complete system (section 7 covers UC-01 to UC-15).

## 1. Significant modifications made during integration

| No. | Change | Classes Affected | Reason | Raised by |
| --- | --- | --- | --- | --- |
| 1 | Added the abstract class ItemReport as the parent of LostItemReport and FoundItemReport. | ItemReport, LostItemReport, FoundItemReport | Both reports share the same data, and MatchingService compares them through it (FR-09). This also fixes the photo composition problem. | Member 1, Member 2, Member 3 |
| 2 | ItemPhoto is now a composition part of ItemReport (1 to 0..3), with a note: lost report at most 1 photo (FR-02), found report up to 3 (FR-07). | ItemPhoto, ItemReport | One class cannot be the part of two compositions. | Member 1, Member 2, Member 3 |
| 3 | Merged ItemOwner and Finder into CampusMember; "owner" and "finder" become role names on the associations. | CampusMember, and every association that used ItemOwner or Finder | One AUS account can lose and find items (FR-02, FR-03, FR-05, FR-06). | Member 3, Member 2 |
| 4 | Approve and reject are kept only in Claim; removed from CampusSecurityOfficer. | Claim, CampusSecurityOfficer | The same behaviour was in two classes. | Member 1, Member 2, Member 3 |
| 5 | Added viewPendingClaims(): List\<Claim\> and viewClaimHistory(): List\<VerificationRecord\> to CampusSecurityOfficer. | CampusSecurityOfficer | UC-11/FR-11 and UC-15/FR-15 work across all claims, but every operation in the claim model worked on one claim. | Member 1, Member 2 |
| 6 | Added createAccount(email: String, password: String): boolean to User and removed changePassword(). | User | FR-01 asks for creating an account; no requirement asks for changing the password. | Member 1, Member 3 |
| 7 | The ItemOwner–ItemSearch association became a dependency (CampusMember uses ItemSearch). | CampusMember, ItemSearch | A search is not stored in any data store, so there is no lasting link. | Member 3, Member 2 |
| 8 | Claim.approve() sets the found item's status to CLAIMED; ReleaseRecord.confirmHandover() sets it to RETURNED. Shown as a UML note. | Claim, ReleaseRecord, FoundItemReport | Matches the DFD flow "Item Status Update" from P6 to D3 and the statuses in FR-10. It also makes FR-08 work, because edit() and withdraw() refuse once the item is CLAIMED. | Member 3, Member 2 |
| 9 | Removed LostItemReport.itemName. | LostItemReport | FR-02 does not ask for an item name, and FoundItemReport has none, so matching cannot use it. | Member 1, Member 2 |
| 10 | Status moved from the lost and found reports to FoundItemReport only; LostItemReport has no status. | LostItemReport, FoundItemReport, ReportStatus | FR-10 gives statuses to found items; FR-02 gives lost reports none, and a lost report is never CLAIMED or RETURNED. | Member 2 |
| 11 | Removed RELEASED from ClaimStatus. | ClaimStatus, ReleaseRecord | FR-05, FR-12 and FR-13 only give Pending Verification, Approved and Rejected. The handover is already recorded by ReleaseRecord and by the item's RETURNED status. | Member 2 |
| 12 | Removed update() and withdraw() from LostItemReport; edit() and withdraw(): boolean exist only on FoundItemReport. | LostItemReport, FoundItemReport | FR-02 only allows submitting a lost report; FR-08 (edit or withdraw) is about found reports. | Member 2, Member 3 |
| 13 | Added UML notes: VerificationRecord.decision is only APPROVED or REJECTED; DisputeCase.resolve() approves the winning claim and rejects every other one; all claims in a dispute are for the same found item. | VerificationRecord, DisputeCase, Claim | Makes FR-15 and FR-16 visible in the diagram. | Member 2 |

## 2. Naming and responsibility differences resolved

| Item | Version A | Version B | Final decision |
| --- | --- | --- | --- |
| Edit operation | LostItemReport.update() (Member 1) | FoundItemReport.edit() (Member 2) | edit() on FoundItemReport only; update() removed (row 12) |
| Location attribute | lastSeenLocation (Member 1) | foundLocation (Member 2) | location in ItemReport (the place where the item was lost or found) |
| Date attribute | dateLost (Member 1) | dateTimeFound (Member 2) | eventDate in ItemReport (the date lost, or the date and time found) |
| Users who report items | ItemOwner and Finder (Member 1) | — | CampusMember with role names owner and finder |
| AUS ID in a claim | User.ausId (Member 1) | Claim.claimantAusId (Member 3) | Keep both: claimantAusId is the value the claimant types in and the officer compares (FR-05, FR-11, FR-12) |
| Private details | LostItemReport.privateDetails (Member 1) | Claim.identifyingDetails (Member 3) | Keep both: one is written in the lost report, the other is given when claiming |
| Email used for notifications | User.email (Member 1) | LostItemReport.contactEmail (Member 1) and Notification.recipientEmail (Member 2) | Keep contactEmail and recipientEmail (see section 6) |

## 3. Attribute and method consistency changes

| Class | Change | Reason |
| --- | --- | --- |
| ItemReport | Holds reportId, category, description, location, eventDate and submittedOn, and the operations submit() and attachPhoto(). | Moved up from both report classes. |
| LostItemReport | Keeps privateDetails and contactEmail, and adds getPublicDetails(): String. itemName, status, update() and withdraw() removed. | getPublicDetails() hides the private details from other users (FR-02). Rows 9, 10 and 12. |
| FoundItemReport | Keeps color, status, edit(description, color, location), withdraw(): boolean, getStatus(), updateStatus() and the private isEditable(). | FR-06, FR-08, FR-10. |
| CampusMember | submitFoundReport(report: FoundItemReport): boolean now takes a parameter. | Same style as reportLostItem(report: LostItemReport). Raised by Member 2 and Member 3. |
| CampusSecurityOfficer | approveClaim() and rejectClaim() removed; viewPendingClaims() and viewClaimHistory() added. | Rows 4 and 5. |
| User | createAccount() added; changePassword() removed. | FR-01. |
| ClaimStatus | RELEASED removed. | Row 11. |

## 4. Inheritance review

| Relationship | Kept or removed | Reason |
| --- | --- | --- |
| CampusMember → User | Kept | A campus member is a user that logs in with an AUS email (UC-01, FR-01). |
| CampusSecurityOfficer → User | Kept | An officer is a user that logs in the same way. |
| ItemOwner → User, Finder → User | Replaced | The two subclasses were merged into CampusMember (row 3). |
| LostItemReport → ItemReport, FoundItemReport → ItemReport | Added | Both are item reports: a lost or found report can be used wherever an item report is expected (substitutability). |
| ReportStatus, ClaimStatus | No inheritance | They are enumerations, not parent classes. |

## 5. Associations and multiplicities review

| Association | Final name and multiplicities | Reason |
| --- | --- | --- |
| CampusMember (owner) — LostItemReport | reports, 1 to 0..* | An owner can report many lost items; each report has one owner. |
| CampusMember (finder) — FoundItemReport | submits, 1 to 0..* | Member 2's association, now with the finder role name. |
| CampusMember (owner) — Claim | submits, 1 to 0..* | Member 3's association, now with the owner role name (FR-05). |
| Notification — CampusMember (owner) | is sent to, 0..* to 1 | FR-04 emails the owner of the lost report. |
| ItemReport ◆— ItemPhoto | has, 1 to 0..3 (composition) | Moved from both report classes (FR-02, FR-07). |
| CampusMember ⇢ ItemSearch | dependency | Was an association (row 7). |
| FoundItemReport ⇢ ReportStatus | dependency | Only found reports have a status (row 10). |
| Other associations from Member 2 and Member 3 | Unchanged | All are named, have both multiplicities and were confirmed in the reviews. |

## 6. Conflicting design decisions

| Issue | Options | Decision | Reason |
| --- | --- | --- | --- |
| Photos owned by two classes | Separate LostItemPhoto and FoundItemPhoto classes, or one ItemPhoto owned by a parent class | Parent class ItemReport | All three reviews preferred it; it also removes the duplicated report attributes and gives a genuine inheritance relationship. |
| Owner and finder | Keep ItemOwner and Finder as subclasses with a note that one student can be both (Member 2), or merge them with role names (Member 3) | Merge into CampusMember | One object cannot be an instance of two subclasses, so a note cannot fix it. The merge carries out Member 2's idea that owner and finder are roles of one account. |
| Who receives the match notification | Owner only (FR-04), or also the finder (UC-09) | Owner only | FR-04 sends the email to the owner; the finder sees matches through the found report. Member 1 first suggested both and agreed with Member 2's explanation. |
| Contact email | Remove LostItemReport.contactEmail and Notification.recipientEmail and use User.email (Member 1, Member 3), or keep them (Member 2) | Keep them | D2 Lost Item Reports stores the owner's contact email, which may differ from the login email. Notification copies it into recipientEmail when it is created. Keeping it keeps the class model consistent with the DFD. |
| Status for lost reports | Reuse ReportStatus, add a separate enumeration, or no status | No status | No requirement gives lost reports a status (FR-02); FR-10 is about found items. |
| ReleaseRecord | Keep as a class, or turn into attributes of Claim | Keep as a class | It has its own data and operation, only approved claims have one (FR-14), and it is the moment the item becomes RETURNED. Member 2 and Member 3 agreed. |
| RELEASED claim status | Keep it, or let the claim stay APPROVED | Remove it | No requirement mentions it, and the handover is recorded elsewhere (row 11). |
| DISPUTED claim status | Add it, or leave claims PENDING_VERIFICATION until the dispute is resolved | Leave it out | No requirement mentions a disputed status (Member 3's decision, accepted). |
| Boundary class for the email service | Add an EmailGateway boundary class, or let Notification.send() represent it | No boundary class | Notification.send() is enough at this level; screens and external interfaces are left to design. |

## 7. Final model for the consolidated diagram (for Member 2)

### Packages

| Package | Classes |
| --- | --- |
| Users and Search | User, CampusMember, CampusSecurityOfficer, ItemSearch |
| Reports | ItemReport, LostItemReport, FoundItemReport, ItemPhoto, ReportStatus |
| Matching and Notification | MatchingService, MatchSuggestion, Notification |
| Claims | Claim, ClaimStatus, VerificationRecord, DisputeCase, ReleaseRecord |

### Classes that changed (draw the others exactly as in the individual diagrams)

| Class | Attributes | Operations |
| --- | --- | --- |
| \<\<entity\>\> *User* (abstract) | - ausId: String; - name: String; - email: String; - passwordHash: String | + createAccount(email: String, password: String): boolean; + login(email: String, password: String): boolean; + logout(): void |
| \<\<entity\>\> CampusMember | - phone: String | + reportLostItem(report: LostItemReport): boolean; + submitFoundReport(report: FoundItemReport): boolean; + startSearch(): ItemSearch |
| \<\<entity\>\> CampusSecurityOfficer | - badgeNumber: String; - office: String | + viewPendingClaims(): List\<Claim\>; + viewClaimHistory(): List\<VerificationRecord\> |
| \<\<entity\>\> *ItemReport* (abstract) | - reportId: String; - category: String; - description: String; - location: String; - eventDate: Date; - submittedOn: Date | + submit(): boolean; + attachPhoto(photo: ItemPhoto): boolean |
| \<\<entity\>\> LostItemReport | - privateDetails: String; - contactEmail: String | + getPublicDetails(): String |
| \<\<entity\>\> FoundItemReport | - color: String; - status: ReportStatus | + edit(description: String, color: String, location: String): boolean; + withdraw(): boolean; + getStatus(): ReportStatus; + updateStatus(newStatus: ReportStatus): void; - isEditable(): boolean |
| \<\<enumeration\>\> ClaimStatus | PENDING_VERIFICATION; APPROVED; REJECTED | — |

### Relationships

| From | Relationship | To | Name / role | Multiplicity (From : To) |
| --- | --- | --- | --- | --- |
| CampusMember | Generalization | User | — | — |
| CampusSecurityOfficer | Generalization | User | — | — |
| LostItemReport | Generalization | ItemReport | — | — |
| FoundItemReport | Generalization | ItemReport | — | — |
| CampusMember | Association | LostItemReport | reports (role: owner) | 1 : 0..* |
| CampusMember | Association | FoundItemReport | submits (role: finder) | 1 : 0..* |
| CampusMember | Association | Claim | submits (role: owner) | 1 : 0..* |
| Notification | Association | CampusMember | is sent to (role: owner) | 0..* : 1 |
| ItemReport | Composition (filled diamond at ItemReport) | ItemPhoto | has | 1 : 0..3 |
| MatchSuggestion | Association | LostItemReport | is for | 0..* : 1 |
| MatchSuggestion | Association | FoundItemReport | points to | 0..* : 1 |
| MatchingService | Association | MatchSuggestion | creates | 1 : 0..* |
| Notification | Association | MatchSuggestion | announces | 0..1 : 1 |
| ItemSearch | Association | FoundItemReport | looks through | 0..* : 0..* |
| Claim | Association | FoundItemReport | is for | 0..* : 1 |
| Claim | Composition (filled diamond at Claim) | VerificationRecord | has | 1 : 0..* |
| VerificationRecord | Association | CampusSecurityOfficer | records | 0..* : 1 |
| DisputeCase | Association | Claim | groups | 0..1 : 2..* |
| CampusSecurityOfficer | Association | DisputeCase | resolves | 0..1 : 0..* |
| Claim | Association | ReleaseRecord | ends with | 1 : 0..1 |
| CampusMember | Dependency (dashed arrow) | ItemSearch | uses | — |
| CampusSecurityOfficer | Dependency | Claim | uses (viewPendingClaims) | — |
| FoundItemReport | Dependency | ReportStatus | uses | — |
| Claim | Dependency | ClaimStatus | uses | — |
| VerificationRecord | Dependency | ClaimStatus | uses | — |
| MatchingService | Dependency | Notification | uses (notifyOwner) | — |

### UML notes to include

1. ItemPhoto: a lost report has at most 1 photo (FR-02); a found report has up to 3 (FR-07).
2. CampusMember: the Item Owner and Finder actors are roles played by the same AUS account; the people themselves are actors outside the system.
3. Claim.approve() sets the found item to CLAIMED; ReleaseRecord.confirmHandover() sets it to RETURNED (DFD flow Item Status Update, FR-10). edit() and withdraw() refuse once the item is CLAIMED (FR-08).
4. VerificationRecord.decision is only APPROVED or REJECTED.
5. DisputeCase: all grouped claims are for the same found item; resolve() approves the winning claim and rejects every other one, so each gets a VerificationRecord (FR-16).
6. Notification.recipientEmail is copied from LostItemReport.contactEmail (D2).
7. The existing notes from the three individual diagrams (data stores, FR-14 release rule, claim history, matching score).
