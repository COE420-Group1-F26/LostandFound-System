# Individual Class Inventory — Member 1

**Contributor:** Mahra Alhammadi (g00098713)

**Component modeled:** Users and authentication, lost item reports, and searching found items

**Level-0 processes:** P1 Authenticate Users, P2 Manage Lost Item Reports, P4 Search Found Items

**Data stores:** D1 User Accounts, D2 Lost Item Reports (D3 Found Item Reports is read by the search)

## Classes

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Contributor |
| --- | --- | --- | --- | --- |
| C01 | User (abstract, entity) | Holds the account data kept in D1 (AUS ID, name, email, password hash) and provides login, logout and password change for every person who uses the system. | UC-01 | Mahra Alhammadi |
| C02 | ItemOwner (entity) | The registered account of a person who lost an item. Submits a lost item report and searches the found items. | UC-01, UC-02, UC-03 | Mahra Alhammadi |
| C03 | Finder (entity) | The registered account of a person who found an item and hands it in through the system. | UC-01 | Mahra Alhammadi |
| C04 | CampusSecurityOfficer (entity) | The registered account of security staff who log in to manage found items and decide claims. | UC-01 | Mahra Alhammadi |
| C05 | LostItemReport (entity) | The record of one lost item: name, category, description, where and when it was lost, private identifying details and the owner's contact email. Stored in D2. | UC-02 | Mahra Alhammadi |
| C06 | ItemSearch (control) | The criteria an owner enters (keyword, category, date range, location) and the operation that looks through the found items, so it carries out the search task. | UC-03 | Mahra Alhammadi |

## Why each class is a class (not just an attribute)

| Class | Justification |
| --- | --- |
| User | The three actors share the same account data and the same login behaviour (UC-01), so it is written once in a parent class. |
| ItemOwner | Has its own behaviour (reporting lost items and searching, UC-02 and UC-03) that the other roles do not have. |
| Finder | A separate actor in the use case model with its own role (submits found reports); it needs its own class so the found-item side can associate with it. |
| CampusSecurityOfficer | A separate actor that decides claims and manages found items; those responsibilities are modeled with Member 2 and Member 3. |
| LostItemReport | A lost report has its own data and life cycle (submit, update, withdraw) and is stored in D2, so it is a class and not an attribute of ItemOwner. |
| ItemSearch | A search has several criteria and one operation that uses them together; modeling it as a class keeps the owner's class small. |

## Object types

- **Entity objects** (persistent information tracked by the system): User, ItemOwner, Finder, CampusSecurityOfficer, LostItemReport.
- **Control objects** (tasks performed by the system): ItemSearch.
- **Boundary objects** (interaction between user and system): none are modeled. The login, lost report and search screens belong to the user interface design, and the team decides in Exercise 3 whether any boundary class is needed.
- **Actors vs classes:** the actors (Item Owner, Finder, Campus Security Officer) are outside the system. ItemOwner, Finder and CampusSecurityOfficer here are the registered accounts the system stores in D1 User Accounts, so they are inside the system.

## Inheritance used

ItemOwner, Finder and CampusSecurityOfficer inherit from the abstract class User. This is a real is-a relationship: each of them is a user that logs in with the same email and password (UC-01), and an ItemOwner, Finder or CampusSecurityOfficer can be used wherever a User is expected (substitutability).

## Relationships

- LostItemReport is a **composition** of ItemPhoto (filled diamond): a photo of a lost item lives and dies with its report.
- ItemOwner is associated with LostItemReport (reports, 1 to 0..*) and with ItemSearch (runs, 1 to 0..*).
- ItemSearch is associated with Member 2's FoundItemReport (looks through, 0..* to 0..*).
- LostItemReport has a **dependency** on the enumeration ReportStatus, because it uses it as the type of its status attribute.

## Classes of other members used for reference only

| Class | Owner |
| --- | --- |
| FoundItemReport | Member 2 |
| ItemPhoto | Member 2 |
| ReportStatus (enumeration) | Member 2 |

## Open design questions (to settle in Exercise 3)

1. Does a lost item report need a status? It is shown as `status: ReportStatus`, reusing Member 2's enumeration. If lost reports use different states, the team will define a separate enumeration.
2. A lost item report has one photo (0..1), while Member 2's found reports may allow several, and ItemPhoto cannot be a composition of two different classes. The team may give each report its own photo class, or create a shared parent class for LostItemReport and FoundItemReport, which both hold category, description, location, date and photo.
3. The approve and reject claim operations on CampusSecurityOfficer may move to Member 3's claim classes during integration.
