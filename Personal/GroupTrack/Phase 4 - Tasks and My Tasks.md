# GroupTrack Reviewer — Phase 4: Task Creation, Assignment, at My Tasks

> [!summary] Pinakaimportanteng tandaan
> Ang **task** ay isang gawain sa loob ng **isang project** na naka-assign sa **isang project member**. **Leader** ang gumagawa at nag-e-edit ng task details. Ang **Project Task List** ay lahat ng tasks ng napiling project; ang **My Tasks** ay tasks na naka-assign sa kasalukuyang user mula sa lahat ng accessible projects niya.

## 1. Saan nagsisimula ang task flow?

**Dashboard project card → Project Task List → plus button → Create Task bottom sheet**

Ipinapasa ang project ID sa Project Task List. Naglo-load ang screen ng title, subject, task counts/progress, at mga task card para sa project na iyon. May access check sa repository: creator o project member lamang ang makakabasa. Ang plus button ay para sa leader at hindi ipinapakita kapag archived ang project.

> [!note] Dalawang listahan, magkaibang tanong
> **Project Task List:** “Ano ang lahat ng gawain ng team sa project na ito?”  
> **My Tasks:** “Ano ang mga gawaing naka-assign sa akin sa lahat ng projects ko?”

## 2. Create Task: input at validation

Sa modal, ilalagay ang **title**, **assignee**, **deadline**, at optional **description**. Required ang title, assignee, at deadline. Dapat **today o future date** ang deadline sa local date validation. Sa date picker, ang napiling araw ay sine-save hanggang 11:59:59 PM, kaya hindi agad lalabas na overdue sa umaga ng due date.

Ang assignee list ay galing sa members ng **napiling project**, at leader lang ang puwedeng mag-load nito para sa assignment. Kung leader lang ang member, maaari siyang ma-preselect. Kung pumalya ang member load, may error at Retry; hindi dapat mag-save na walang eligible assignee.

Sa repository, inuulit ang importanteng checks kahit may UI validation:

- Signed-in user ay creator at may `LEADER` membership sa project.
- Project ay umiiral at **hindi archived**.
- Assignee member ay umiiral at kabilang sa **parehong project**.
- Non-empty ang title at valid ang deadline.

Kung valid, gumagawa ng task na may initial status **`NOT_STARTED`** at creation entry sa task history. Magkasama ang task at history insert sa transaction. May local task-assignment notification kung linked local account ang assignee.

**Presentation line:** “Hindi puwedeng mag-assign ng member mula sa ibang project. Chine-check ito ng repository bago i-save ang task.”

## 3. Edit at delete

Maaaring baguhin ng leader ang task title, description, assignee, at deadline sa active project. Sa edit, kailangan pa rin ng valid title, eligible assignee sa parehong project, at deadline na today o future date. May task deletion action para sa authorized active-project leader. Ang member ay hindi nag-e-edit ng task metadata.

Kapag **archived** ang project, visible pa rin ang tasks pero hindi puwedeng mag-create, edit, o delete. Ang repository ang nagre-reject ng restricted mutation, kaya hindi lang ito UI hiding.

## 4. Project Task List

Bawat card ay nagpapakita ng task title, optional description, status, assignee, at deadline. Kapag lagpas na ang deadline at **hindi `DONE`**, may overdue badge at naka-highlight ang deadline. Ang pag-tap ng card ay nagbubukas ng Task Detail bottom sheet.

May **All, Not Started, Working, For Review, Done** status filters. Ang search ay tumitingin sa title, description, at assignee name sa kasalukuyang project. May loading, empty, error, at Retry states. Ang counts at progress sa summary ay galing sa actual task rows. Ang status actions at leader review buttons ay detalyado sa Phase 5.

## 5. My Tasks: cross-project view

Ang bottom navigation **Tasks** ay kasalukuyang nagbubukas ng **My Tasks** screen. Query nito ang tasks na ang assignee member record ay linked sa signed-in user, kahit magkaibang projects. Ipinapakita sa card ang **project name**, task details/status/deadline, at archived marker kung applicable. Nakaayos ang result ayon sa deadline, earliest first.

