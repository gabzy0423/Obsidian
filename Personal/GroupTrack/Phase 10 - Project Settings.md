# GroupTrack Reviewer — Phase 10: Project Settings

> [!summary] Pinakaimportanteng tandaan
> Ang **Project Settings** ay leader-only bottom sheet para sa napiling project. Dito makikita at makokopya ang project code, makikita ang member count at reviewer status, mababago ang member limit, mabubuksan ang Manage Members, at maia-archive ang active project.

## 1. Saan ito binubuksan at sino ang may access?

Sa **Project Task List**, buksan ang three-dot project menu at piliin ang Project Settings. Ipinapasa ang project ID sa bottom sheet. Nilo-load ng repository ang project at chine-check na ang signed-in user ay parehong **`LEADER` member** at **creator** ng project. Kapag hindi leader o hindi makita ang project, error ang ipapakita. May loading state at **Retry** kung pumalya ang load.

Ang **Edit Project** (title, subject, description, deadline) ay hiwalay na bottom sheet at nakapaliwanag sa Phase 3. Ang Project Settings ay para sa code, membership controls, at archive.

## 2. Ano ang nakikita sa sheet?

| Bahagi | Gamit |
|---|---|
| Project title at code | Pagkilala sa napiling project; maaaring i-copy ang code sa clipboard para sa join request ng ibang local account. |
| Member count at member limit | Ilang connected members na ang nasa project at ilan ang kasalukuyang pinapayagan. Kasama ang leader sa count. |
| Reviewer status | Pangalan ng active reviewer, notice kung wala pa, o solo-project notice kung leader lang ang member. |
| Manage Members | Nagbubukas ng hiwalay na Members screen para sa reviewer role at membership actions (Phase 7). |
| Archive Project | Leader-only action na nagmamarka sa project bilang archived (Phase 3). |

Kung solo project, nakatago ang **Manage Members** button sa current UI. Kung may kasama pero wala pang reviewer, may notice para malinaw kung bakit kailangan ng reviewer sa leader-assigned task flow. Ang review eligibility mismo ay nasa Phase 5.

## 3. Change Member Limit

Default limit ng bagong project ay **10**, kasama ang leader. Sa Change Member Limit dialog, ang entered value ay dapat number na **hindi mas mababa sa current member count** at nasa **1–20** ayon sa UI validation. Pag-save, muling chine-check ng repository ang leader role, active project state, at current count bago i-update ang project row. Kung invalid o pumalya ang save, hindi magbabago ang limit at may error feedback.

Ang limit ay mahalaga sa **join request approval**: kung puno na ang project, hindi ma-aapprove ang bagong member. Hindi nito inaalis ang mga kasalukuyang member kapag binago ang value. Kapag archived ang project, disabled ang Change Member Limit button; tinatanggihan din ito ng repository.

**Presentation line:** “May capacity control ang leader. Hindi niya puwedeng itakda ang limit na mas mababa sa bilang ng existing members, at muling chine-check ang limit bago mag-approve ng join request.”

## 4. Archive mula sa Project Settings

Sa active project, may **Archive Project** action. May confirmation dialog, processing state habang nagse-save, at success dialog kapag natapos. Leader lang ang makakapag-archive. Hindi nito dine-delete ang project, tasks, o members; lalabas ito sa Archived filter at magiging read-only ang task workflow hanggang ma-restore. Ang **restore** ay nasa Dashboard archive flow, hindi sa Project Settings sheet.

Kung archived na ang project, disabled ang archive at member-limit buttons sa sheet. Maaari pa ring makita o kopyahin ang project code; ang member-management actions sa Members screen ay nakatago habang archived.

## 5. Sample demo scenario

1. Mag-sign in bilang project leader at buksan ang Project Task List.
2. Sa three-dot menu, buksan ang Project Settings. Ipakita ang project code, member count/limit, at reviewer status.
3. I-copy ang code at ipaliwanag na join request pa lang ang kaya nitong simulan; kailangan pa ng leader approval.
4. Buksan ang Change Member Limit dialog. Ipakita ang validation sa value na mas mababa sa current count; saka pumili ng valid value kung nais ituloy.
5. Ipakita ang Manage Members entry at archive confirmation. Ituloy ang archive sa huling bahagi ng demo kung kailangan pang gamitin ang active project.

## 6. Posibleng panel questions

**Q: Puwede bang buksan ng ordinary member ang Project Settings?**  
A: Hindi. Chine-check ng repository na creator at leader member ang signed-in user.

**Q: Access token ba ang project code?**  
A: Hindi. Lookup key ito para sa join request; walang project access hangga't hindi approved member (Phase 3).

**Q: Puwede bang gawing 2 ang limit kung 3 na ang members?**  
A: Hindi. Tinatanggihan ito ng UI at repository; hindi nito basta inaalis ang existing members.

**Q: Saang screen ina-appoint ang reviewer?**  
A: Sa Manage Members screen. Status at entry point lamang ang nasa Project Settings; role actions ay nasa Phase 7.

**Q: Bakit may archive dito at sa Dashboard?**  
A: Parehong project archive state ang binabago. May confirmation, processing, at success feedback ang Project Settings flow; ang restore ay nasa Dashboard.

## 7. One-minute practice script

“Ang Project Settings ay bottom sheet para lang sa project creator na leader member. Dito nakikita at nakokopya ang project code, member count at limit, at reviewer status. Maaaring baguhin ang member limit basta hindi bababa sa current count at nasa UI range na 1 hanggang 20. Mula rito, mabubuksan ang Manage Members o maia-archive ang project. Ang archive ay may confirmation at hindi deletion: nananatili ang data at read-only ang task actions hanggang ma-restore sa Dashboard. Hiwalay ang Edit Project details sa Phase 3.”

## Sources at scope

Sinuri noong **November 3, 2026**: `ProjectSettingsBottomSheet`, `ProjectRepository`, `ProjectSettingsData`, `ProjectDao`, `ProjectTaskListActivity`, at `docs/01-project-overview.md`. Presentation reviewer ito batay sa current source; hindi live device QA. Ang join flow ay Phase 3, task review ay Phase 5, at Manage Members ay Phase 7.
