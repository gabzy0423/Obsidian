# GroupTrack Reviewer — Phase 3: Projects at Membership

> [!summary] Tandaan sa presentation
> Ang **project** ang lalagyan ng team, members, at tasks. Ang creator ay awtomatikong **leader at connected member**. Ang ibang local account ay magre-request gamit ang project code; **leader approval** pa ang kailangan bago maging member at makapasok sa project.

## 1. Project lifecycle sa isang tingin

**Create project → Share code → Join request → Leader approves/rejects → Member access → Manage project → Archive/restore**

Lahat ng hakbang ay sa **parehong device installation**. Walang cloud invitation o cross-device synchronization sa current MVP.

## 2. Create project

Sa Dashboard, pinipili ng signed-in user ang **Create Project** at pinupunan ang project title, subject, deadline, at optional description. Required ang title at subject; ang deadline ay **today o future date**. Sa successful save:

1. Gumagawa ang repository ng unique internal project ID at display/share code.
2. Sine-save ang `Project` row sa Room/SQLite.
3. Sine-save ang creator bilang `LEADER` na `CONNECTED` sa `ProjectMember`.
4. Magkasama ang dalawang insert sa database transaction: kung pumalya ang isa, walang half-created project.

Ang default member limit sa current entity ay **10** (kasama ang leader); puwedeng baguhin ng leader sa Project Settings, pero hindi maaaring mas mababa sa kasalukuyang member count.

**Presentation line:** “Kapag gumawa ng project ang user, siya ang sole leader at unang member. Atomic ang pag-save ng project at leader membership para walang project na walang owner.”

### Edit Project

Sa project card o Project Task List menu, maaaring buksan ng **project creator/leader** ang Edit Project bottom sheet. Maaaring baguhin ang **title, subject, optional description, at deadline**. Required pa rin ang title, subject, at deadline; ang date picker ay may minimum na today. Pag successful ang save, nire-reload ang project display. Hindi nagbabago ang internal project ID, share code, o membership dahil project details lang ang ina-update.

May access check sa database update: dapat ang `created_by` ng project ay ang signed-in user. Kung walang matching project/creator o pumalya ang update, error ang ibinabalik sa UI at hindi ito ituturing na successful edit.

## 3. Project ID vs project code

| Term | Gamit |
|---|---|
| Internal project ID | Unique UUID na gamit sa database relations at screen navigation. |
| Project code | Mas maikling code na puwedeng ibahagi sa ibang local account para mag-request sumali. |

Ang generated code ay `GT-` plus **6 characters** mula sa uppercase letters at digits, gaya ng `GT-ABC234` (sample format lang). Nino-normalize ng join flow ang code input; ang repository ay tumatanggap din ng internal project ID. Ang code ay **lookup key, hindi access token**: hindi ito sapat para mabasa ang project data kung hindi approved member.

## 4. Join request flow

**Member side:** Sa Join Project modal, ilalagay ang code. Hinahanap ng repository ang project; kung wala, “not found” ang result. Kung member na o may pending request na, hindi gumagawa ng duplicate. Kung valid, magse-save ng `PENDING` request. Makikita rin ng user ang status sa **My Requests**.

**Leader side:** Makikita ng project leader ang pending requests. Sa **Approve**, iche-check muli ang pending status, leader authority, project state, member limit, at requester account. Saka lamang gagawa ng connected `MEMBER` row at gagawing `APPROVED` ang request sa iisang transaction. Sa **Reject**, magiging `REJECTED` ang request at **walang membership na idaragdag**. May local notification para sa handled request. Maaaring muling mag-request ang rejected user; ibinabalik ang request sa `PENDING`.

> [!important] Business rule
> Ang `PENDING`, `APPROVED`, at `REJECTED` ay **request statuses**. Ang `CONNECTED` ay **membership status**. Magkaiba ang request at membership records. Pending request ≠ project access.

## 5. Project access at roles

- Ang creator/connected project member ang may access sa project data. Hindi dapat makakuha ng task o member details ang non-member sa pamamagitan lang ng pagpalit ng project ID.
- **Leader** ang nagma-manage ng project details, join requests, member limit, reviewer appointment, at archive/restore.
- **Member** ang nakikilahok sa accessible project at assigned tasks. Ang project-specific reviewer role ay tinalakay sa Phase 2; task-review rules ay sa Phase 5.
- Ang repository ang nagche-check ng role sa restricted actions; hindi sapat ang pagtatago ng button sa UI.

## 6. Dashboard project list at filters