May search sa **task title, description, o project name**. May **All, Not Started, Working, For Review, Done, Overdue** filters at count badges. Ang Overdue ay deadline na lumipas na at status ay hindi `DONE`. Ang pag-tap ng task ay nagbubukas ng Task Detail; hindi bagong duplicate task ang ginagawa ng My Tasks screen.

Nagre-reload ang list kapag bumalik sa screen at pagkatapos ng task-detail change. May pull-to-refresh. Kapag pumalya ang refresh habang may na-load nang tasks, nananatili ang huling list at may brief error feedback; kapag unang load ang pumalya, may error state at Retry. Nasa My Tasks pa rin ang assigned tasks mula sa archived projects, pero read-only ang actions habang archived.

## 6. Business rules na madalas itanong

| Tanong | Sagot |
|---|---|
| Puwede bang gumawa ng task ang ordinary member? | Hindi; leader ang may create/assign permission sa project. |
| Puwede bang i-assign sa hindi member? | Hindi; dapat member ng parehong project. |
| May task ba na walang assignee? | Hindi sa current create flow; required ang assignee. |
| Ano ang status ng bagong task? | `NOT_STARTED`. |
| Ano ang kahulugan ng overdue? | Lumipas ang saved deadline at hindi pa `DONE` ang task. |
| Pareho ba ang Project Task List at My Tasks? | Hindi; project-wide vs current-user-assigned across projects. |
| Puwede bang baguhin ang task sa archived project? | Hindi; nababasa pa rin pero read-only hanggang restore. |

## 7. Sample demo scenario

1. Gumawa si Ana ng Project A; approved member si Ben.
2. Sa Project Task List ng Project A, gumawa si Ana ng “Write introduction,” in-assign kay Ben, at nagtakda ng deadline.
3. `NOT_STARTED` ang bagong task; makikita ito ng team sa Project Task List.
4. Pag nag-sign in si Ben, makikita niya ang task sa My Tasks kasama ang pangalan ng Project A. Kung may assignment din siya sa Project B, nasa parehong My Tasks list iyon.
5. Pag in-archive ni Ana ang Project A, visible pa rin ang task ngunit wala munang mutation actions.

## 8. Posibleng panel questions

**Q: Saan gumagawa ng task?**  
A: Sa loob ng isang napiling project, gamit ang Create Task bottom sheet sa Project Task List.

**Q: Paano napipigilan ang wrong-project assignment?**  
A: Sa repository, chine-check na ang napiling assignee ay member ng parehong project ID bago mag-save.

**Q: Paano nakikita ng isang user ang lahat ng assignments niya?**  
A: Sa My Tasks tab, na kumukuha ng assigned tasks niya across projects at ipinapakita ang project name sa bawat card.

**Q: Kapag walang internet, makikita pa rin ba ang tasks?**  
A: Oo, local SQLite ang source ng core task data. Hindi nito sini-sync ang tasks sa ibang device.

**Q: Auto-completed ba kapag lumipas ang deadline?**  
A: Hindi. Overdue indicator lang iyon; hiwalay ang status workflow at review.

## 9. One-minute practice script

“Ang task ay ginagawa sa loob ng isang project. Leader ang maglalagay ng title, deadline, optional description, at assignee mula sa project members. Bago i-save, chine-check muli ng repository ang role, project state, fields, at same-project assignment. `NOT_STARTED` ang initial status. Sa Project Task List, makikita ang lahat ng tasks at puwedeng mag-search o mag-filter ayon sa status. Sa My Tasks, pinagsasama ang assignments ng signed-in user mula sa lahat ng projects niya. Ang overdue ay visual warning para sa unfinished task na lumampas sa deadline; hindi nito awtomatikong binabago ang status.”

## Sources at scope

Sinuri noong **November 3, 2026**: `ProjectTaskRepository`, `TaskDao`, `CreateTaskBottomSheet`, `ProjectTaskListActivity`, `TasksActivity`, at `docs/04-feature-specifications.md`. Ang exact status transitions, review comments, proof attachments, at self-review policy ay Phase 5. Hindi ito live device QA.
