# Scheduling Consistency Check

Checked by Hamda Bahakeem (g00100146). We compared WBS.md, the Gantt chart (Gantt_Chart/Gantt_Chart.pdf), the network schedule (Network_Schedule/Network_Schedule.md and Network_Diagram.png) and Critical_Path.md side by side.

| Check | Result |
| --- | --- |
| WBS activities represent actual project work | Yes. The 10 tasks are specific to the Lost and Found system (database, UI, authentication, reporting, search and matching, notifications, claim verification, integration and testing, deployment) rather than a generic Analysis → Design → Coding → Testing list. |
| Gantt Chart activities correspond to WBS activities | Yes. The Gantt chart has exactly the same 10 tasks (T1–T10), with the same names, durations and predecessors. No tasks were added or removed. |
| Task dependencies are logically defined | Yes. Design work (T2, T3) follows requirements (T1). Authentication needs the database (T2). Reporting needs both the database and the UI (T2, T3). Matching follows reporting (T5), notifications follow matching (T6), and claims need authentication and reports (T4, T5). Testing waits for all modules (T7, T8), and deployment comes last (T9). T2/T3 and T6/T8 run in parallel. |
| Time estimates are realistic and project-specific | Yes. Durations are 2–4 working days per task for a team of three students, giving 21 working days (about 4 weeks) in total. |
| Expected Times are correctly calculated | Yes. Each Expected Time E = (O + 4M + P) / 6 rounds to the duration used in the WBS, Gantt chart and network diagram (see the Time Estimates table in WBS.md). |
| Network diagram reflects the WBS dependencies | Yes. Network_Diagram.png has one node per WBS task (T1–T10), and its arrows match the WBS predecessors exactly: T1→T2, T1→T3, T2→T4, T2→T5, T3→T5, T5→T6, T6→T7, T4→T8, T5→T8, T7→T9, T8→T9 and T9→T10. Critical tasks have thick borders. |
| ES, EF, LS, LF and Slack calculations are correct | Yes. The forward and backward passes in Network_Schedule.md give the same ES, EF, LS, LF and slack for every task as our independent check in Critical_Path.md. T4 has 4 days of slack, T8 has 2, and all other tasks have 0. The values in the diagram nodes match the tables. |
| Critical Path is correctly identified | Yes. There are two critical paths of equal length: T1 → T2 → T5 → T6 → T7 → T9 → T10 and T1 → T3 → T5 → T6 → T7 → T9 → T10, with a duration of 21 days. |
| Critical Path and duration verified with a tool | Yes. The PM Calculators online CPM calculator shows the same two critical paths and 21 days (Gantt_Chart/CPM_Verification.png). The Gantt chart also ends on Mon 30 Nov 2026, which is 21 working days from Mon 2 Nov 2026. |

## Changes made to earlier labs

These are the changes listed in section 6 of Project/Data_Flow_Modeling/DFD_Consistency_Check.md. Full details are in Documents/Lab3_Changes.md.

| File | Change Made | Reason |
| --- | --- | --- |
| Project/Requirements/Use_Cases.md | UC-01 Log In now lists Item Owner, Finder and Campus Security Officer as actors | The final use case diagram and both DFDs connect all three to Log In / P1 |
| Project/Requirements/Use_Cases.md | UC-09 Generate Match Suggestions now lists Email / Notification Service as a secondary actor | P5 sends the Match Notification Request to this entity |
| Project/Requirements/Functional_Requirements.md | Added FR-16 for resolving disputed claims | UC-14 had no matching functional requirement |
| Project/Scope.md | No change needed | Account Creation and Login is already listed as in scope |
