# Network Schedule

Task-on-node network built from `Project/Project_Scheduling/WBS.md`. Durations are working days. The diagram is in `Network_Diagram.png` (editable source: `Network_Diagram.drawio`).

## Task Table

| ID | Task | Predecessor(s) | Duration |
| --- | --- | --- | --- |
| T1 | Refine System Requirements | --- | 2 |
| T2 | Design Database | T1 | 3 |
| T3 | Design User Interface | T1 | 3 |
| T4 | Implement Authentication | T2 | 2 |
| T5 | Implement Lost and Found Reporting | T2, T3 | 4 |
| T6 | Implement Search and Matching | T5 | 4 |
| T7 | Implement Notifications | T6 | 2 |
| T8 | Implement Claim Verification | T4, T5 | 4 |
| T9 | Integrate and Test System | T7, T8 | 4 |
| T10 | Prepare Deployment | T9 | 2 |

## Forward Pass

ES = largest EF of the predecessors (0 if none). EF = ES + duration.

| ID | ES calculation | ES | EF |
| --- | --- | --- | --- |
| T1 | 0 | 0 | 0 + 2 = 2 |
| T2 | max(EF(T1)=2) | 2 | 2 + 3 = 5 |
| T3 | max(EF(T1)=2) | 2 | 2 + 3 = 5 |
| T4 | max(EF(T2)=5) | 5 | 5 + 2 = 7 |
| T5 | max(EF(T2)=5, EF(T3)=5) | 5 | 5 + 4 = 9 |
| T6 | max(EF(T5)=9) | 9 | 9 + 4 = 13 |
| T7 | max(EF(T6)=13) | 13 | 13 + 2 = 15 |
| T8 | max(EF(T4)=7, EF(T5)=9) | 9 | 9 + 4 = 13 |
| T9 | max(EF(T7)=15, EF(T8)=13) | 15 | 15 + 4 = 19 |
| T10 | max(EF(T9)=19) | 19 | 19 + 2 = 21 |

Project finish = largest EF = EF(T10) = **21 days**.

## Backward Pass

LF = smallest LS of the successors (project finish if none). LS = LF - duration.

| ID | LF calculation | LF | LS |
| --- | --- | --- | --- |
| T10 | project finish = 21 | 21 | 21 - 2 = 19 |
| T9 | min(LS(T10)=19) | 19 | 19 - 4 = 15 |
| T8 | min(LS(T9)=15) | 15 | 15 - 4 = 11 |
| T7 | min(LS(T9)=15) | 15 | 15 - 2 = 13 |
| T6 | min(LS(T7)=13) | 13 | 13 - 4 = 9 |
| T5 | min(LS(T6)=9, LS(T8)=11) | 9 | 9 - 4 = 5 |
| T4 | min(LS(T8)=11) | 11 | 11 - 2 = 9 |
| T3 | min(LS(T5)=5) | 5 | 5 - 3 = 2 |
| T2 | min(LS(T4)=9, LS(T5)=5) | 5 | 5 - 3 = 2 |
| T1 | min(LS(T2)=2, LS(T3)=2) | 2 | 2 - 2 = 0 |

## Slack

Slack = LS - ES.

| ID | ES | EF | LS | LF | Slack | Critical? |
| --- | --- | --- | --- | --- | --- | --- |
| T1 | 0 | 2 | 0 | 2 | 0 | Yes |
| T2 | 2 | 5 | 2 | 5 | 0 | Yes |
| T3 | 2 | 5 | 2 | 5 | 0 | Yes |
| T4 | 5 | 7 | 9 | 11 | 4 | No |
| T5 | 5 | 9 | 5 | 9 | 0 | Yes |
| T6 | 9 | 13 | 9 | 13 | 0 | Yes |
| T7 | 13 | 15 | 13 | 15 | 0 | Yes |
| T8 | 9 | 13 | 11 | 15 | 2 | No |
| T9 | 15 | 19 | 15 | 19 | 0 | Yes |
| T10 | 19 | 21 | 19 | 21 | 0 | Yes |

## Critical Path

Tasks with slack = 0: T1, T2, T3, T5, T6, T7, T9, T10.

- T1 → T2 → T5 → T6 → T7 → T9 → T10
- T1 → T3 → T5 → T6 → T7 → T9 → T10

T2 and T3 both take 3 days and both feed T5, so there are two critical paths of equal length. Each adds up to 21 days (2 + 3 + 4 + 4 + 2 + 4 + 2).

T4 has 4 days of slack and T8 has 2 days, because the path T2 → T4 → T8 → T9 reaches T9 earlier than the critical path does.

This result matches `Critical_Path.md` (same ES, EF, LS, LF and slack for every task, same two critical paths, same 21-day duration) and the online tool verification recorded there.

Prepared by Mahra Alhammadi (Member 1) for the Network Schedule exercise (Exercise 6).
