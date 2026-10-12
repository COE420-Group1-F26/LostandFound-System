# Critical Path

**Critical Path:**

T1 → T2 → T5 → T6 → T7 → T9 → T10

T1 → T3 → T5 → T6 → T7 → T9 → T10

(T2 Design Database and T3 Design User Interface both take 3 days and both lead into T5, so the project has two critical paths of equal length.)

**Expected Project Duration:**

21 working days (Mon 2 Nov 2026 to Mon 30 Nov 2026)

**Explanation:**

We found the critical path from the network calculations in Exercise 6. In the forward pass, each task starts at the latest EF of its predecessors, which gives a project finish (largest EF) of 21 days at T10. In the backward pass, we started from 21 at T10 and worked back, with LF being the smallest LS of a task's successors.

| ID | E | ES | EF | LS | LF | Slack |
| --- | --- | --- | --- | --- | --- | --- |
| T1 | 2 | 0 | 2 | 0 | 2 | 0 |
| T2 | 3 | 2 | 5 | 2 | 5 | 0 |
| T3 | 3 | 2 | 5 | 2 | 5 | 0 |
| T4 | 2 | 5 | 7 | 9 | 11 | 4 |
| T5 | 4 | 5 | 9 | 5 | 9 | 0 |
| T6 | 4 | 9 | 13 | 9 | 13 | 0 |
| T7 | 2 | 13 | 15 | 13 | 15 | 0 |
| T8 | 4 | 9 | 13 | 11 | 15 | 2 |
| T9 | 4 | 15 | 19 | 15 | 19 | 0 |
| T10 | 2 | 19 | 21 | 19 | 21 | 0 |

The tasks with Slack = 0 (ES = LS) are T1, T2, T3, T5, T6, T7, T9 and T10, so they make up the critical path. Their durations add up to the full project length: 2 + 3 + 4 + 4 + 2 + 4 + 2 = 21 days. Any delay to one of these tasks pushes back the finish of the whole project, because nothing they lead into has any spare time.

The other paths are shorter. The path through T4 (Implement Authentication) and T8 (Implement Claim Verification) reaches T9 two days before it is needed, so T8 has 2 days of slack and T4 has 4. These tasks can slip by that much without delaying the project.

## Tool-based verification

We checked the critical path and duration with the PM Calculators online Critical Path Method calculator (https://www.pmcalculators.com/critical-path-method-calculator/). We entered the 10 WBS tasks with the same durations and predecessors. The tool names activities with letters, so A = T1, B = T2, C = T3, D = T4, E = T5, F = T6, G = T7, H = T8, I = T9 and J = T10.

| Item | Result |
| --- | --- |
| Tool used | PM Calculators – Critical Path Method Calculator (online) |
| Critical path shown by the tool | 2 critical paths: A → B → E → F → G → I → J and A → C → E → F → G → I → J, which is T1 → T2 → T5 → T6 → T7 → T9 → T10 and T1 → T3 → T5 → T6 → T7 → T9 → T10 |
| Project duration shown by the tool | 21 days |
| Matches the manual calculation? | Yes. Same two critical paths and the same 21-day duration |
| Screenshot | Gantt_Chart/CPM_Verification.png |
