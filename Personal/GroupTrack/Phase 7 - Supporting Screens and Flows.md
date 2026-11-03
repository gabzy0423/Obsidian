# GroupTrack Reviewer — Phase 7: Supporting Screens and Flows

> [!summary] Tandaan sa presentation
> Ang **Manage Members** ang screen para sa project membership at reviewer role. Ang **Notifications** ay local records para sa account sa parehong device; hiwalay ang Android notification display setting. Ang **Settings** ay entry point para sa profile, password, notifications, Privacy Policy, Terms & Conditions, at logout.

## 1. Saan pumapasok ang Phase 7?

Ang core project, task, review, at progress rules ay nasa Phases 1–6. Dito nakalagay ang supporting screens na maaaring itanong habang nagde-demo. Pareho pa rin ang local-only limit: ang accounts at project data ay nasa SQLite ng isang device installation, walang cloud sync sa current MVP.

## 2. Manage Members screen

Mula sa **Project Settings → Manage Members**, binubuksan ang Members screen para sa napiling project. Ang list ay nagpapakita ng member name/avatar, role, join date, bilang ng assigned tasks, at bilang ng completed tasks. May loading, empty, at error state na may **Retry**. Nire-reload ang list pagkatapos ng successful role o membership change.

| Action | Sino at kailan | Resulta |
|---|---|---|
| Appoint reviewer | Leader, sa connected ordinary member ng active project | Nagiging `REVIEWER` ang target. Kung may existing reviewer, may replacement confirmation; ibinabalik muna ang dati sa `MEMBER`. Isang reviewer lang ang active sa project. |
| Remove reviewer role | Leader, sa current reviewer ng active project | Babalik ang role sa `MEMBER`; mananatili siyang project member. |
| Remove member | Leader, sa member/reviewer ng active project | May confirmation bago alisin ang membership. Hindi puwedeng alisin ang leader. |

Nakatago ang management actions sa hindi leader, sa leader's own card, at kapag archived ang project. May repository checks din sa role at target project; hindi lang button visibility ang batayan ng authorization. Kapag inalis ang isang member, mawawala ang membership access niya sa project. **Huwag ipagpalagay na automatic na nade-delete o nare-reassign ang dating tasks niya**; hiwalay na behavior iyon at hindi bahagi ng reviewer claim na ito.

**Presentation line:** “Ang project leader ang namamahala sa members. Ang reviewer ay espesyal na role ng isang member para sa task review; puwede itong italaga, palitan, o ibalik sa ordinary member. Hindi puwedeng alisin ang leader sa sarili niyang project.”

## 3. Notifications: paggawa, listahan, at navigation

Gumagawa ang app ng local notification records para sa mga pangyayaring gaya ng **task assignment, submit/review decision, join request/decision, reviewer role change, member removal, progress milestone, at due-soon/overdue task**. Ang record ay para sa target local account. Kapag pinayagan ng device at naka-enable sa Settings, maaari ring magpakita ng Android system notification.

Ang Notifications screen ay mabubuksan mula sa notification button/drawer ng main screens. May list at empty state, pull-to-refresh, at **Mark all as read**. Kapag tinap ang item, mina-mark itong read at dinadala ang user sa kaugnay na task/project, Members, o Progress screen ayon sa notification type. Ang join rejection at member removal ay papunta sa Dashboard. Kung wala na ang target project, may feedback at babalik sa Dashboard.

Kapag tinap ang **Android system notification**, chine-check muna kung may signed-in account at kung ito ang target account. Kung signed out, dadalhin sa Sign In; kung ibang account ang active, hindi ididiretso sa target data. Ang project/task screens ang may sariling access checks bago magpakita ng detalye.

**Mahalagang pagkakaiba:** Ang Settings toggle ay para sa **Android system notification display**. Nasa SQLite pa rin ang in-app notification records kahit naka-off ang system display; puwede pa ring buksan ang Notifications screen.

### Deadline reminders

Ang periodic local check ay tumitingin sa **unfinished tasks sa hindi archived na projects** na may linked local account ang assignee. Kung lampas na ang deadline, overdue notification; kung nasa susunod na 24 oras, due-soon notification. May duplicate check para hindi paulit-ulit ang kaparehong reminder. Inexact Android alarm ang schedule, kaya **hindi garantisadong eksaktong oras** darating ang notification; nakadepende rin ang system display sa device permission at settings.

## 4. Settings, profile, at password

