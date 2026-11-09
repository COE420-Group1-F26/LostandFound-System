# UC-03 Search Found Items — Activity Diagram

**Drawn by:** Mahra Alhammadi (g00098713)

**Use case:** UC-03 Search Found Items (same use case as the Lab 6 sequence diagram UC-03_Search_Found_Items)

**Requirements:** FR-03

**Files:** AD_UC03_Search_Found_Items.pdf, AD_UC03_Search_Found_Items.png (editable: AD_UC03_Search_Found_Items.drawio)

**Swimlanes:** Item Owner, ItemSearch, FoundItemReport

**Decisions:** withdrawn or not (12), matches or not (14), more reports (17), results found or not (19).

**Parallel activities:** the four search filters can be entered in any order (fork 2 / join 7).

## Workflow

The owner enters the four search filters in any order and submits the search. ItemSearch saves the filters and goes through the found item reports one by one: withdrawn reports are skipped, every other report is checked against the filters, and matching reports are added to the results. When there are no more reports, the results are sorted newest first (FR-03). The owner then sees either the matching found items or a message that nothing matched.

## Nodes and control flows

| No. | Swimlane | Node | Label | Next (guard) | Matches (Lab 6 sequence / Lab 5 class) |
| --- | --- | --- | --- | --- | --- |
| 1 | Item Owner | Initial | — | → 2 | — |
| 2 | Item Owner | Fork | — | → 3, 4, 5, 6 | — |
| 3 | Item Owner | Action | Enter keyword | → 7 | FR-03 |
| 4 | Item Owner | Action | Select category | → 7 | FR-03 |
| 5 | Item Owner | Action | Select location found | → 7 | FR-03 |
| 6 | Item Owner | Action | Select date range | → 7 | FR-03 |
| 7 | Item Owner | Join | — | → 8 | — |
| 8 | Item Owner | Action | Submit search | → 9 | Seq 1–2 |
| 9 | ItemSearch | Action | Save search filters | → 10 | setFilters() · Seq 1 |
| 10 | ItemSearch | Merge | — | → 11 | — |
| 11 | ItemSearch | Action | Get next found item report | → 12 | execute() loop · Seq 3 |
| 12 | FoundItemReport | Decision | — | [status = WITHDRAWN] → 16; [status != WITHDRAWN] → 13 | getStatus() · Seq 3–4 |
| 13 | FoundItemReport | Action | Check report against filters | → 14 | matches() · Seq 5–6 |
| 14 | FoundItemReport | Decision | — | [matches] → 15; [does not match] → 16 | Seq 6 |
| 15 | ItemSearch | Action | Add report to results | → 16 | note in Seq |
| 16 | FoundItemReport | Merge | — | → 17 | — |
| 17 | ItemSearch | Decision | — | [more reports] → 10; [no more reports] → 18 | loop end |
| 18 | ItemSearch | Action | Sort results newest first | → 19 | sortNewestFirst() · Seq 7 |
| 19 | ItemSearch | Decision | — | [results found] → 20; [no results] → 21 | — |
| 20 | Item Owner | Action | View matching found items | → 22 | Seq 8 |
| 21 | Item Owner | Action | See "no matching items" message | → 22 | Seq 8 (empty list) |
| 22 | Item Owner | Merge | — | → 23 | — |
| 23 | Item Owner | Final | — | — | — |
