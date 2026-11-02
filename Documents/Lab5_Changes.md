# Earlier Lab Changes for Lab 6

Changes made to the Lab 5 class diagram so that it supports the three sequence diagrams.

| File | Change Made | Caused By |
| --- | --- | --- |
| Project/Object_Modeling/Class_Diagram/Consolidated_Class_Diagram | ItemSearch: Add + setFilters(keywords: String, category: String, location: String, dateFrom: Date, dateTo: Date): void | UC-03 message 1: the owner enters the filters (FR-03). |
| Project/Object_Modeling/Class_Diagram/Consolidated_Class_Diagram | ItemSearch: Add - sortNewestFirst(results: List\<FoundItemReport\>): List\<FoundItemReport\> | UC-03 message 7: results are shown newest first (FR-03). |
| Project/Object_Modeling/Class_Diagram/Consolidated_Class_Diagram | ItemReport: Add + matches(keywords: String, category: String, location: String, dateFrom: Date, dateTo: Date): boolean | UC-03 message 5: the search asks each report whether it fits the filters. It is on ItemReport because category, description, location and eventDate are there. |
| Project/Object_Modeling/Class_Diagram/Consolidated_Class_Diagram | LostItemReport: Add + getContactEmail(): String | UC-09 message 9: the notification copies the owner's contact email (Lab 5 decision). |
| Project/Object_Modeling/Class_Diagram/Consolidated_Class_Diagram | CampusMember: Remove + startSearch(): ItemSearch and the dashed CampusMember ⇢ ItemSearch dependency | In UC-03 the owner works with the ItemSearch control object directly. The lecture slides say entity objects should not access control objects, and CampusMember is an entity. |

UC-14 needed no change: its notes file lists no operation missing from the Lab 5 class diagram. All other classes and relationships are unchanged.