Ang Dashboard ay naglo-load ng projects na accessible sa current account at kinukuha ang task counts, progress, at member count mula sa local data. Puwedeng hanapin sa **title o subject**.

| Filter | Lalabas |
|---|---|
| All Projects | Lahat ng **hindi archived** na accessible projects. |
| Active | Hindi archived at may unfinished task **o wala pang tasks**. |
| Completed | Hindi archived, may tasks, at **lahat ay `DONE`**. |
| Archived | Mga archived projects ng user. |

Ang project card ay puwedeng buksan para makita ang project task list. Ang summary counts at progress ay galing sa task records; hindi ito manually ine-edit na aggregate.

## 7. Archive at restore

**Leader lang** ang maaaring mag-archive o mag-restore. Kapag archived, nananatili sa SQLite ang project, members, at tasks. Nasa Archived filter ang card at maaari pa rin itong buksan, pero **read-only ang task workflow**: bawal ang task create/edit/delete, assignee status action, at review action hanggang ma-restore. Ang restore ay nagbabalik sa normal project listing at task actions.

Ang archive state ay nasa **project row**, kaya pareho itong nakikita ng local accounts na may access sa project. Hindi ito delete operation.

> [!note] Docs check
> May lumang pahayag sa `docs/03-database-schema.md` na archive ay visibility-only. Ang approved behavior sa `docs/01-project-overview.md` / `docs/04-feature-specifications.md` at current task repository ay **read-only habang archived**. Iyon ang gamitin sa presentation.

## 8. Sample demo scenario

1. Si Ana ay nag-create ng “Final Group Project” at naging leader.
2. Kinopya ni Ana ang `GT-` project code.
3. Sa parehong device, nag-sign in si Ben at nagsumite ng join request.
4. Hindi pa makapasok si Ben habang `PENDING`.
5. Nag-sign in si Ana at in-approve si Ben; lumitaw siya bilang connected member.
6. In-archive ni Ana ang project: nasa Archived filter ito at nabubuksan pa, pero wala munang task mutations. Pag-restore, puwede uli ang normal actions.

## 9. Posibleng panel questions

**Q: Unique ba ang project code?**  
A: Gumagawa ang app ng code at chine-check kung ginagamit na sa local database. Hiwalay ito sa internal UUID.

**Q: Kapag alam ko ang code, member na ba ako?**  
A: Hindi. Request pa lang ang nagagawa nito; leader approval ang nagbibigay ng membership.

**Q: Ano ang mangyayari kung puno na ang project?**  
A: Tinatanggihan ang approval kapag naabot na ang member limit; hindi nadadagdagan ang membership.

**Q: Ano ang epekto ng rejection?**  
A: `REJECTED` ang request at walang project access. May local feedback/notification; puwedeng mag-request muli.

**Q: Delete ba ang archive?**  
A: Hindi. Nananatili ang project at related records, at leader ang puwedeng mag-restore.

**Q: Sino ang puwedeng mag-edit ng project details?**  
A: Ang creator/leader. Title, subject, description, at deadline ang nae-edit; hindi nito pinapalitan ang project ID, code, o members.

**Q: Puwede bang mag-edit ng tasks habang archived?**  
A: Hindi. Visible at openable ang project, pero read-only ang task workflow hanggang ma-restore.

**Q: Makakasali ba ang user sa ibang phone gamit ang code?**  
A: Hindi sa current MVP. Local lang ang project at request records sa device installation.

## 10. One-minute practice script

“Sa GroupTrack, ang signed-in user ay maaaring gumawa ng project na may title, subject, at deadline. Awtomatiko siyang leader at connected member. Ang leader ay maaari ring mag-edit ng project details. Gumagawa rin ang app ng shareable project code. Sa parehong device installation, ibang account ang maglalagay ng code para magsumite ng join request. Pending pa lang iyon; ang leader ang mag-aapprove o magre-reject. Sa approval, saka pa lang gagawa ng member record, subject sa member limit. Sa Dashboard, may search at Active, Completed, at Archived filters. Ang archive ay hindi deletion: makikita pa rin ang project at tasks, pero read-only ang task actions hanggang i-restore ng leader.”

## Sources at scope

Sinuri noong **November 3, 2026**: `ProjectRepository`, `DashboardRepository`, `ProjectDao`, `Project`, `ProjectJoinRequest`, `ProjectCodeGenerator`, `DashboardActivity`, `EditProjectBottomSheet`, `ProjectTaskListActivity`, `ProjectTaskRepository`, at project overview/feature specs. Presentation reviewer ito; hindi live device QA. Phase 4 ang task creation, assignment, at My Tasks.
