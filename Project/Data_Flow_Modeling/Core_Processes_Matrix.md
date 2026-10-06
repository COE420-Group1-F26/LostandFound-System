# Core Processes Matrix

The following matrix describes the six major processes identified in the Level-0 DFD of the Lost and Found Property Claim System.

| Process No. and Name | Input Data | Output Data | Rules |
| --- | --- | --- | --- |
| P1 – Authenticate Users | Login Credentials, Account Record | Login Result, New Account Record | Users must provide valid AUS credentials. The system verifies account information before granting access. |
| P2 – Manage Lost Item Reports | Lost Item Report | Lost Report Confirmation, Lost Report Record | The item owner must provide the required lost-item details. A valid report is stored in the Lost Item Reports data store. |
| P3 – Manage Found Item Reports | Found Item Report, Report Update Request, Status Request, Stored Found Report | Found Report Confirmation, Item Status, Found Item Record | The finder must provide the required found-item details. Existing reports may be updated and their current status may be viewed. |
| P4 – Search Found Items | Search Request, Found Item Listings | Search Results | The system searches stored found-item listings according to the owner's search request and returns matching results. |
| P5 – Match Items and Notify Owners | Delivery Status, Match View Request, Found Item Data, Lost Report Data, Stored Matches | Match Details, Match Notification Request, Match Suggestions, Match Suggestion Record | Lost and found item data are compared to identify possible matches. Match suggestions are stored and relevant owners are notified through the notification service. |
| P6 – Process and Decide Claims | Claim Decision, Claim Query, Claim Request, Item Details, Stored Claims | Claim Details, Claim History Report, Claim Status, Claim Record, Item Status Update | Claims are reviewed by the Campus Security Officer. Decisions and dispute outcomes are recorded, and the found item's status is updated when required. |