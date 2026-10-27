# Integration Notes

**Prepared by:** Mahra Alhammadi (g00098713), Member 1

This file records the significant changes made while consolidating the three individual class models into one team model. Member 2 draws the consolidated class diagram from section 7; Member 3 checks it in Class_Model_Consistency_Check.md.

**Exercise 3 steps covered:** review of all individual models (Review_of_Others.md files); duplicate classes (Class_Inventory.md section 2); names and responsibilities (section 2 below); missing classes (Class_Inventory.md section 4); attributes and methods (section 3); inheritance (section 4); associations and multiplicities (section 5); redundant classes (Class_Inventory.md section 3); conflicting decisions (section 6); complete system (section 7 covers UC-01 to UC-15).

## 1. Significant modifications made during integration

| No. | Change | Classes Affected | Reason | Raised by |
| --- | --- | --- | --- | --- |
| 1 | Added the abstract class ItemReport as the parent of LostItemReport and FoundItemReport. | ItemReport, LostItemReport, FoundItemReport | Both reports share the same data and life cycle, and this fixes the photo composition problem. | Member 1, Member 3 |
| 2 | ItemPhoto is now a composition part of ItemReport (1 to 0..3), with a note: lost report at most 1 photo (FR-02), found report up to 3 (FR-07). | ItemPhoto, ItemReport | One class cannot be the part of two compositions. | Member 1, Member 2, Member 3 |
| 3 | Merged ItemOwner and Finder into CampusMember; "owner" and "finder" become role names on the associations. | CampusMember, and every association that used ItemOwner or Finder | One AUS account can lose and find items (FR-02, FR-03, FR-05, FR-06). | Member 3 |
| 4 | Approve and reject are kept only in Claim; removed from CampusSecurityOfficer. | Claim, CampusSecurityOfficer | The same behaviour was in two classes. | Member 1, Member 3 |
| 5 | Added viewPendingClaims(): List\<Claim\> and viewClaimHistory(): List\<VerificationRecord\> to CampusSecurityOfficer. | CampusSecurityOfficer | UC-11/FR-11 and UC-15/FR-15 had no operation that lists all pending claims or the full history. | Member 1 |
| 6 | Added createAccount(email: String, password: String): boolean to User and removed changePassword(). | User | FR-01 asks for creating an account; no requirement asks for changing the password. | Member 1, Member 3 |
| 7 | The ItemOwner–ItemSearch association became a dependency (CampusMember uses ItemSearch). | CampusMember, ItemSearch | A search is not stored in any data store, so there is no lasting link. | Member 3 |
| 8 | Claim.approve() sets the found item's status to CLAIMED; ReleaseRecord.confirmHandover() sets it to RETURNED. Shown as a UML note. | Claim, ReleaseRecord, FoundItemReport | Matches the DFD flow "Item Status Update" from P6 to D3 and the statuses in FR-10. | Member 3 |
| 9 | Removed Notification.recipientEmail, LostItemReport.contactEmail and LostItemReport.itemName. | Notification, LostItemReport | The email is already in User and reached through the association; itemName is not in FR-02 (category and description describe the item). | Member 3, Member 1 |

## 2. Naming and responsibility differences resolved

| Item | Version A | Version B | Final decision |
| --- | --- | --- | --- |
| Edit operation | LostItemReport.update() (Member 1) | FoundItemReport.edit() (Member 2) | edit() in both subclasses |
| Location attribute | lastSeenLocation (Member 1) | foundLocation (Member 2) | location in ItemReport (the place where the item was lost or found) |
| Date attribute | dateLost (Member 1) | dateTimeFound (Member 2) | eventDate in ItemReport (the date lost, or the date and time found) |
| Users who report items | ItemOwner and Finder (Member 1) | — | CampusMember with role names owner and finder |
| AUS ID in a claim | User.ausId (Member 1) | Claim.claimantAusId (Member 3) | Keep both: claimantAusId is the value the claimant types in for verification (FR-05) |
| Private details | LostItemReport.privateDetails (Member 1) | Claim.identifyingDetails (Member 3) | Keep both: one is written in the lost report, the other is given when claiming |

## 3. Attribute and method consistency changes

| Class | Change | Reason |
| --- | --- | --- |
| ItemReport | Holds reportId, category, description, location, eventDate, submittedOn and status, and the operations submit(), attachPhoto(), withdraw(), getStatus() and updateStatus(). | Moved up from both report classes. |
| ItemReport | withdraw() returns boolean. | FR-08: withdrawing can fail once the item is claimed. Member 1 had void, Member 2 had boolean. |
| LostItemReport | Keeps privateDetails, edit(description, location) and getPublicDetails(): String. | getPublicDetails() hides the private details from other users (FR-02). |
| FoundItemReport | Keeps color, edit(description, color, location) and the private isEditable(). | FR-06 and FR-08. |
| CampusMember | submitFoundReport(report: FoundItemReport): boolean now takes a parameter. | Same style as reportLostItem(report: LostItemReport). Raised by Member 3. |
| CampusSecurityOfficer | approveClaim() and rejectClaim() removed; viewPendingClaims() and viewClaimHistory() added. | See section 1, rows 4 and 5. |
| User | createAccount() added; changePassword() removed. | FR-01. |
| Notification | recipientEmail removed. | Duplicate of User.email. |

