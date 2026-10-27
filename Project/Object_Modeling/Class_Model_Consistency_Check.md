# Class Model Consistency and Traceability Check

**Checked by:** Hamda Bahakeem (g00100146) — Member 3

**What I checked:** Class_Inventory.md and Integration_Notes.md (Member 1), the consolidated class diagram (Member 2), the use cases, the functional requirements and the Level-0 DFD.

## 1. Use cases vs classes (every use case must be supported by at least one class)

| Use Case | Classes that support it | Covered? |
| --- | --- | --- |
| UC-01 Log In | User, CampusMember, CampusSecurityOfficer | Yes |
| UC-02 Report Lost Item | CampusMember (owner), LostItemReport, ItemReport, ItemPhoto | Yes |
| UC-03 Search Found Items | CampusMember, ItemSearch, FoundItemReport | Yes |
| UC-04 View Match Notification | Notification, MatchSuggestion, CampusMember (owner) | Yes |
| UC-05 Submit Claim | CampusMember (owner), Claim, ClaimStatus, FoundItemReport | Yes |
| UC-06 Report Found Item | CampusMember (finder), FoundItemReport, ItemReport | Yes |
| UC-07 Attach Photo to Report | ItemReport (attachPhoto()), ItemPhoto | Yes |
| UC-08 Edit or Withdraw Found Item Report | FoundItemReport (edit(), withdraw(), isEditable()), ReportStatus | Yes |
| UC-09 Generate Match Suggestions | MatchingService, MatchSuggestion, LostItemReport, FoundItemReport, Notification | Yes |
| UC-10 Check Item Status | FoundItemReport (getStatus()), ReportStatus | Yes |
| UC-11 View Pending Claims | CampusSecurityOfficer (viewPendingClaims()), Claim, FoundItemReport | Yes |
| UC-12 Approve Claim | Claim (approve()), VerificationRecord, ReleaseRecord, CampusSecurityOfficer | Yes |
| UC-13 Reject Claim | Claim (reject()), VerificationRecord, CampusSecurityOfficer | Yes |
| UC-14 Resolve Disputed Claim | DisputeCase, Claim, VerificationRecord, CampusSecurityOfficer | Yes |
| UC-15 View Claim History | CampusSecurityOfficer (viewClaimHistory()), VerificationRecord | Yes |

All 15 use cases have at least one class.

## 2. Functional requirements vs classes

| Functional Requirement | Classes (and attributes or methods) that satisfy it | Covered? |
| --- | --- | --- |
| FR-01 | User (email, passwordHash, createAccount(), login()) | Yes |
| FR-02 | LostItemReport (privateDetails, getPublicDetails()), ItemReport (category, description, location, eventDate, submit()), ItemPhoto (max 1 photo, see note) | Yes |
| FR-03 | ItemSearch (keywords, category, location, dateFrom, dateTo, execute()) | Yes |
| FR-04 | Notification (recipientEmail, detailsLink, send()), MatchingService.notifyOwner() | Yes |
| FR-05 | Claim (claimantAusId, identifyingDetails, submit()), ClaimStatus.PENDING_VERIFICATION | Yes |
| FR-06 | FoundItemReport (color), ItemReport (category, description, location, eventDate) | Yes |
| FR-07 | ItemReport ◆— ItemPhoto (0..3), attachPhoto() | Yes |
| FR-08 | FoundItemReport.edit(), withdraw(), isEditable() (refuses once CLAIMED, see note) | Yes |
| FR-09 | MatchingService (calculateScore(), rankMatches()), MatchSuggestion (score, rank) | Yes |
| FR-10 | FoundItemReport.getStatus(), ReportStatus | Yes |
| FR-11 | CampusSecurityOfficer.viewPendingClaims(), Claim (claimantAusId, identifyingDetails) linked to FoundItemReport | Yes |
| FR-12 | Claim.approve(), ClaimStatus.APPROVED, VerificationRecord | Yes |
| FR-13 | Claim.reject(), ClaimStatus.REJECTED, VerificationRecord | Yes |
| FR-14 | Claim.markReadyForRelease() (only when APPROVED), ReleaseRecord | Yes |
| FR-15 | VerificationRecord (decision, decidedOn) linked to CampusSecurityOfficer, viewClaimHistory() | Yes |
| FR-16 | DisputeCase (compareClaims(), resolve()) groups 2..* claims | Yes |

