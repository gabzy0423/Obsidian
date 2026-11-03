# GroupTrack Reviewer — Phase 6: Progress at Contributions

> [!summary] Pinakaimportanteng tandaan
> Ang Progress screen ay **nagbabasa at nagbibilang ng saved tasks** sa Room/SQLite. Hindi manual na ine-edit ang percentage. **Completed = `DONE`**, **pending = lahat ng hindi `DONE`**, at **progress = completed ÷ total tasks × 100**. Kapag walang task, **0%**.

## 1. Para saan ang Progress module?

Ipinapakita nito kung gaano na kalayo ang accessible projects at kung ilan ang natapos na tasks ng bawat member. Makakatulong ito sa leader at members na makita ang status ng team bago ang deadline. Basic tracking at data visualization ito, hindi predictive analytics o grading system.

**Presentation line:** “Ang progress at contribution ay derived mula sa task records, kaya sumasabay ang display sa actual task statuses kapag nag-reload ang screen.”

## 2. Data source at access

**ProgressActivity → ProgressRepository → Room DAOs → local SQLite** ang current flow. Kinukuha ang projects na **created by signed-in user** o may `CONNECTED` membership siya. Ang pending join request ay hindi pa access. Pagbukas ng specific Project Progress detail, muling chine-check ang creator/member access bago basahin ang tasks at members.

Kasama rin sa accessible results ang **archived projects**; may archived label sa detail. Nababasa ang progress kahit read-only ang task actions habang archived. Walang remote API o cross-device sync.

## 3. Progress page: filters, search, at overall summary

| Filter | Ipinapakita |
|---|---|
| **All Progress** | Lahat ng accessible projects ng signed-in user. |
| **My Projects** | Mga project na siya ang creator/leader. |
| **Team** | Mga project na kasali siya ngunit hindi siya ang leader. |

Ang search ay tumitingin sa **project title at subject**. Ang Overall Progress section ay kinukuwenta mula sa **kasalukuyang visible projects pagkatapos ng filter at search**: total projects, total tasks, completed tasks, at overall percent. Ang project cards ay may sariling percent at nagbubukas ng Project Progress detail kapag tinap.

**Mahalagang halimbawa:** May dalawang visible projects: A = 1/2 done, B = 8/8 done. Overall = **9/10 = 90%**, hindi average na `(50% + 100%) / 2 = 75%`. Task counts ang basehan, kaya mas maraming task ang may mas malaking ambag sa overall percent.

## 4. Exact calculations

| Metric | Formula / rule |
|---|---|
| Total tasks | Bilang ng lahat ng task rows sa project. |
| Completed tasks | Bilang ng tasks na status ay `DONE`. |
| Pending tasks | `Total tasks − completed tasks`; kasama ang `NOT_STARTED`, `WORKING`, `FOR_REVIEW`. |
| Project progress | `completed × 100 / total`; kung total = 0, 0%. Whole-number percentage, rounded down. |
| Overall progress | `sum(completed) × 100 / sum(total)` para sa **visible** projects; kung zero tasks, 0%. |
| In progress card | `WORKING` **plus** `FOR_REVIEW`. |
| Not started card | Bilang ng `NOT_STARTED`. |
| Member completed contribution | `DONE` tasks na naka-assign sa member na iyon. |
| Member contribution percent | `member DONE × 100 / lahat ng DONE sa project`; kung walang DONE, 0%. |

**Halimbawa ng member contribution:** Kung may **4 DONE** tasks sa Project A at 3 doon ay assigned kay Ana, si Ana ay **3 completed tasks at 75% contribution share**. Hindi ito score sa quality o effort; bilang lang ng completed assigned tasks.

## 5. Project Progress detail: tatlong tabs

**Overview** — Project name/subject, Active o Archived badge, circular progress, completed/total, KPI cards, at status breakdown. Ang breakdown ay **Not Started, Working, For Review, Done**. Nakabukod ang `FOR_REVIEW` sa breakdown kahit kasama ito sa **In Progress** KPI card. May zero-task empty state.

