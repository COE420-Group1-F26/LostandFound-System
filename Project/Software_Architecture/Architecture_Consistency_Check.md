# Architecture Consistency Check

**Checked by:** Mahra Alhammadi (g00098713), Member 2

The architecture in Component_Descriptions.md and Class_Distribution.md was compared with the use cases and requirements (Lab 3), the consolidated class diagram (Lab 5), the sequence diagrams (Lab 6) and the activity diagrams (Lab 7). The drawing of the component diagram itself is checked in Architecture_Review.md (questions 1, 2 and 4).

## A. Lab 3: use cases and requirements

Every use case UC-01 to UC-15 is supported by one component and at least one interface operation.

| Use case | Component | Interface operation | Requirement | Supported? |
| --- | --- | --- | --- | --- |
| UC-01 Log In | CO1 | IAccountService.createAccount, login, logout | FR-01 | Yes |
| UC-02 Report Lost Item | CO2 | IReportService.submitLostReport | FR-02 | Yes |
| UC-03 Search Found Items | CO3 | ISearchService.setFilters, execute | FR-03 | Yes |
| UC-04 View Match Notification | CO4 | IMatchingService.getDetails, markViewed | FR-04 | Yes |
| UC-05 Submit Claim | CO5 | IClaimService.submitClaim | FR-05 | Yes |
| UC-06 Report Found Item | CO2 | IReportService.submitFoundReport | FR-06 | Yes |
| UC-07 Attach Photo to Report | CO2 | IReportService.attachPhoto | FR-07 | Yes |
| UC-08 Edit or Withdraw Found Item Report | CO2 | IReportService.editFoundReport, withdrawFoundReport | FR-08 | Yes |
| UC-09 Generate Match Suggestions | CO4 | IMatchingService.findMatchesForFound | FR-04, FR-09 | Yes |
| UC-10 Check Item Status | CO2 | IReportService.getStatus | FR-10 | Yes |
| UC-11 View Pending Claims | CO5 | IClaimService.getPendingClaims | FR-11 | Yes |
| UC-12 Approve Claim | CO5 | IClaimService.approveClaim, markReadyForRelease | FR-12, FR-14 | Yes |
| UC-13 Reject Claim | CO5 | IClaimService.rejectClaim | FR-13 | Yes |
| UC-14 Resolve Disputed Claim | CO5 | IDisputeService.compareClaims, resolveDispute | FR-16 | Yes |
| UC-15 View Claim History | CO5 | IClaimService.getClaimHistory | FR-15 | Yes |

The security requirements NFR-11 and NFR-13 are supported by IAccountService.isOfficer (used by CO5) and by keeping the identifying details of a claim inside CO5. NFR-10 is supported by keeping the matching logic in CO4, separate from Report Management (CO2).

## B. Lab 5: classes and operations

All 17 classes of the consolidated class diagram are in Class_Distribution.md, each in one component (CO1: 2, CO2: 5, CO3: 1, CO4: 3, CO5: 6). Each interface operation was compared with the class operation it comes from in Project/Object_Modeling/Class_Diagram.

| Interface | Interface operation → Lab 5 class operation | Matches Lab 5? | Note |
| --- | --- | --- | --- |
| IAccountService | createAccount, login, logout → User.createAccount, login, logout | Yes | isOfficer is a service operation of CO1, not a class operation. |
| IReportService | submitLostReport → CampusMember.reportLostItem / ItemReport.submit; submitFoundReport → CampusMember.submitFoundReport; attachPhoto → ItemReport.attachPhoto; editFoundReport → FoundItemReport.edit; withdrawFoundReport → FoundItemReport.withdraw; getStatus, updateStatus → FoundItemReport.getStatus, updateStatus | Yes | The interface adds reportId so the caller can name the report. Return types are the same as in Lab 5. |
| IReportQuery | getFoundReports, getLostReports | Yes | Service operations of CO2 (reading D2 and D3), not class operations. |
| ISearchService | setFilters, execute → ItemSearch.setFilters, execute | Yes | Same names, parameters and return types. |
| IMatchingService | findMatchesForFound, findMatchesForLost → MatchingService; getDetails, markViewed → MatchSuggestion | Yes | getDetails and markViewed take the suggestion as a parameter. |
| IClaimService | submitClaim → Claim.submit; getPendingClaims → CampusSecurityOfficer.viewPendingClaims; approveClaim, rejectClaim → Claim.approve, reject; getClaimHistory → CampusSecurityOfficer.viewClaimHistory; markReadyForRelease → Claim.markReadyForRelease; confirmHandover → ReleaseRecord.confirmHandover | Yes | Interface names describe the service (approveClaim instead of approve); parameters and return types are the same plus the claim or release record. |
| IDisputeService | compareClaims, resolveDispute → DisputeCase.compareClaims, resolve | Yes | resolveDispute takes the dispute as a parameter. |

## C. Lab 6: sequence diagrams

| Diagram | Messages that cross a component boundary | Interface in the component diagram | Consistent? |
| --- | --- | --- | --- |
| UC-03 | Messages 3 and 5: ItemSearch (CO3) calls getStatus() and matches() on each FoundItemReport (CO2) | CO3 requires IReportQuery from CO2 (getFoundReports gives the reports to check) | Yes |
| UC-09 | Messages 2, 7 and 9: MatchingService (CO4) reads the lost reports, gets the contact email and sets the found item to MATCHED | CO4 requires IReportQuery and IReportService (updateStatus) from CO2 | Yes |
| UC-09 | Messages 14 and 15: Notification (CO4) sends the email and gets the delivery status | CO4 requires ISendEmail from the Email Service | Yes |
| UC-14 | Message 1: viewPendingClaims() on CampusSecurityOfficer; messages 3 to 16: DisputeCase, Claim and VerificationRecord | All in CO5 (CampusSecurityOfficer was placed in CO5 for this reason) | Yes |
| UC-14 | Message 10: Claim (CO5) calls updateStatus(CLAIMED) on FoundItemReport (CO2) | CO5 requires IReportService from CO2 | Yes |

In every diagram the actor's first message goes to the control object (ItemSearch, MatchingService, DisputeCase). In the architecture this is the Web Interface calling ISearchService, IMatchingService and IDisputeService.

## D. Lab 7: activity diagrams

| Diagram | Swimlanes | Component | Consistent? |
| --- | --- | --- | --- |
| AD UC-03 | Item Owner, ItemSearch, FoundItemReport | Item Owner uses the Web Interface; ItemSearch is in CO3; FoundItemReport is in CO2 | Yes |
| AD UC-09 | Finder, MatchingService, Notification, Email / Notification Service | MatchingService and Notification are in CO4; the Email Service is the external system behind ISendEmail | Yes |
| AD UC-14 | Campus Security Officer, DisputeCase, Claim, FoundItemReport | DisputeCase and Claim are in CO5; FoundItemReport is in CO2 | Yes |

Every swimlane that is a class belongs to exactly one component, and every hand-over between lanes of different components has a matching interface.

## Problems found and how they were fixed

No mismatch needed a change to an earlier artifact. Two points are recorded:

1. Some interface operation names differ from the class operation names (for example approveClaim for Claim.approve, getPendingClaims for CampusSecurityOfficer.viewPendingClaims), and some add the target object as a parameter. This is expected, because an interface is called from outside the component; section B lists every mapping.
2. CampusSecurityOfficer (CO5) is a subclass of User (CO1). This is shown as the dependency of CO5 on CO1 through IAccountService, so the dependencies still go one way.
