# Class Distribution

**Prepared by:** Mahra Alhammadi (g00098713), Member 2

All 17 classes of the consolidated Lab 5 class diagram (Project/Object_Modeling/Class_Diagram) are assigned to one primary component.

## 1. Components

| ID | Component Name | Responsibilities | Included Classes |
| --- | --- | --- | --- |
| CO1 | Account Management | Accounts, login and logout, officer check | User, CampusMember |
| CO2 | Report Management | Lost and found reports, photos, found item status | ItemReport, LostItemReport, FoundItemReport, ItemPhoto, ReportStatus |
| CO3 | Item Search | Search filters and searching the found reports | ItemSearch |
| CO4 | Matching and Notification | Match scoring and ranking, email notification of owners | MatchingService, MatchSuggestion, Notification |
| CO5 | Claim Verification | Claims, officer decisions, claim history, disputes, release | CampusSecurityOfficer, Claim, ClaimStatus, VerificationRecord, DisputeCase, ReleaseRecord |

## 2. Class by class

| Class ID | Class Name | Object Type | Component | Why it is there |
| --- | --- | --- | --- | --- |
| C01 | User (abstract) | Entity | CO1 | Account data and login operations |
| C02 | CampusMember | Entity | CO1 | The owner and finder account |
| C04 | CampusSecurityOfficer | Entity | CO5 | Its operations are the claim views (viewPendingClaims, viewClaimHistory) and it is the officer parameter of approve and reject. It extends User (CO1), which is the dependency of CO5 on CO1. |
| C05 | LostItemReport | Entity | CO2 | Report data |
| C06 | ItemSearch | Control | CO3 | Runs the search |
| C07 | ItemReport (abstract) | Entity | CO2 | Data shared by both reports |
| C10 | FoundItemReport | Entity | CO2 | Report data and status |
| C11 | ItemPhoto | Entity | CO2 | Part of a report (composition) |
| C12 | ReportStatus | Enumeration | CO2 | Status of a found item |
| C13 | MatchSuggestion | Entity | CO4 | Result of matching |
| C14 | MatchingService | Control | CO4 | Runs the matching |
| C15 | Notification | Entity | CO4 | Created from a match and sent to the owner |
| C20 | Claim | Entity | CO5 | A request to collect an item |
| C21 | ClaimStatus | Enumeration | CO5 | Status of a claim |
| C22 | VerificationRecord | Entity | CO5 | A decision in the claim history |
| C23 | DisputeCase | Entity | CO5 | Groups competing claims |
| C24 | ReleaseRecord | Entity | CO5 | Handover of an approved item |

Total: 17 classes (CO1: 2, CO2: 5, CO3: 1, CO4: 3, CO5: 6). Every class appears once.
