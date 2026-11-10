# Selected Workflows for Activity Modeling

The activity diagrams model the same three use cases that were selected and modeled with sequence diagrams in Lab 6 (Selected_Use_Cases.md). No new use cases were selected. Each activity diagram shows the workflow of its use case, while the sequence diagram shows the messages between the objects.

| Use Case ID | Use Case Name | Sequence Diagram (Lab 6) | Activity Diagram (Lab 7) | Swimlanes | Decisions | Parallel Activities | Drawn by |
| --- | --- | --- | --- | --- | --- | --- | --- |
| UC-03 | Search Found Items | Sequence_Diagrams/UC-03_Search_Found_Items | Activity_Diagrams/AD_UC03_Search_Found_Items | Item Owner, ItemSearch, FoundItemReport | withdrawn or not (12), matches or not (14), more reports (17), results found or not (19) | the four search filters can be entered in any order (fork 2 / join 7) | Mahra Alhammadi (Member 1) |
| UC-09 | Generate Match Suggestions | Sequence_Diagrams/UC-09_Generate_Match_Suggestions | Activity_Diagrams/AD_UC09_Generate_Match_Suggestions | Finder, MatchingService, Notification, Email / Notification Service | score reaches the minimum or not (7), more lost reports (10), suggestions found or not (11), more suggestions to notify (20) | marking the found item MATCHED happens at the same time as notifying the owners (fork 13 / join 21) | Hamda Bahakeem (Member 2) |
| UC-14 | Resolve Disputed Claim | Sequence_Diagrams/UC-14_Resolve_Disputed_Claim | Activity_Diagrams/AD_UC14_Resolve_Disputed_Claim | Campus Security Officer, DisputeCase, Claim, FoundItemReport | claim pending or already decided (6), more claims (9), two or more pending claims or not (10) | marking the found item CLAIMED happens at the same time as rejecting the other claims (fork 14 / join 17) | Hamda Albahri (Member 1) |
