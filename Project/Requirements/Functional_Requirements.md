# Functional Requirements

## Member 1 Contributions — Mahra Alhammadi (g00098713)

|FR ID|Functional Requirement|Source Scenario/Stakeholder|Contributor|
|-|-|-|-|
|FR-01|The system shall allow a user to create an account and log in using an AUS email address and password, and shall reject any email address outside the aus.edu domain.|S-01, S-02|Mahra Alhammadi (g00098713)|
|FR-02|The system shall allow a logged-in user to submit a lost item report with a category, description, location lost, date lost, an optional photo, and optional private identifying details that are not shown to other users. It shall not accept the report until the category, description, location and date are filled in.|S-01|Mahra Alhammadi (g00098713)|
|FR-03|The system shall allow a logged-in user to search found items by keyword, filter the results by category, location found and date range, and display the results newest first.|S-02|Mahra Alhammadi (g00098713)|
|FR-04|The system shall notify the owner of a lost item report by email when a possible match is identified, including the found item's category, location found, date found, and a link to view its details.|S-01|Mahra Alhammadi (g00098713)|
|FR-05|The system shall allow a logged-in user to submit a claim on a found item by entering their AUS ID and at least one identifying detail not shown in the public listing, and shall set the claim status to "Pending Verification".|S-02|Mahra Alhammadi (g00098713)|

## Member 2 Contributions - Hamda Raed (g00100146) 
| NFR ID | Category | Non-Functional Requirement | Contributor |
| --- | --- | --- | --- |
| NFR-06 | Performance | The system shall show matching suggestions within 3 seconds, even with up to 10,000 reports in the database. | Hamda Raed (g00100146) |
| NFR-07 | Reliability | The system shall send match notification emails within 5 minutes of a match being found, and at least 98 out of 100 notifications shall actually go through. | Hamda Raed (g00100146) |
| NFR-08 | Scalability | The system shall handle at least 500 people submitting found-item reports at the same time without slowing down by more than double. | Hamda Raed (g00100146) |
| NFR-09 | Size | Each photo attached to a found-item report shall be no larger than 5MB, and a report can have up to 3 photos. | Hamda Raed (g00100146) |
| NFR-10 | Maintainability | The matching logic shall be built as its own separate part of the system, so it can be updated later without breaking the reporting or notification features. | Hamda Raed (g00100146) |

| FR ID | Functional Requirement | Source Scenario/Stakeholder | Contributor |
| --- | --- | --- | --- |
| FR-06 | The system shall allow a Finder to report a found item by entering category, description, color, location found, and date/time found. | S-03 | Hamda Raed (g00100146) |
| FR-07 | The system shall allow a Finder to attach up to 3 photos to a found-item report. | S-03 | Hamda Raed (g00100146) |
| FR-08 | The system shall allow a Finder to edit or withdraw a found-item report they submitted, provided it has not already been claimed. | S-03 | Hamda Raed (g00100146) |
| FR-09 | The system shall automatically generate a ranked list of candidate matches between found-item reports and lost-item reports based on category, description keywords, and location/date proximity. | S-04 | Hamda Raed (g00100146) |
| FR-10 | The system shall allow a Finder to view the current status (Pending, Matched, Claimed, Returned) of an item they handed in. | S-04 | Hamda Raed (g00100146) |


## Member 3 Contributions — Hamda Albahri (g00100284)

| FR ID | Functional Requirement | Source Scenario/Stakeholder | Contributor |
| --- | --- | --- | --- |
| FR-11 | The system shall allow an authenticated campus security officer to view all pending claims, including the associated item report, claimant's AUS ID, and private identifying details submitted with the claim. | S-05, S-06 / Campus Security Officer | Hamda Albahri (g00100284) |
| FR-12 | The system shall allow an authenticated campus security officer to approve a pending claim after reviewing the claimant's AUS ID and identifying details, and shall update the claim status to "Approved". | S-05 / Campus Security Officer | Hamda Albahri (g00100284) |
| FR-13 | The system shall allow an authenticated campus security officer to reject a pending claim and shall update the claim status to "Rejected". | S-06 / Campus Security Officer | Hamda Albahri (g00100284) |
| FR-14 | The system shall prevent an item from being marked as ready for release unless a campus security officer has approved the associated claim. | S-05, S-06 / Campus Security Officer | Hamda Albahri (g00100284) |
| FR-15 | The system shall maintain a claim history that records each approval or rejection, the campus security officer who performed the action, and the date and time of the action. | S-05, S-06 / Campus Security Officer | Hamda Albahri (g00100284) |

## Team Consolidated

*To be added after the oral.*

