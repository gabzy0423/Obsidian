# GroupTrack Reviewer — Phase 5: Task Workflow at Review

> [!summary] Pinakaimportanteng tandaan
> Ang task ay hindi basta minamarkahang tapos ng assignee. Sa normal flow, **assignee ang nagsisimula at nagsu-submit**, at **ibang eligible leader/reviewer ang nag-aapprove o humihingi ng pagbabago**. May no-self-review rule at isang malinaw na exception para sa solo leader.

## 1. Apat na task status

| Database status | Sa madaling salita | Sino ang susunod na kikilos? |
|---|---|---|
| `NOT_STARTED` | May task na, hindi pa sinisimulan | Assignee |
| `WORKING` | Ginagawa o nire-rework | Assignee |
| `FOR_REVIEW` | Na-submit para suriin | Eligible leader/reviewer |
| `DONE` | Na-approve at completed | Wala, maliban kung i-reopen ng leader |

**Normal path:** `NOT_STARTED → WORKING → FOR_REVIEW → DONE`  
**Kapag may revisions:** `FOR_REVIEW → WORKING → FOR_REVIEW`  
**Reopen:** `DONE → WORKING`, leader action na may confirmation.

Hindi puwedeng lumaktaw ang ordinary assignee mula `NOT_STARTED` diretso sa `DONE`. Ang repository ang nagva-validate ng exact transition, project membership, assignee identity, at archive state.

## 2. Assignee actions

Ang **nakatalagang assignee lang** ang maaaring mag-Start (`NOT_STARTED → WORKING`) at mag-Submit for Review (`WORKING → FOR_REVIEW`). Makikita ang actions sa task card/Task Detail kung eligible. Kapag successful, ina-update ang task status at nagdaragdag ng task-history record sa iisang database transaction. May local notification kapag na-submit for review.

Kapag **archived** ang project, visible ang task pero hindi maaaring mag-status action. Kapag hindi valid ang transition o hindi ang current user ang assignee, tinatanggihan ito sa repository kahit piliting tawagin ang operation.

## 3. Sino ang puwedeng mag-review?

Ang task ay dapat `FOR_REVIEW`, nasa parehong project ang actor at assignee, at **hindi sila iisang project-member identity**.

| Assignee role | Eligible reviewer |
|---|---|
| `MEMBER` | Ibang `LEADER` o `REVIEWER` |
| `LEADER` | Ibang `REVIEWER` |
| `REVIEWER` | Ibang `LEADER` |

Ang ordinary member ay hindi makakapag-review nang walang reviewer role. Ang leader ay **hindi** puwedeng mag-approve ng sariling task sa normal review flow. Ang approval ay `FOR_REVIEW → DONE`; ang Request Changes ay `FOR_REVIEW → WORKING`.

> [!important] Solo-leader exception
> Kung **iisa lang ang member** ng project at siya ang leader at assignee, maaari siyang mag-complete mula `WORKING → DONE` nang direkta. Ito ay separate direct-completion path, hindi self-approval ng `FOR_REVIEW` task. Kapag may iba nang member, normal review rules ang susundin.

## 4. Review comments at task history

Sa **Request Changes**, kailangang may hindi blankong reason/comment; chine-check ito sa UI at repository. Naka-save ang reason sa task-history record kasama ang status change, kaya makikita sa Task Detail timeline ang dahilan ng rework. Sa **Approve**, may confirmation dialog; optional ang comment sa repository review operation.

Kasama sa history ang creation, status transitions, reviewer decisions, actor, at oras. Magkasama ang status update at history insertion sa transaction: hindi dapat magkaroon ng status na nagbago ngunit walang katugmang history entry kung pumalya ang save.

Pagkatapos mag-approve, may local notification sa assignee. May local notification din kapag nag-request ng changes, at naka-link ito sa task context. Ang `DONE` task ay maaaring i-reopen ng project leader sa `WORKING`, na may history at notification. Hindi ibig sabihin ng reopen na burado ang naunang review records.

