# Individual Class Inventory — Member 2

**Contributor:** Hamda Albahri (g00100284)

**Component modeled:** Found Item Reports, Matching and Notifications

**Level-0 Processes:** P3 Manage Found Item Reports, P5 Match Items and Notify Owners

## Classes

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Contributor |
| --- | --- | --- | --- | --- |
| C10 | FoundItemReport | Stores information about a found item, including its description, location, date, and current status. | UC-06, UC-07, UC-08, UC-10 | Hamda Albahri |
| C11 | ItemPhoto | Represents a photo attached to a found item report to help identify the item. | UC-06, UC-07 | Hamda Albahri |
| C12 | ReportStatus | Defines the allowed report statuses: Pending, Matched, Claimed, Returned, and Withdrawn. | UC-10 | Hamda Albahri |
| C13 | MatchSuggestion | Represents a possible match between a lost item report and a found item report, including a matching score. | UC-09 | Hamda Albahri |
| C14 | MatchingService | Compares lost and found item reports and generates possible match suggestions. | UC-09 | Hamda Albahri |
| C15 | Notification | Represents a notification sent to an item owner when a possible match is identified. | UC-04 | Hamda Albahri |

## Why Each Class Is Needed

| Class | Justification |
| --- | --- |
| FoundItemReport | A found item requires its own record containing multiple details and a status. |
| ItemPhoto | A report may contain photos that help users recognize found items. |
| ReportStatus | An enumeration ensures that reports use only valid status values. |
| MatchSuggestion | Each suggested match needs to store its associated reports and matching score. |
| MatchingService | Matching requires separate logic to compare reports and generate suggestions. |
| Notification | Notifications require their own message details and delivery information. |

## Classes of Other Members Used for Reference Only

| Class | Owner |
| --- | --- |
| User | Member 1 |
| ItemOwner | Member 1 |
| Finder | Member 1 |
| LostItemReport | Member 1 |
| Claim | Member 3 |