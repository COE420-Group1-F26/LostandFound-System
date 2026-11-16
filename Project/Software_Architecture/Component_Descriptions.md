# Component Descriptions

**Prepared by:** Mahra Alhammadi (g00098713), Member 2

Architectural style: layered architecture (Presentation, Application, Data). The five components below form the Application layer. The Web Interface (Presentation), the Database (Data) and the Email Service (external) are outside the five components.

## CO1 Account Management

**Classes:** User, CampusMember

**Responsibilities:** create AUS accounts, log users in and out, and tell the other components whether an account belongs to a campus security officer. Applies the sign-in rules (15 minutes of inactivity for officers, lock after 5 failed attempts).

**Supported functionality:** UC-01 (FR-01); the login part of every other use case; NFR-11, NFR-12, NFR-15.

**Provided interface:** IAccountService

**Required interfaces:** IReportService (CampusMember.reportLostItem and submitFoundReport pass the report to Report Management), IDataAccess (D1 User Accounts)

## CO2 Report Management

**Classes:** ItemReport, LostItemReport, FoundItemReport, ItemPhoto, ReportStatus

**Responsibilities:** store lost and found reports with their photos, let the finder edit or withdraw a found report until it is claimed, hide the private details of a lost report from other users, and keep the status of each found item (PENDING, MATCHED, CLAIMED, RETURNED, WITHDRAWN). It uses no other component.

**Supported functionality:** UC-02, UC-06, UC-07, UC-08, UC-10 (FR-02, FR-06, FR-07, FR-08, FR-10); NFR-05, NFR-08, NFR-09, NFR-13.

**Provided interfaces:** IReportService, IReportQuery

**Required interfaces:** IDataAccess (D2 Lost Reports, D3 Found Reports)

## CO3 Item Search

**Classes:** ItemSearch

**Responsibilities:** hold the search filters (keyword, category, location, date range), run the search over the found reports and return the results newest first.

**Supported functionality:** UC-03 (FR-03); NFR-01.

**Provided interface:** ISearchService

**Required interfaces:** IReportQuery (to read the found reports)

## CO4 Matching and Notification

**Classes:** MatchingService, MatchSuggestion, Notification

**Responsibilities:** compare a found report with the lost reports (or the reverse), score and rank the possible matches, set the found item to MATCHED, create the notification for the owner of each lost report, send it by email and record whether it was delivered, and let the owner open a suggestion.

**Supported functionality:** UC-04, UC-09 (FR-04, FR-09); NFR-06, NFR-07, NFR-10.

**Provided interface:** IMatchingService

**Required interfaces:** IReportQuery (lost and found reports), IReportService (updateStatus), ISendEmail (Email Service), IDataAccess (D4 Match Suggestions)

## CO5 Claim Verification

**Classes:** CampusSecurityOfficer, Claim, ClaimStatus, VerificationRecord, DisputeCase, ReleaseRecord

**Responsibilities:** accept claims, let the officer view the pending claims, approve or reject a claim and record each decision, keep the claim history, compare and resolve competing claims for the same item, prepare the release and record the handover.

**Supported functionality:** UC-05, UC-11, UC-12, UC-13, UC-14, UC-15 (FR-05, FR-11 to FR-16); NFR-11, NFR-13, NFR-14.

**Provided interfaces:** IClaimService, IDisputeService

**Required interfaces:** IReportService (getStatus and updateStatus of the found item), IAccountService (isOfficer), IDataAccess (D5 Claims)

## Interfaces

| Interface | Provided by | Required by | Operations (name, parameters, return type) |
| --- | --- | --- | --- |
| IAccountService | CO1 | Web Interface, CO5 | createAccount(email: String, password: String): boolean; login(email: String, password: String): boolean; logout(): void; isOfficer(ausId: String): boolean |
| IReportService | CO2 | Web Interface, CO1, CO4, CO5 | submitLostReport(report: LostItemReport): boolean; submitFoundReport(report: FoundItemReport): boolean; attachPhoto(reportId: String, photo: ItemPhoto): boolean; editFoundReport(reportId: String, description: String, color: String, location: String): boolean; withdrawFoundReport(reportId: String): boolean; getStatus(reportId: String): ReportStatus; updateStatus(reportId: String, newStatus: ReportStatus): void |
| IReportQuery | CO2 | CO3, CO4 | getFoundReports(): List<FoundItemReport>; getLostReports(): List<LostItemReport> |
| ISearchService | CO3 | Web Interface | setFilters(keywords: String, category: String, location: String, dateFrom: Date, dateTo: Date): void; execute(): List<FoundItemReport> |
| IMatchingService | CO4 | Web Interface | findMatchesForFound(found: FoundItemReport): List<MatchSuggestion>; findMatchesForLost(lost: LostItemReport): List<MatchSuggestion>; getDetails(match: MatchSuggestion): String; markViewed(match: MatchSuggestion): void |
| IClaimService | CO5 | Web Interface | submitClaim(claim: Claim): boolean; getPendingClaims(): List<Claim>; approveClaim(claim: Claim, officer: CampusSecurityOfficer, notes: String): VerificationRecord; rejectClaim(claim: Claim, officer: CampusSecurityOfficer, reason: String): VerificationRecord; getClaimHistory(): List<VerificationRecord>; markReadyForRelease(claim: Claim): ReleaseRecord; confirmHandover(release: ReleaseRecord, ausId: String): boolean |
| IDisputeService | CO5 | Web Interface | compareClaims(dispute: DisputeCase): List<Claim>; resolveDispute(dispute: DisputeCase, approved: Claim, officer: CampusSecurityOfficer): void |
| ISendEmail | Email Service (external) | CO4 | sendEmail(email: String, message: String): boolean |
| IDataAccess | Database (Data layer) | CO1, CO2, CO4, CO5 | save(record: Object): boolean; findById(recordType: String, id: String): Object; findAll(recordType: String): List<Object> |

Every operation is taken from a Lab 5 class operation (see Class_Distribution.md). The only operations that are not class operations are isOfficer, getFoundReports and getLostReports (and the two IDataAccess reads), which are service operations of the component; the Lab 5 class diagram does not change.
