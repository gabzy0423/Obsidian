# GroupTrack Reviewer — Phase 7: Manage Members

> [!summary] Pinakaimportanteng tandaan
> Ang **Manage Members** ay para sa membership at reviewer role ng isang project. Ang leader lang ang puwedeng magtalaga o magpalit ng reviewer at mag-alis ng member. Iba ang pag-alis ng reviewer role sa pag-alis ng project membership.

## 1. Saan ito sa app?

Sa napiling project, buksan ang **Project Settings → Manage Members**. Ipinapasa ang project ID para ang members ng tamang project ang ma-load. Ang screen ay nagpapakita ng name/avatar, role, join date, assigned-task count, at completed-task count ng bawat member. May loading, empty, at error state na may **Retry**. Nagre-reload ang list pagkatapos ng successful action at kapag bumalik sa screen.

Ang Members screen na ito ay para sa **project management**. Ang read-only Members tab sa Project Progress detail ay para sa **contribution statistics** (Phase 6).

## 2. Role at action rules

| Action | Sino ang puwede? | Epekto |
|---|---|---|
| Appoint reviewer | Leader; target ay ordinary connected member | Target ay magiging `REVIEWER`. |
| Replace reviewer | Leader; may existing reviewer | May confirmation. Babalik sa `MEMBER` ang dati at magiging `REVIEWER` ang bago sa iisang database transaction. Isang active reviewer lang sa project. |
| Remove reviewer role | Leader; target ay current reviewer | Babalik sa `MEMBER` ang target, pero mananatiling project member. |
| Remove member | Leader; target ay member o reviewer | May confirmation; aalisin ang membership. Hindi puwedeng alisin ang leader. |

Ang action buttons ay hindi ipinapakita sa hindi leader, sa leader's own card, at sa archived project. May repository checks din sa leader at target project/role; hindi button visibility lang ang authorization. Pagkatapos ng role change o member removal, may local notification para sa target account kung linked ito.

**Mahalagang limitasyon:** Walang automatic task reassignment bago alisin ang member. Ang `Task` ay may foreign key sa assigned/creator member na `NO_ACTION`, kaya maaaring pumalya ang removal kung may tasks na naka-reference pa sa member. Sa presentation, huwag ipangakong lahat ng member removal ay magtatagumpay; ipakita lamang ang case na nasubukan sa device.

## 3. Kaugnayan sa task review

Ang reviewer ay **project member na may review role**, hindi kapalit ng leader sa pag-manage ng project. Ang eksaktong puwedeng mag-review ng isang task, kasama ang no-self-review rule at solo-leader exception, ay nasa Phase 5. Kung magbabago ang reviewer role, maaaring magbago kung sino ang eligible sa susunod na task review.

**Presentation line:** “Leader ang nagma-manage ng project members. Isang reviewer ang active sa current implementation; puwede siyang italaga o palitan, at puwede ring ibalik sa ordinary member nang hindi inaalis sa project.”

## 4. Sample demo scenario

1. Mag-sign in bilang leader at buksan ang active project.
2. Buksan ang **Project Settings → Manage Members** at ipakita ang roles, join dates, at task counts.
3. Magtalaga ng ordinary member bilang reviewer. Kung may reviewer na, ipakita ang replacement confirmation.
4. Ipakita ang kaibahan ng **Remove Reviewer Role** at **Remove Member** confirmation. Hindi kailangang ituloy ang removal sa live demo kung kailangan pa ang account para sa susunod na steps.

## 5. Posibleng panel questions

**Q: Puwede bang mag-appoint ng reviewer ang ordinary member?**  
A: Hindi. Leader lang ang puwede; chine-check ito sa repository.

**Q: Ilang reviewer ang puwede sa isang project?**  
A: Isang active reviewer sa current implementation. Ang replacement ay nagbabalik muna sa dating reviewer sa `MEMBER` role sa iisang transaction.

**Q: Ano ang kaibahan ng remove reviewer at remove member?**  
A: Sa remove reviewer, role lang ang nawawala at member pa rin siya. Sa remove member, sinusubukang alisin ang membership; maaaring tumanggi ang database kapag may dependent tasks.

**Q: Puwede bang alisin ang leader?**  
A: Hindi; tinatanggihan ito sa repository at wala ring remove action sa leader card.

**Q: Puwede bang mag-manage ng members habang archived?**  
A: Nakatago ang management actions sa archived project sa Members screen. Ang archive/read-only flow ay nasa Phase 3.

## 6. One-minute practice script

“Sa Manage Members screen, nakikita ng team ang project members, roles, join dates, at assigned at completed task counts. Ang project leader lang ang puwedeng magtalaga o magpalit ng reviewer at sumubok mag-alis ng member. Isang reviewer ang active sa project. Kapag inalis ang reviewer role, member pa rin ang tao. Ang membership removal ay maaaring pumalya kung may dependent task records; wala pang automatic reassignment. Hiwalay ang screen na ito sa contribution view sa Progress. Sinusuri ng repository ang role bago gumawa ng restricted action.”

## Sources at scope

Sinuri noong **November 3, 2026**: `MembersActivity`, `ProjectRepository`, `ProjectSettingsBottomSheet`, `ProjectMember`, `Task`, `ProjectMemberDao`, at `TaskReviewPolicy`. Presentation reviewer ito batay sa current source; hindi live device QA. Ang project creation/join flow ay nasa Phase 3, at ang contribution calculations ay nasa Phase 6.
