# Architectural Design Goals

The goals below come from the non-functional requirements (Lab 3, Non_Functional_Requirements.md) and the main features of the project. They are ranked by how much the project depends on them.

| Goal ID | Design Goal | Description | Related Requirement | Priority | Justification |
| --- | --- | --- | --- | --- | --- |
| AG1 | Maintainability | The matching rules, the search and the claim rules can each be changed in one place without breaking the other parts. | NFR-10, FR-09 | High | NFR-10 asks for the matching logic to be a separate part. The scoring rules will need tuning after launch, and a change there must not break reporting or claims. |
| AG2 | Security | Only authenticated users reach their own functions, only campus security accounts reach claim verification, and private identifying details are never shown publicly. | NFR-11, NFR-12, NFR-13, NFR-15, FR-11, FR-12 | High | Releasing an item to the wrong person is the worst failure of the system, and claims contain private details. |
| AG3 | Performance | Search results and match suggestions appear quickly, even with many records. | NFR-01, NFR-06, FR-03, FR-09 | High | Search must answer within 2 seconds for 5,000 found items and matching within 3 seconds for 10,000 reports. Slow search makes users go back to social media groups. |
| AG4 | Reliability | The system stays available and an interrupted submission never leaves a partial or duplicate record. A failure of the email service does not stop reports or claims. | NFR-04, NFR-05, NFR-07, FR-04 | Medium | Owners depend on the system to learn that an item was found, and email is an outside service that can fail. |
| AG5 | Scalability | Many users can submit reports at the same time without slowing down the system. | NFR-08 | Medium | At least 500 people may submit found-item reports at the same time (for example at the start of a semester). |
