# GroupTrack Reviewer — Phase 11: Database, Migrations, at Demo Data

> [!summary] Pinakaimportanteng tandaan
> **Room over local SQLite** ang data layer ng current GroupTrack app. Ang database ay `grouptrack.db`, kasalukuyang **version 10**, at may migration chain mula version 1 hanggang 10. May automatic demo seeder sa current code; mahalagang ipaliwanag ito nang hiwalay sa normal user-created data.

## 1. Paano dumadaloy ang data?

**Android Activity / bottom sheet → repository → Room DAO → SQLite** ang karaniwang flow. Ang Activity ang kumukuha ng input at nagpapakita ng result; ang repository ang nagva-validate ng business rules at tumatawag sa DAO; ang DAO ang nagbabasa o nagsusulat sa local tables. Ang current signed-in user ID ay nasa private `SharedPreferences` sa `SessionManager`, hindi sa cloud session.

Sa current production path, walang Firebase/Firestore o cross-device sync. Ang dalawang local account ay makakakita ng iisang project kung nasa **parehong app installation** at may naaangkop na membership. Ang app data sa magkahiwalay na phone ay hindi awtomatikong magtutugma.

## 2. Walong Room entities at relasyon

| Entity/table | Para saan? | Pangunahing ugnayan |
|---|---|---|
| `User` / `users` | Local account, normalized email, password hash/salt, profile | Ang creator ng project at linked member account. |
| `Project` / `projects` | Title, subject, deadline, code, archive state, member limit | May creator user; may members, join requests, tasks. |
| `ProjectJoinRequest` / `project_join_requests` | `PENDING`, `APPROVED`, `REJECTED` request | Nakaugnay sa project at requester user; hiwalay sa actual membership. |
| `ProjectMember` / `project_members` | Role at membership sa isang project | Nag-uugnay ng project at local user; assignee/creator reference ng tasks. |
| `Task` / `tasks` | Work item, assignee, deadline, status | Nasa isang project at tumutukoy sa member records. |
| `TaskHistory` / `task_history` | Status events, actor, timestamp, review comment | Nakaugnay sa task at actor. |
| `TaskAttachment` / `task_attachments` | Proof file/link metadata | Nakaugnay sa task. |
| `Notification` / `notifications` | In-app event record at read state | Para sa target local account; maaaring may project/task context. |

May foreign keys at indexes ang mga kaugnay na entity. Halimbawa, unique ang project code, at unique ang pares na project/user sa membership at join request para maiwasan ang duplicate records. **Join request ay hindi katumbas ng membership**; ang leader approval pa ang lumilikha ng connected member row. Ang task status at progress ay kinukuha mula sa task records, hindi sa hiwalay na manually edited percentage.

**Mahalagang database consequence:** Ang task ay may foreign-key reference sa assigned at creator member na may `NO_ACTION` kapag dine-delete ang member. Kaya maaaring pumalya ang member removal kung may dependent tasks pa; wala pang automatic reassignment. Nasa Phase 7 ang user-facing caveat.

## 3. Migrations: bakit may version 1 → 10?

Kapag nagbago ang table structure sa bagong app version, kailangan ng **migration** para ma-upgrade ang existing local database nang hindi basta dine-delete ang user data. Nire-register ng `GroupTrackDatabase` ang sunod-sunod na migrations: **1→2, 2→3, …, 9→10**. Sa startup, Room ang magbubukas ng database at mag-aapply ng kailangan nitong migrations ayon sa naka-install na version.

May instrumented migration test na nagsisimula sa lumang version-1 database at ina-upgrade hanggang version 10. Sinusuri nito ang ilang preservation/constraint cases, kasama ang existing account at duplicate project membership. **Hindi ito patunay na lahat ng upgrade paths at device states ay na-manual QA na.**

**Presentation line:** “May migration chain kami para manatili ang existing SQLite data kapag nagbago ang schema. May automated migration test para sa piling preservation at uniqueness rules, pero local device QA pa rin ang kailangan para sa buong upgrade experience.”

## 4. Automatic demo seeding sa current code

