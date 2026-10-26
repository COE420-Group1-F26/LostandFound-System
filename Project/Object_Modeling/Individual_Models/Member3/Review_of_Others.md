# Review of the Other Individual Models — by Member 3

**Reviewer:** Hamda Bahakeem (g00100146)

**Models reviewed:** Member 1 (Mahra Alhammadi) — users, lost reports and search; Member 2 (Hamda Albahri) — found items, matching and notifications

## 1. Duplicate or overlapping classes

| Class A (owner) | Class B (owner) | What overlaps | My suggestion |
| --- | --- | --- | --- |
| ItemPhoto in LostItemReport (Member 1) | ItemPhoto in FoundItemReport (Member 2) | Both use composition with ItemPhoto, but one class can't be a composition part of two different classes. | Make a parent class ItemReport that has the photos, or split it into LostItemPhoto and FoundItemPhoto. |
| CampusSecurityOfficer approveClaim() / rejectClaim() (Member 1) | Claim approve() / reject() (Member 3) | Approving and rejecting a claim is in two places. | Keep the logic in Claim since it changes the claim status and saves the history. The officer's methods can just call the Claim ones. |
| ReportStatus in LostItemReport (Member 1) | ReportStatus in FoundItemReport (Member 2) | ReportStatus values (PENDING, MATCHED, CLAIMED, RETURNED, WITHDRAWN) come from FR-10, which is about found items. A lost report doesn't really get "claimed" or "returned". | Either agree lost reports use the same statuses, or make a small separate enum for lost reports. |

## 2. Name or responsibility differences

| Class | Problem | My suggestion |
| --- | --- | --- |
| LostItemReport / FoundItemReport | Same kind of info with different names: lastSeenLocation vs foundLocation, dateLost vs dateTimeFound. | Use the same names if we add a parent class, and only keep the ones that are really different. |
| LostItemReport.privateDetails (Member 1) and Claim.identifyingDetails (Member 3) | They sound the same but they're not. One is the details the owner wrote in the lost report, the other is what the person gives when claiming. | Keep both, just mention in the integration notes that they're different. |
| Notification.recipientEmail (Member 2) | The email is already saved in User, and Notification is already linked to ItemOwner. | Remove it or say it's just a copy used when sending. |
| Finder.submitFoundReport(): boolean (Member 1) | It has no parameter, but ItemOwner.reportLostItem(report) does. | Change it to submitFoundReport(report: FoundItemReport): boolean so both work the same way. |

## 3. Missing classes

| Missing Class | Why it is needed (use case or requirement) |
| --- | --- |
| No missing class | Every use case from UC-01 to UC-15 has at least one class. |
| Missing link: who updates the item status after a claim | The DFD has "Item Status Update" going from P6 to D3, and FR-10 shows CLAIMED and RETURNED. No diagram shows which class changes it. I think Claim.approve() should set it to CLAIMED and ReleaseRecord.confirmHandover() should set it to RETURNED. |

## 4. Attributes and methods (consistency, data types, visibility)

| Class | Problem | My suggestion |
| --- | --- | --- |
| LostItemReport.withdraw(): void vs FoundItemReport.withdraw(): boolean | Same method, different return types. FR-08 says withdraw can fail if the item is claimed, so it should return something. | Make both boolean. |
| LostItemReport.update() vs FoundItemReport.edit() | Same idea, different names. | Use edit for both. |
| User.changePassword() (Member 1) | No requirement asks for changing passwords, FR-01 is only create account and log in. | Remove it, or add it to FR-01. |
| Both models | Visibility, data types, parameters and return types are all shown. | No change. |

## 5. Inheritance

| Relationship | Is it a real is-a relationship? | My suggestion |
| --- | --- | --- |
| ItemOwner, Finder, CampusSecurityOfficer → User (Member 1) | Yes, they all log in the same way (UC-01, FR-01). | Keep it. One thing to discuss: the same student can lose something and also find something, but with subclasses one account can only be one type. We should mention how we handle that. |
| Member 2 has no inheritance | Correct, ReportStatus is an enum and the other classes aren't types of each other. | If we add ItemReport as a parent of LostItemReport and FoundItemReport, that would be real inheritance and it also fixes the photo problem. |

## 6. Associations and multiplicities

| Association | Problem | My suggestion |
| --- | --- | --- |
| ItemOwner runs ItemSearch (1 to 0..*) (Member 1) | A search isn't saved anywhere (no data store for it), so a permanent association doesn't really fit. | Make it a dependency (ItemOwner uses ItemSearch). |
| LostItemReport has ItemPhoto (0..1) and FoundItemReport has ItemPhoto (0..3) | Each one is right for its own requirement, but together they break the composition rule. | Fix it the same way as section 1. |
| Notification is sent to ItemOwner (Member 2) | Correct for FR-04, but UC-09 says the Finder is the main actor. | Keep it, and fix the UC-09 wording if the team agrees. |
| MatchSuggestion to FoundItemReport and LostItemReport (0..* to 1) | Correct. | No change. |

## 7. Unnecessary or redundant classes

| Class | Why it can be removed or merged |
| --- | --- |
| ItemSearch (Member 1) | It only has one method, so it could just be a method. But it keeps the 5 search filters from FR-03 together, so I think it's fine to keep. |
| Member 2's classes | All needed, each one connects to FR-04 or FR-06 to FR-10. |