## 4. Inheritance review

| Relationship | Kept or removed | Reason |
| --- | --- | --- |
| CampusMember → User | Kept | A campus member is a user that logs in with an AUS email (UC-01, FR-01). |
| CampusSecurityOfficer → User | Kept | An officer is a user that logs in the same way. |
| ItemOwner → User, Finder → User | Replaced | The two subclasses were merged into CampusMember (section 1, row 3). |
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
| CampusMember ⇢ ItemSearch | dependency | Was an association (section 1, row 7). |
| Other associations from Member 2 and Member 3 | Unchanged | All are named, have both multiplicities and were checked against the requirements in the reviews. |

## 6. Conflicting design decisions

| Issue | Options | Decision | Reason |
| --- | --- | --- | --- |
| Photos owned by two classes | Separate LostItemPhoto and FoundItemPhoto classes, or one ItemPhoto owned by a parent class | Parent class ItemReport | Removes the duplicated report attributes at the same time, and gives a genuine inheritance relationship. |
| Owner and finder as subclasses | Keep ItemOwner and Finder, or merge them with role names | Merge into CampusMember | The requirements let the same logged-in user report lost and found items. |
| Who receives the match notification | Owner only (FR-04), or also the finder (UC-09) | Owner only | FR-04 sends the email to the owner; the finder sees matches through the found report. Member 1 first suggested both and agreed with Member 2's explanation. |
| ReleaseRecord | Keep as a class, or turn into attributes of Claim | Keep as a class | It has its own data and operation, and only approved claims have one (FR-14). |
| Status for lost reports | Separate enumeration, or reuse ReportStatus | Reuse ReportStatus with a note | Lost reports only use PENDING, MATCHED, RETURNED and WITHDRAWN; a second enumeration would almost duplicate the first. |
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

### Classes that changed (draw the others exactly as in the individual diagrams, minus the removed attributes in section 3)

| Class | Attributes | Operations |
| --- | --- | --- |
| \<\<entity\>\> *User* (abstract) | - ausId: String; - name: String; - email: String; - passwordHash: String | + createAccount(email: String, password: String): boolean; + login(email: String, password: String): boolean; + logout(): void |
| \<\<entity\>\> CampusMember | - phone: String | + reportLostItem(report: LostItemReport): boolean; + submitFoundReport(report: FoundItemReport): boolean; + startSearch(): ItemSearch |
| \<\<entity\>\> CampusSecurityOfficer | - badgeNumber: String; - office: String | + viewPendingClaims(): List\<Claim\>; + viewClaimHistory(): List\<VerificationRecord\> |
| \<\<entity\>\> *ItemReport* (abstract) | - reportId: String; - category: String; - description: String; - location: String; - eventDate: Date; - submittedOn: Date; - status: ReportStatus | + submit(): boolean; + attachPhoto(photo: ItemPhoto): boolean; + withdraw(): boolean; + getStatus(): ReportStatus; + updateStatus(newStatus: ReportStatus): void |
| \<\<entity\>\> LostItemReport | - privateDetails: String | + edit(description: String, location: String): boolean; + getPublicDetails(): String |
| \<\<entity\>\> FoundItemReport | - color: String | + edit(description: String, color: String, location: String): boolean; - isEditable(): boolean |
| \<\<entity\>\> Notification | - notificationId: String; - message: String; - detailsLink: String; - sentOn: Date; - delivered: boolean | as in Member 2's diagram |

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
| ItemReport | Dependency | ReportStatus | uses | — |
| Claim | Dependency | ClaimStatus | uses | — |
| VerificationRecord | Dependency | ClaimStatus | uses | — |
| MatchingService | Dependency | Notification | uses (notifyOwner) | — |

### UML notes to include

1. ItemPhoto: a lost report has at most 1 photo (FR-02); a found report has up to 3 (FR-07).
2. ReportStatus: lost reports use only PENDING, MATCHED, RETURNED and WITHDRAWN.
3. CampusMember: the Item Owner and Finder actors are roles played by the same AUS account; the people themselves are actors outside the system.
4. Claim.approve() sets the found item to CLAIMED; ReleaseRecord.confirmHandover() sets it to RETURNED (DFD flow Item Status Update, FR-10).
5. The existing notes from the three individual diagrams (data stores, FR-14 release rule, claim history, dispute rule, matching score).
