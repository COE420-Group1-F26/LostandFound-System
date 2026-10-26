# Individual Class Inventory — Member 2

**Contributor:** Hamda Albahri (g00100284)

**Component modeled:** Found Item Reports, Matching and Notifications

**Level-0 Processes:** P3 Manage Found Item Reports, P5 Match Items and Notify Owners

**Data stores:** D3 Found Item Reports, D4 Match Suggestions

**Requirements covered:** FR-04, FR-06 to FR-10

## Classes

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Contributor |
| --- | --- | --- | --- | --- |
| C10 | FoundItemReport | Stores information about a found item, including its category, description, color, location and date/time found, and its current status. Stored in D3. | UC-06, UC-07, UC-08, UC-10 | Hamda Albahri |
| C11 | ItemPhoto | Represents a photo attached to a found item report to help identify the item (up to 3 per report, FR-07). | UC-06, UC-07 | Hamda Albahri |
| C12 | ReportStatus | Defines the allowed report statuses: Pending, Matched, Claimed, Returned, and Withdrawn. | UC-08, UC-10 | Hamda Albahri |
| C13 | MatchSuggestion | Represents a possible match between a lost item report and a found item report, including a matching score and rank. Stored in D4. | UC-09, UC-04 | Hamda Albahri |
| C14 | MatchingService | Compares lost and found item reports and generates ranked match suggestions. | UC-09 | Hamda Albahri |
| C15 | Notification | Represents the email notification sent to an item owner when a possible match is identified. | UC-04 | Hamda Albahri |

## Why Each Class Is Needed

| Class | Justification |
| --- | --- |
| FoundItemReport | A found item requires its own record containing multiple details and a status (FR-06, FR-10), and it has its own life cycle: submit, edit, withdraw (FR-08). |
| ItemPhoto | A report may contain up to 3 photos that help users recognize found items (FR-07). Each photo has its own file name and upload date, so it is more than one attribute. |
| ReportStatus | An enumeration ensures that reports use only valid status values (FR-10, D3). |
| MatchSuggestion | Each suggested match needs to store its associated reports, matching score and rank (FR-09, D4). |
| MatchingService | Matching requires separate logic to compare reports and generate ranked suggestions (FR-09). |
| Notification | Notifications require their own message details, link and delivery information (FR-04). |

## Object Types

- **Entity objects:** FoundItemReport, ItemPhoto, MatchSuggestion, Notification.
- **Control objects:** MatchingService.
- **Enumeration:** ReportStatus.

## Inheritance

No inheritance is used in this component. ReportStatus is an enumeration (a fixed list of values), not a parent class. FoundItemReport and LostItemReport share some attributes, but the requirements do not yet justify a shared parent class, so this is left for the team to decide in Exercise 3.

## Relationships

- FoundItemReport is a **composition** of ItemPhoto (has, 1 to 0..3): a photo cannot exist without its report.
- Finder submits FoundItemReport (1 to 0..*).
- MatchingService creates MatchSuggestion (1 to 0..*).
- MatchSuggestion points to one FoundItemReport and is for one LostItemReport (0..* to 1 each).
- Notification announces one MatchSuggestion (0..1 to 1) and is sent to one ItemOwner (0..* to 1).
- FoundItemReport has a **dependency** on ReportStatus (uses it as the type of its status attribute).

## Classes of Other Members Used for Reference Only

| Class | Owner |
| --- | --- |
| Finder | Member 1 |
| ItemOwner | Member 1 |
| LostItemReport | Member 1 |

## Open Design Questions (for Exercise 3)

1. Member 1 also links ItemPhoto to LostItemReport by composition. One class cannot be a composition part of two different wholes, so the team must choose: separate photo classes, or a shared parent class for the two reports.
2. CampusSecurityOfficer has no direct association with FoundItemReport here, because UC-08 and UC-10 belong to the Finder. The officer changes the status through the claim process (P6, Member 3).
3. UC-09 names the Finder as the primary actor, but FR-04 sends the email to the owner of the lost report. The Notification follows FR-04; the Finder sees matches through the found report.