Sa Settings makikita ang current profile preview at mga entry para sa **Profile**, **Change Password**, **Notifications**, **Privacy Policy**, **Terms & Conditions**, at **Logout**. Ang detalye ng registration, profile editing, password validation, session, at logout ay nasa Phase 2. Sa Settings, ang Change Password ay may validation at confirmation bago i-save; ang Logout ay may confirmation at processing state bago bumalik sa Sign In.

Kapag in-enable ang Android notifications sa Android 13 pataas, humihingi ang app ng `POST_NOTIFICATIONS` permission kung kailangan. Kapag tinanggihan ang permission, mananatiling off ang switch. Ang switch ay sumusunod din sa system-level notification availability. Ang reminder check ay local sa device; hindi ito cloud push service.

## 5. Privacy Policy at Terms & Conditions

Accessible ang dalawang text dialog mula sa **Create Account** at **Settings**. Ang Privacy Policy ay nagpapaliwanag na account, profile, project, membership, at task data ay local sa SQLite; walang cloud sync sa current MVP; ang approved local members ay maaaring makakita ng project data; at maaaring mawala ang data kapag na-clear ang app data o na-uninstall ang app. Ang Terms ay tungkol sa angkop na academic/team use, pagbabahagi ng pinapayagang impormasyon, pag-iingat ng credentials, at pananagutan sa accuracy ng data.

**Presentation line:** “Mababasa ang privacy at terms bago gumawa ng account at muli sa Settings. Tahasan nitong sinasabi na local-only ang current MVP at maaaring mawala ang records kapag nabura ang app data.”

## 6. Sample demo scenario

1. Buksan ng leader ang isang active project, saka **Project Settings → Manage Members**.
2. Ipakita ang role, join date, assigned at completed task counts. Magtalaga ng ordinary member bilang reviewer; kung may reviewer na, ipakita ang replacement confirmation.
3. Gumawa o mag-submit ng task sa kabilang local account. Buksan ang Notifications screen ng target account, saka i-tap ang notification para makita ang kaugnay na screen.
4. Sa Settings, ipakita ang system notification toggle at buksan ang Privacy Policy at Terms & Conditions.
5. Ipaliwanag na maaaring magpalit ng local account sa parehong device para ma-demo ang sender at recipient sides; walang cross-device sync.

## 7. Posibleng panel questions

**Q: Puwede bang maging reviewer ang leader?**  
A: Ang leader ay hiwalay na role. Ang reviewer appointment ay para sa connected ordinary member; ang leader ang nagtatalaga sa kanya.

**Q: Ilang reviewer ang puwede sa project?**  
A: Isang active reviewer sa current implementation. Kapag pumili ng bago, ibinabalik ang dati sa member role sa iisang transaction.

**Q: Ano ang kaibahan ng remove reviewer at remove member?**  
A: Sa una, role lang ang nawawala at member pa rin siya. Sa pangalawa, inaalis ang project membership pagkatapos ng confirmation.

**Q: Kapag off ang Notifications sa Settings, wala na bang notification sa app?**  
A: May in-app records pa rin. Ang off switch ay pumipigil sa Android system display para sa account na iyon.

**Q: Real-time cloud push ba ang reminders?**  
A: Hindi. Local notification at periodic Android alarm ang gamit; hindi eksaktong oras o cross-device delivery ang pangako.

**Q: Saan makikita ang Privacy Policy at Terms?**  
A: Sa Create Account screen at sa Settings, bilang nababasang dialogs.

## 8. One-minute practice script

“Bukod sa core project at task flows, may supporting screens ang GroupTrack. Sa Manage Members, nakikita ang roles at task counts, at ang leader lang ang puwedeng magtalaga o magpalit ng reviewer at mag-alis ng member. May local notifications para sa task, membership, review, at deadline events; kapag tinap, binubuksan ang kaugnay na screen. Hiwalay ang in-app records sa Android notification toggle sa Settings. Nasa Settings din ang profile, password, logout, Privacy Policy, at Terms & Conditions. Local-only ang data at notifications sa current MVP, kaya sa iisang device namin ipinapakita ang multi-account flow.”

## Sources at scope

Sinuri noong **November 3, 2026**: `MembersActivity`, `ProjectRepository`, `ProjectSettingsBottomSheet`, `NotificationsActivity`, `NotificationRepository`, `NotificationService`, `NotificationRouter`, `NotificationRoutingActivity`, `DeadlineNotificationReceiver`, `AndroidNotificationHelper`, `SettingsActivity`, `CreateAccountActivity`, at `strings.xml`. Presentation reviewer ito batay sa current source; hindi live device QA. Ang Edit Project flow ay idinagdag sa Phase 3.