Sa `GroupTrackDatabase.getInstance()`, tinatawag ang `DatabaseSeeder.seedAsync(instance)` pagkatapos mabuo ang Room instance. **Walang debug-only guard sa kasalukuyang path**, kaya tumatakbo ang seeder sa anumang build na gumagamit ng database class na ito. Sa fresh database, naghahanda ito ng **tatlong demo accounts**, isang `GroupTrack` demo project na may leader/reviewer/member, **anim na sample tasks** na sumasakop sa workflow statuses/history, at sample notifications.

Sinusuri ng seeder kung umiiral na ang ilang demo records bago gumawa ng bago. Kung may tasks na sa demo project, nilalaktawan nito ang task/history/notification seed section. Asynchronous ang seeding, kaya sa unang bukas ng bagong installation ay maaaring may maikling pagitan bago lumitaw ang demo records; hindi ito dapat ipangakong instant sa live demo.

> [!warning] Current implementation risk
> Kapag mayroon nang seeded demo account, nire-reset ng seeder ang password hash/salt nito sa nakatakdang demo password tuwing muling ginagawa ang database instance sa bagong app process. Maaari rin nitong ibalik ang seeded members sa nakatakdang demo roles. Kaya **huwag gamitin ang seeded accounts para ipakitang permanenteng nananatili ang password o role change pagkatapos ng restart**. Ito ay code behavior na dapat ayusin bago totoong user release; hindi ito normal na requirement ng local account module.

Ang normal na user-created records ay hiwalay sa demo seed, pero ang presence ng sample project sa fresh install ay dahilan kung bakit maaaring hindi agad makita ang “No projects yet” state gamit ang seeded account. Para sa clean empty-state demo, gumamit ng bagong local account na walang project membership o maingat na ihanda ang test data.

## 5. Sample demo scenario

1. Ipakita sa architecture diagram ang **Activity → Repository → Room DAO → SQLite**.
2. Ipaliwanag ang ugnayan: User → Project → Member/Join Request → Task → History/Attachment; Notification ay para sa target local account.
3. Sa fresh prepared demo installation, ipakita ang seeded `GroupTrack` project at sample task statuses. Sabihing demo data ito, hindi live cloud data.
4. Gumawa ng bagong project o task upang ipakita na sa parehong SQLite database din nase-save ang user-created records.
5. Kung tatanungin sa upgrade, banggitin ang version 10 at migration chain; huwag mag-claim ng untested device migration.

## 6. Posibleng panel questions

**Q: SQLite ba o Firebase ang current backend?**  
A: Room over local SQLite. Walang Firebase/Firestore sa current production data path.

**Q: Pareho ba ang database ng dalawang phone?**  
A: Hindi. Bawat app installation ay may sariling local database; wala pang sync.

**Q: Bakit may sample accounts at project agad?**  
A: May automatic demo seeder sa current implementation para sa presentation/test data. Hiwalay iyon sa normal create/join workflow.

**Q: Ano ang ginagawa ng migration?**  
A: Binabago nito ang lumang local schema patungo sa bagong version habang pinananatili ang existing data kung tama ang migration.

**Q: Permanent ba ang change password sa demo account?**  
A: Hindi dapat iyon ipangako sa current code: maaaring i-reset ng seeder ang seeded account sa demo password sa susunod na app process. Ang normal account behavior ay hiwalay na flow sa Phase 2.

## 7. One-minute practice script

“Ang GroupTrack ay gumagamit ng Room over SQLite sa mismong Android device. Ang screen ay tumatawag sa repository, ang repository sa DAO, at ang DAO ang nagbabasa o nagsusulat ng users, projects, memberships, tasks, history, attachments, at notifications. Version 10 ang current schema at may migrations mula sa dating versions para mapanatili ang local data. May automatic demo seeder na naghahanda ng sample accounts, project, tasks, at notifications para sa presentation. Hindi ito cloud sync; ang data ng dalawang phone ay magkahiwalay. May current seeder limitation sa seeded passwords at roles, kaya hindi namin ito ipapakitang permanenteng user change hangga't hindi naaayos ang code.”

## Sources at scope

Sinuri noong **November 3, 2026**: `GroupTrackDatabase`, `DatabaseSeeder`, eight Room entity classes, `SessionManager`, `ProjectRepository`, at `GroupTrackDatabaseMigrationTest`. Source-backed reviewer ito; hindi live device QA at hindi migration test run sa turn na ito. Ang business workflows ay nasa Phases 2–10.