**Tasks** — Read-only list ng project tasks na may status badge, assignee, at deadline. Dito tinitingnan ang datos; task actions ay nasa task screens.

**Members** — Project participants, role, join date, assigned-task count, completed-task count, at contribution percentage. Nakukuha ang contribution mula sa `DONE` tasks na naka-assign sa bawat member. Hindi ini-eedit ang percentage dito.

## 6. Refresh, empty, at error states

Nagre-reload ang Progress list at Project Progress detail kapag bumalik sa screen (`onResume`). Kaya pagkatapos ng task creation o status change, ang next load ay magpapakita ng updated counts. May loading indicator habang binabasa ang database, empty state kung walang accessible projects, no-results state para sa filter/search na walang match, at error state na may **Retry** kapag pumalya ang query. Sa project detail, may empty states din para sa zero tasks o members.

> [!important] Huwag sabihin sa panel
> Huwag sabihing real-time cloud analytics o automatic sync sa maraming phone. Local query at screen reload ang current behavior.

## 7. Sample demo scenario

1. May project na may apat na tasks: `NOT_STARTED`, `WORKING`, `FOR_REVIEW`, at `DONE` — tig-isa.
2. Total = **4**, completed = **1**, pending = **3**, progress = **25%**.
3. Sa Overview, **In Progress = 2** (`WORKING` + `FOR_REVIEW`), **Not Started = 1**, **Completed = 1**. Ang status breakdown ay tig-iisa sa apat na kategorya.
4. Kapag in-approve ang `FOR_REVIEW` task at bumalik sa Progress, completed = **2**, pending = **2**, progress = **50%**.
5. Sa Members tab, tataas ang completed contribution ng assignee ng na-approve na task.

## 8. Posibleng panel questions

**Q: Paano kino-compute ang 0-task project?**  
A: Zero tasks at **0%**, para walang divide-by-zero at hindi mapagkamalang completed.

**Q: Kasama ba ang For Review sa completed?**  
A: Hindi. Pending pa ito hanggang ma-approve at maging `DONE`; kasama ito sa In Progress card.

**Q: Average ba ng project percentages ang overall?**  
A: Hindi. Pinagsasama muna ang completed at total task counts ng visible projects, saka kinukuwenta ang percentage.

**Q: Ano ang basis ng contribution ng member?**  
A: Bilang ng `DONE` tasks na naka-assign sa kanya. Ang percent ay share niya sa lahat ng `DONE` tasks ng project.

**Q: Kasama ba ang archived project?**  
A: Oo kung accessible sa user. May Archived label sa detail; nababasa ang progress kahit read-only ang task workflow.

**Q: Kailan nagbabago ang displayed progress?**  
A: Kapag na-save ang task/status change at nag-reload ang Progress screen, gaya ng pagbalik dito.

## 9. One-minute practice script

“Ang Progress module ay nagko-compute mula sa actual task records sa local SQLite. Ang completed ay `DONE` tasks; ang pending ay lahat ng ibang statuses. Ang percent ay completed divided by total tasks, at 0% kapag walang tasks. May All Progress, My Projects, at Team filters, at nagbabago ang overall summary ayon sa visible projects. Kapag binuksan ang project, may Overview para sa KPIs at status breakdown, Tasks para sa read-only list, at Members para sa assigned at completed counts. Ang member contribution percent ay share niya sa lahat ng completed tasks ng project. Nagre-refresh ang data sa pagbalik sa screen; wala itong cloud sync.”

## Sources at scope

Sinuri noong **November 3, 2026**: `ProgressRepository`, `ProgressActivity`, `ProjectProgressActivity`, `ProjectDao`, `TaskDao`, `docs/progress-page-flow.md`, at `docs/04-feature-specifications.md`. Reviewer ito batay sa current source; hindi live device QA.
