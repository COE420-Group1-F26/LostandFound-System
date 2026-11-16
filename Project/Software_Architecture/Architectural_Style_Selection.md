# Architectural Style Selection

## 1. Selected style

**Layered architecture** with three layers: Presentation, Application and Data. The Application layer is divided into five components, each with its own provided and required interfaces.

## 2. Main characteristics

- The system is split into horizontal layers, and a layer only uses the layer below it.
- The Presentation layer (web interface in the browser) shows the screens and sends the user's requests to the Application layer.
- The Application layer holds the business rules, split into components with clear responsibilities.
- The Data layer stores the records (D1 to D5 from the DFD) and is reached through one interface.
- Components talk to each other only through interfaces, not by reaching into each other's classes.

## 3. Why it is appropriate for the project

- The project is a web application with screens, business rules (matching, claim verification) and stored records. These are three different kinds of work, which is exactly what the three layers separate.
- The three groups of users (owners, finders and campus security officers) use different screens but the same business rules, so one shared Application layer serves all of them.
- Our Lab 6 sequence diagrams already follow this shape: in UC-03 and UC-09 the actor talks to a control object (ItemSearch, MatchingService), which uses the entity objects and, where needed, the outside Email Service; in UC-14 the officer works through the claim objects (DisputeCase, Claim, VerificationRecord), which update the found item through Report Management.
- The dependencies between our components go in one direction only (for example Claim Verification uses Report Management, but Report Management uses no other component). This is what a layered style requires.

## 4. How it supports the design goals

| Goal | How the style supports it |
| --- | --- |
| AG1 Maintainability | Each component can be changed behind its interface. The matching rules live in the Matching and Notification component, separate from Report Management (NFR-10). |
| AG2 Security | Every request enters through the Application layer, so login and the officer check (isOfficer in Account Management) are applied in one place. Private details stay inside Claim Verification and Report Management and are not returned to the public screens. |
| AG3 Performance | Search and matching are separate components, so each can be tuned or given its own resources. The Data layer is reached through one interface, so queries and indexes can be improved in one place. |
| AG4 Reliability | The outside Email Service is reached through one interface (ISendEmail), so an email failure affects only notifications. Each submission is saved through one operation of the Data layer, which makes it easy to save a record completely or not at all (NFR-05). |
| AG5 Scalability | The Application layer holds no screens, so more copies of it can run behind the web interface when many users submit reports. |

## 5. Advantages

- Clear separation of screens, rules and data.
- Components can be developed and tested one by one by different team members.
- Easy to replace one part (for example the email provider) without touching the others.

## 6. Potential limitations

- A request passes through several layers, which adds a small delay. This is acceptable for the 2 and 3 second targets (NFR-01, NFR-06).
- The Data layer is a shared point of failure. NFR-04 (99% availability) needs a reliable database.
- NFR-10 asks for the matching logic to be separate from both the reporting and the notification features. It is separate from reporting (Report Management is its own component), but Matching and Notification share one component, because a Notification is created from a MatchSuggestion. Inside that component they are still separate classes (MatchingService holds the scoring rules, Notification builds and sends the email through ISendEmail), so a change to the scoring rules does not touch the notification code. If the notification rules grow, Notification can be moved into its own component; the architecture would then have six components, which is still within the limit.
- Claim Verification is the largest component (6 classes). If it grows, it can be split later.
