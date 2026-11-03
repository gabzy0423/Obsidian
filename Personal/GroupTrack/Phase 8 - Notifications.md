# GroupTrack Reviewer — Phase 8: Notifications

> [!summary] Pinakaimportanteng tandaan
> Ang notification ay **local record para sa isang account sa device**. Hiwalay ang record sa Android system notification: kahit naka-off ang system display, maaari pa ring makita ang in-app Notifications list. Walang cloud push o cross-device delivery sa current MVP.

## 1. Anong events ang may notification?

Gumagawa ang app ng local notifications para sa mga pangyayaring gaya ng **task assignment, submit for review, approval o request changes, task reopen, join request at approval/rejection, reviewer role change, member removal, progress milestone, at due-soon/overdue task**. Ang record ay may target user at maaaring may project/task context. Ang mga task at project event rules ay nasa Phases 3–6.

Kapag nag-dispatch ang `NotificationService`, ini-insert muna ang notification sa SQLite, pagkatapos ay sinusubukang ipakita ito bilang Android system notification kung pinapayagan ng device at Settings. Kaya hindi nakadepende ang in-app record sa pagiging visible ng system notification.

## 2. Notifications screen

Mabubuksan ito sa notification button o drawer ng main screens. May list na naka-group ayon sa petsa, empty state, loading state, at pull-to-refresh. May **Mark all as read** para sa account. Kapag tinap ang isang item, mina-mark itong read at binubuksan ang kaugnay na screen.

| Notification context | Karaniwang destination |
|---|---|
| Task assignment, review, due soon, overdue | Project Task List; bubuksan ang task kung may task ID. |
| Join request received | Project Task List at Join Requests. |
| Join request approved | Project Task List. |
| Member joined / reviewer role change | Members screen ng project. |
| Progress milestone / project completed | Project Progress detail. |
| Join request rejected / member removed | Dashboard. |

Kung wala na ang target project, may feedback at babalik sa Dashboard. Ang target screen ay may sariling access checks bago magpakita ng project data.

## 3. Pag-tap ng Android system notification

Bago mag-route, chine-check ng `NotificationRoutingActivity` kung signed in ang user at kung siya ang target account. Kung signed out, bubuksan ang Sign In. Kung ibang local account ang active, may feedback at dadalhin sa Dashboard sa halip na direktang buksan ang target. Kung valid, mina-mark read ang notification at dadaan sa parehong navigation router.

**Presentation line:** “May in-app notification record na naka-link sa local account. Kapag tinap, chine-check muna ang account at dinadala sa kaugnay na project o task screen.”

## 4. Due-soon at overdue reminders

May periodic local Android alarm na sumusuri sa **unfinished tasks sa hindi archived na projects** na may linked local account ang assignee. Kapag lampas na ang deadline, overdue notification; kapag nasa susunod na 24 oras, due-soon notification. May duplicate check para hindi paulit-ulit ang kaparehong reminder.

Inexact alarm ang schedule, kaya **hindi garantisado ang eksaktong oras** ng check o display. Maaari ring hindi lumabas ang Android notification kung naka-off ang system display o hindi nabigyan ng device permission. Local-only ang reminders; hindi sila ipinapadala sa ibang phone.

## 5. Sample demo scenario

1. Sa parehong device, mag-sign in bilang leader at mag-assign ng task sa isa pang local account na project member.
2. Mag-sign in bilang assignee at buksan ang Notifications screen. Ipakita ang task-assignment item.
3. I-tap ang item para magpunta sa Project Task List at task detail.
4. Bumalik at ipakita ang **Mark all as read**. Sa Settings, ipaliwanag na ang notification switch ay para sa Android system display; ang in-app list ay hiwalay.

## 6. Posibleng panel questions

**Q: Real-time cloud push ba ito?**  
A: Hindi. Local SQLite record at Android system notification sa parehong device installation ang gamit.

**Q: Kapag off ang Notifications sa Settings, mawawala ba ang in-app list?**  
A: Hindi. System display lang ang naka-off; ang local notification records ay maaari pa ring basahin sa app.

**Q: Puwede bang buksan ng ibang signed-in account ang notification ng kapwa user?**  
A: Kapag galing sa Android notification tap, tine-check ang target account at hindi ididiretso ang maling account. Ang destination screen ay may sarili ring access checks.

**Q: Bakit hindi eksaktong 24 hours before deadline ang alert?**  
A: Periodic inexact local alarm ang gamit, kaya sa susunod na scheduled check ito matutukoy; hindi eksaktong delivery time ang pangako.

## 7. One-minute practice script

“Ang GroupTrack ay gumagawa ng local notifications para sa task, join request, role, progress, at deadline events. Sa Notifications screen, makikita ang list, puwedeng mag-refresh at mark all as read, at puwedeng i-tap ang item para buksan ang kaugnay na screen. Hiwalay ang saved in-app record sa Android system notification; ang Settings toggle ay para sa system display. Ang due-soon at overdue alerts ay galing sa periodic local check ng unfinished tasks. Walang cloud push o cross-device delivery sa current MVP.”

## Sources at scope

Sinuri noong **November 3, 2026**: `NotificationService`, `NotificationRepository`, `NotificationsActivity`, `NotificationAdapter`, `NotificationRouter`, `NotificationRoutingActivity`, `DeadlineNotificationReceiver`, `AndroidNotificationHelper`, at `Notification`. Presentation reviewer ito batay sa current source; hindi live device QA. Ang Settings toggle ay nasa Phase 9.
