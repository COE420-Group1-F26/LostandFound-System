# UC-03 Search Found Items — Sequence Diagram

**Drawn by:** Mahra Alhammadi (g00098713)

**Use case:** UC-03 Search Found Items

**Primary actor:** Item Owner

**Requirements:** FR-03

**Diagram:** UC-03_Search_Found_Items.png (editable: UC-03_Search_Found_Items.drawio)

## Participants

| Lifeline | Class | Object type |
| --- | --- | --- |
| Item Owner | Actor (stick figure) | The person; outside the system |
| search: ItemSearch | ItemSearch | Control object (2nd column) |
| found: FoundItemReport | FoundItemReport (inherits ItemReport) | Entity object; one lifeline stands for every found report |

## Messages

| No. | From → To | Message | Class operation |
| --- | --- | --- | --- |
| 1 | Item Owner → search | setFilters(keywords, category, location, dateFrom, dateTo) | ItemSearch.setFilters(...): void (added in Lab 6) |
| 2 | Item Owner → search | execute() | ItemSearch.execute(): List\<FoundItemReport\> |
| 3 | search → found | *[for each found item report] getStatus() | FoundItemReport.getStatus(): ReportStatus |
| 4 | found → search (dashed) | return status | return message |
| 5 | search → found | [status != WITHDRAWN] matches(keywords, category, location, dateFrom, dateTo) | ItemReport.matches(...): boolean (added in Lab 6, inherited) |
| 6 | found → search (dashed) | return isMatch | return message |
| 7 | search → search (self-call) | sortNewestFirst(results) | ItemSearch.sortNewestFirst(...) (private, added in Lab 6) |
| 8 | search → Item Owner (dashed) | return results | return List\<FoundItemReport\> |

**Iteration and guards:** iteration on message 3; guard on message 5. Messages 3 to 6 are inside a loop frame (for each found item report).

**Notes in the diagram:** [isMatch = true] the report is added to results; results are sorted newest first (FR-03).

## Operations this diagram needs that were not in the Lab 5 class diagram

| Class | Operation | Reason |
| --- | --- | --- |
| ItemSearch | + setFilters(keywords: String, category: String, location: String, dateFrom: Date, dateTo: Date): void | Message 1: the owner enters the search filters (FR-03). |
| ItemSearch | - sortNewestFirst(results: List\<FoundItemReport\>): List\<FoundItemReport\> | Message 7: results are shown newest first (FR-03). |
| ItemReport | + matches(keywords: String, category: String, location: String, dateFrom: Date, dateTo: Date): boolean | Message 5: each found report says whether it fits the filters (inherited by FoundItemReport). |
| CampusMember | Remove + startSearch(): ItemSearch and the CampusMember ⇢ ItemSearch dependency | The owner works with the ItemSearch control object directly; entity objects should not access control objects (lecture slides). |