All 16 functional requirements are covered.

## 3. Data stores (DFD) vs classes

| Data Store | Class(es) that hold this data | Attributes cover the data described? |
| --- | --- | --- |
| D1 User Accounts | User, CampusMember, CampusSecurityOfficer | Yes, User has ausId, email and passwordHash |
| D2 Lost Item Reports | LostItemReport, ItemReport, ItemPhoto | Yes, category, description, location, date, photo, private details and contact email |
| D3 Found Item Reports | FoundItemReport, ItemReport, ItemPhoto, ReportStatus | Yes, details, photos and status |
| D4 Match Suggestions | MatchSuggestion | Yes, the lost/found pair and the score |
| D5 Claims | Claim, ClaimStatus, VerificationRecord, DisputeCase, ReleaseRecord | Yes, claims, decisions, disputes and the history |

Every data store has a class, and every entity class is saved in a data store (ItemSearch and MatchingService are control classes, so they aren't stored).

## 4. Relationships and multiplicities in the consolidated diagram

I checked every line in Consolidated_Class_Diagram.drawio against section 7 of Integration_Notes.md. All 26 relationships are there with the right type, name and multiplicities.

| Association | Name | Multiplicities | Justification from the requirements | Correct? |
| --- | --- | --- | --- | --- |
| CampusMember → User | generalization | — | A campus member logs in with an AUS account (UC-01, FR-01) | Yes |
| CampusSecurityOfficer → User | generalization | — | An officer logs in the same way (UC-01, FR-01) | Yes |
| LostItemReport → ItemReport | generalization | — | A lost report is a kind of item report (FR-02) | Yes |
| FoundItemReport → ItemReport | generalization | — | A found report is a kind of item report (FR-06) | Yes |
| CampusMember (owner) — LostItemReport | reports | 1 to 0..* | One owner can report many items, each report has one owner (FR-02) | Yes |
| CampusMember (finder) — FoundItemReport | submits | 1 to 0..* | One finder can hand in many items (FR-06) | Yes |
| CampusMember (owner) — Claim | submits | 1 to 0..* | One owner can make many claims, each claim is from one person (FR-05) | Yes |
| Notification — CampusMember (owner) | is sent to | 0..* to 1 | The email goes to the owner of the lost report (FR-04) | Yes |
| ItemReport ◆— ItemPhoto | has | 1 to 0..3 | Up to 3 photos, and a photo can't exist without its report (FR-02, FR-07) | Yes |
| MatchingService — MatchSuggestion | creates | 1 to 0..* | Matching makes ranked suggestions (FR-09) | Yes |
| MatchSuggestion — LostItemReport | is for | 0..* to 1 | Each suggestion has one lost report (FR-09) | Yes |
| MatchSuggestion — FoundItemReport | points to | 0..* to 1 | Each suggestion has one found report (FR-09) | Yes |
| Notification — MatchSuggestion | announces | 0..1 to 1 | One email per match (FR-04) | Yes |
| ItemSearch — FoundItemReport | looks through | 0..* to 0..* | A search can return many found items (FR-03) | Yes |
| Claim — FoundItemReport | is for | 0..* to 1 | Many people can claim the same item, each claim is for one item (FR-05, FR-16) | Yes |
| Claim ◆— VerificationRecord | has | 1 to 0..* | Every decision is kept in the claim's history (FR-15) | Yes |
| VerificationRecord — CampusSecurityOfficer | records | 0..* to 1 | The history shows which officer decided (FR-15) | Yes |
| DisputeCase — Claim | groups | 0..1 to 2..* | A dispute needs at least 2 claims for the same item (UC-14, FR-16) | Yes |
| CampusSecurityOfficer — DisputeCase | resolves | 0..1 to 0..* | The officer decides the dispute (FR-16) | Yes |
| Claim — ReleaseRecord | ends with | 1 to 0..1 | Only approved claims get released (FR-14) | Yes |
| CampusMember ⇢ ItemSearch | uses (dependency) | — | A search isn't saved anywhere, so it's not a lasting link (FR-03) | Yes |
| CampusSecurityOfficer ⇢ Claim | uses (dependency) | — | viewPendingClaims() returns claims (FR-11) | Yes |
| FoundItemReport ⇢ ReportStatus | uses (dependency) | — | Type of the status attribute (FR-10) | Yes |
| Claim ⇢ ClaimStatus | uses (dependency) | — | Type of the status attribute (FR-05) | Yes |
| VerificationRecord ⇢ ClaimStatus | uses (dependency) | — | Type of the decision attribute (FR-15) | Yes |
| MatchingService ⇢ Notification | uses (dependency) | — | notifyOwner() creates the notification (FR-04) | Yes |

Small things I noticed (not wrong, just cosmetic): the MatchingService ⇢ Notification and VerificationRecord ⇢ ClaimStatus arrows don't have the «uses» label like the other dependencies, and the 0..* next to ItemSearch on "looks through" is a bit hidden by a line crossing. These don't change the model.

## 5. Handout requirements for the consolidated diagram

| Requirement | Result |
| --- | --- |
| Approved classes from the individual models included | Yes, all 17 classes in Class_Inventory.md are in the diagram, each one only once |
| Attributes and methods shown | Yes, every class has its attributes and methods (enumerations show their values) |
| Visibility shown | Yes, + and - on every attribute and method |
| Data types, parameters and return types shown | Yes, for example + approve(officer: CampusSecurityOfficer, notes: String): VerificationRecord |
| Meaningful, named associations with multiplicities | Yes, all 16 associations have a name and multiplicities at both ends, and role names (owner, finder) where CampusMember plays a role |
| Genuine inheritance only | Yes, 4 generalizations, all real is-a: CampusMember and CampusSecurityOfficer are users, LostItemReport and FoundItemReport are item reports. User and ItemReport are abstract (italic) |
| Helpful UML notes | Yes, 6 notes (roles of CampusMember, photos and data stores, matching score and email, status updates and release rule, disputes, claim history) |
| Readable and professionally organized | Yes, the classes are grouped into 4 packages (Users and Search, Reports, Matching and Notification, Claims). It's big, so in the report it goes on its own landscape page |
| Created with a UML/modeling tool | Yes, draw.io (.drawio and .png in Class_Diagram/) |
| Consistent with Labs 1-4 | Yes, after the changes in section 6 below |

## 6. Changes needed to earlier labs

| File | Change Needed | Reason |
| --- | --- | --- |
| Project/Data_Flow_Modeling/Level_0_DFD/Level_0_Flows.md | P6 Process and Decide Claims: "FR-05, FR-11 to FR-15" changed to "FR-05, FR-11 to FR-16" | FR-16 was added in Lab 4 but P6 still said FR-11 to FR-15 |
| Project/Data_Flow_Modeling/DFD_Consistency_Check.md | Same change for P6 in section 5 | Same thing |
| Project/Requirements/Use_Cases.md | UC-09 description: the owner of the lost report gets the email, and the finder sees the match through their found report | The team decided the notification goes to the owner only (Integration_Notes.md section 6, FR-04). UC-09 said it notifies the finder |
| Project/Requirements/Use_Cases.md | UC-10 description: added Withdrawn to the list of statuses | ReportStatus has WITHDRAWN (FR-08 lets the finder withdraw a report), and D3 already lists it |
| Project/Requirements/Functional_Requirements.md | FR-10: added Withdrawn to the list of statuses | Same reason |
| Project/Requirements/Functional_Requirements.md | No change: UC-14 not having an FR was already fixed in Lab 4 (FR-16), and DisputeCase covers it | Just noting that I checked it |
| Project/Requirements/Use_Cases.md | No change to the actors: Item Owner and Finder stay as actors | In the class model they are roles of one CampusMember account, but in the use case model they are still two different actors (note on CampusMember) |
