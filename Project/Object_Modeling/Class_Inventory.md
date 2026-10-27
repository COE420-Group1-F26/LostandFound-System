# Class Inventory and Consolidation Record

**Prepared by:** Mahra Alhammadi (g00098713), Member 1

**Inputs:** the three individual inventories and class diagrams (Project/Object_Modeling/Individual_Models/Member1 to Member3), the written reviews in Review_of_Others.md, and the open design questions in each inventory. Every decision is explained in Integration_Notes.md.

Class IDs are kept from the individual inventories. C07 is new (added during integration). C03 was merged into C02.

## 1. Approved classes

| Class ID | Class Name | Object Type | Purpose/Responsibility | Related Use Case(s) | Source (Member) |
| --- | --- | --- | --- | --- | --- |
| C01 | User (abstract) | Entity | Account data kept in D1 User Accounts (AUS ID, name, email, password hash); create account, log in, log out. | UC-01 | Member 1 |
| C02 | CampusMember | Entity | The account of an AUS student or staff member. Plays the **owner** role (reports lost items, searches, claims) and the **finder** role (reports found items). Replaces ItemOwner and Finder. | UC-01 to UC-10 | Member 1 (merged during integration) |
| C04 | CampusSecurityOfficer | Entity | The account of a security officer. Views pending claims and the claim history, records decisions and resolves disputes. | UC-01, UC-11 to UC-15 | Member 1 |
| C05 | LostItemReport | Entity | One lost item report, including private identifying details not shown to other users. Stored in D2. | UC-02 | Member 1 |
| C06 | ItemSearch | Control | Holds the search filters (keyword, category, location, date range) and runs the search over the found items. | UC-03 | Member 1 |
| C07 | ItemReport (abstract) | Entity | Data and operations shared by lost and found reports: category, description, location, date, status and photos. | UC-02, UC-06, UC-07, UC-08, UC-10 | Added during integration |
| C10 | FoundItemReport | Entity | One found item report, with its color. Stored in D3. | UC-06, UC-07, UC-08, UC-10 | Member 2 |
| C11 | ItemPhoto | Entity | A photo of an item. Exists only as part of its report. | UC-02, UC-07 | Member 2 |
| C12 | ReportStatus | Enumeration | PENDING, MATCHED, CLAIMED, RETURNED, WITHDRAWN. | UC-08, UC-10 | Member 2 |
| C13 | MatchSuggestion | Entity | A possible pair of one lost and one found report, with a score and rank. Stored in D4. | UC-04, UC-09 | Member 2 |
| C14 | MatchingService | Control | Compares lost and found reports, ranks the suggestions and triggers the owner's notification. | UC-09 | Member 2 |
| C15 | Notification | Entity | The email sent to the owner of a lost report when a possible match is found. | UC-04 | Member 2 |
| C20 | Claim | Entity | A request to collect a found item, with the AUS ID and identifying details entered by the claimant. Stored in D5. | UC-05, UC-11, UC-12, UC-13 | Member 3 |
| C21 | ClaimStatus | Enumeration | PENDING_VERIFICATION, APPROVED, REJECTED, RELEASED. | UC-05, UC-12, UC-13 | Member 3 |
| C22 | VerificationRecord | Entity | One approve or reject decision, with the officer, notes and date and time. Together they form the claim history. | UC-12, UC-13, UC-15 | Member 3 |
| C23 | DisputeCase | Entity | Groups two or more claims for the same item so the officer can compare them and approve one. | UC-14 | Member 3 |
| C24 | ReleaseRecord | Entity | Created when an approved claim is ready for pickup; completed when the item is handed over. | UC-12 | Member 3 |

Total: 17 classes (13 entity, 2 control, 2 enumeration).

## 2. Duplicate or overlapping classes identified and resolved

| Classes | Members | Resolution | Reason |
| --- | --- | --- | --- |
| ItemPhoto as a composition part of LostItemReport and of FoundItemReport | Member 1, Member 2 | ItemPhoto becomes a composition part of the new parent class ItemReport (1 to 0..3), with a note limiting lost reports to 1 photo. | A class cannot be the part of two different compositions. Raised by Member 1, Member 2 and Member 3. |
| LostItemReport and FoundItemReport | Member 1, Member 2 | Shared attributes and operations moved to the abstract class ItemReport; each subclass keeps only what is different. | Same data (category, description, location, date, photos, status) and same life cycle. Raised by Member 1 and Member 3. |
| ItemOwner and Finder | Member 1 | Merged into CampusMember; "owner" and "finder" are now role names on the associations. | FR-02, FR-03 and FR-05 say "a logged-in user": one AUS account can both lose and find items, which two subclasses cannot represent. Raised by Member 3. |
| Approve and reject operations in CampusSecurityOfficer and in Claim | Member 1, Member 3 | Kept only in Claim (approve and reject take the officer as a parameter). | The operations change the claim status and create the VerificationRecord, which belong to Claim. Raised by Member 1 and Member 3. |
| ReportStatus used by both report types | Member 1, Member 2 | One ReportStatus, used by ItemReport, with a note that lost reports never become CLAIMED. | Avoids two almost identical enumerations. Raised by Member 3. |

## 3. Classes removed or merged

| Class | Action | Reason |
| --- | --- | --- |
| C02 ItemOwner | Renamed to CampusMember | See section 2. The ID C02 is kept. |
| C03 Finder | Merged into C02 CampusMember | See section 2. The finder role appears as a role name. |

No other class was removed. ReleaseRecord was reviewed as a possible attribute group of Claim and kept (see Integration_Notes.md, section 6).

## 4. Classes added during integration

| Class | Purpose | Why it was missing |
| --- | --- | --- |
| C07 ItemReport (abstract) | Parent class of LostItemReport and FoundItemReport; owns the photos and the status. | No individual model could add it alone, because the two report classes belonged to different members. |
