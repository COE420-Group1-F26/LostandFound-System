# Lab 3 Changes for Lab 4

| File | Change Made | Caused By |
| --- | --- | --- |
| Project/Requirements/Use_Cases.md | UC-01 Log In: actors changed from Item Owner only to Item Owner, Finder and Campus Security Officer, and the description now covers all three roles. | Context Diagram / Level-0 DFD (P1 Authenticate Users), Lab 3 UML diagram |
| Project/Requirements/Use_Cases.md | UC-09 Generate Match Suggestions: added Email / Notification Service as a secondary actor. | Level-0 DFD (P5 sends Match Notification Request), Lab 3 UML diagram |
| Project/Requirements/Functional_Requirements.md | Added FR-16: the officer views competing claims for the same item, approves the verified one, rejects the others, and the resolution is recorded in the claim history. | UC-14 Resolve Disputed Claim, S-06, P6 Process and Decide Claims |

No change was needed to Project/Scope.md, because Account Creation and Login was already added as an in-scope feature in Lab 3.

After these changes, P6 Process and Decide Claims in the DFD consistency check (section 5) should also list FR-16.