## 5. Proof-of-completion attachments

Sa Task Detail, ang **active assignee** ay maaaring magdagdag ng proof bago pa ma-review: local file/screenshot o HTTP(S) link. Puwede ito habang `NOT_STARTED` o `WORKING`; hindi na puwede kapag `FOR_REVIEW`, `DONE`, o archived ang project.

- Sa **link**, chine-check ng repository na `http` o `https` at may valid host.
- Sa **file**, kinokopya ang napiling file sa private app storage na `task_attachments`; ang metadata ay naka-save sa SQLite.
- Nakikita at nabubuksan ng authorized project viewers/reviewers ang saved proof sa Task Detail bago magdesisyon.
- Local ang files. Hindi sila nai-upload o nagsi-sync sa ibang phone.

Proof ay **optional** sa current flow. Ang submit for review ay status action; hindi required ang attachment para ma-submit.

## 6. Halimbawang end-to-end scenario

1. Gumawa ang leader ng task para kay Ben. Status: `NOT_STARTED`.
2. Pinindot ni Ben ang **Start**. Status: `WORKING`.
3. Nagdagdag si Ben ng screenshot o link bilang proof, tapos **Submit for Review**. Status: `FOR_REVIEW`.
4. Binuksan ng eligible leader/reviewer ang Task Detail at tiningnan ang proof at history.
5. Kung may kulang, **Request Changes** na may reason. Bumalik sa `WORKING`; nakikita ni Ben ang comment sa history.
6. Inayos at ni-resubmit ni Ben. Sa **Approve**, naging `DONE` ang task.

Ang task ng leader ay kailangang i-review ng appointed reviewer, maliban sa solo-leader direct-completion exception. Ang reviewer task naman ay leader ang magre-review.

## 7. Posibleng panel questions

**Q: Puwede bang i-mark DONE ng ordinary member ang sarili niyang task?**  
A: Hindi sa normal flow. Isusumite niya ito sa `FOR_REVIEW`, at ibang eligible leader/reviewer ang mag-aapprove.

**Q: Bakit may reviewer role kung leader na ang nagre-review?**  
A: Para may hiwalay na taong makapag-review ng leader-assigned task at para maipamahagi ang review ng member tasks. Hindi puwede ang self-review.

**Q: Ano ang mangyayari kapag hindi pasado ang task?**  
A: Required ang reason sa Request Changes; babalik sa `WORKING`, at nasa task history ang feedback para sa assignee.

**Q: Puwede bang mag-submit nang walang proof?**  
A: Oo. Optional ang file o HTTP(S) link; hindi iyon mandatory field ng status transition.

**Q: Puwede bang mag-approve sa archived project?**  
A: Hindi. Read-only ang task workflow hanggang i-restore ang project.

**Q: Puwede bang bawiin ang DONE?**  
A: Maaaring i-reopen ng leader pabalik sa `WORKING`; nakatala ang transition sa history.

## 8. One-minute practice script

“May apat na status ang task: Not Started, Working, For Review, at Done. Ang assignee ang nagsisimula at nagsu-submit. Pag nasa For Review na, ibang eligible leader o reviewer ang mag-aapprove o hihingi ng changes. Required ang reason kapag may rework, at naka-save ito sa task history. Optional din ang local screenshot/file o web link bilang proof na puwedeng tingnan bago mag-review. Hindi puwedeng i-approve ng tao ang sariling task; sa solo-leader project lang may direct completion mula Working papuntang Done. Kapag archived ang project, nababasa pa rin ang tasks pero walang status o review actions.”

## Sources at scope

Sinuri noong **November 3, 2026**: `ProjectTaskRepository`, `TaskReviewPolicy`, `TaskDetailBottomSheet`, `Task`, `TaskHistory`, `TaskAttachment`, at `docs/04-feature-specifications.md`. Ang Phase 6 ang progress/contribution calculations. Reviewer ito batay sa current source; hindi live device QA.
