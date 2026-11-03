# GroupTrack Reviewer — Phase 1: Project Overview at Architecture

> [!summary] Tandaan sa presentation
> Ang **GroupTrack** ay Android app para sa student group projects. Tinutulungan nito ang team na mag-organize ng projects, mag-assign at mag-review ng tasks, at makita ang progress at contributions. **Local sa isang device installation** ang accounts at data; walang cloud sync o remote collaboration sa kasalukuyang MVP.

## 1. Ano ang problema at solusyon?

Sa group project, mahirap makita kung ano ang gawain ng bawat miyembro, alin ang tapos na, at gaano na kalayo ang project. Sa GroupTrack, nakatali ang tasks sa project at assignee, dumadaan ang natapos na gawain sa review, at kinukuha ang progress mula sa task records.

**Maikling sagot sa panel:** “Ang GroupTrack ay local Android project management app para sa student teams. Pinagsasama nito ang project membership, task assignment, review workflow, at progress tracking sa iisang lugar.”

## 2. Sino ang gumagamit?

| Role | Pangunahing ginagawa |
|---|---|
| Project leader | Gumagawa at namamahala ng project; nag-aassign ng tasks at nagre-review ng submitted work. |
| Member | Sumali sa project pagkatapos ma-approve; nakikita at inaasikaso ang assigned tasks. |
| Reviewer | Maaaring italaga para mag-review ng task ayon sa project rules. |

Detalyadong permission at transition rules ang sakop ng Phases 2–5. Ang roles ay **per project**, kaya puwedeng leader ang isang user sa isang project at member sa iba.

## 3. Ano ang sakop ng MVP?

1. Local account registration, sign in, sign out, at basic profile.
2. Project creation at pag-join sa pamamagitan ng request at approval.
3. Task creation, assignment, deadline, at description.
4. Task status workflow at review ng submitted work.
5. Progress, completed/pending task counts, at member contribution counts.

May supporting flows gaya ng archive/restore, comments, proof attachments, at notifications sa current app. Ipapaliwanag ang kani-kanilang rules sa naaangkop na phase.

## 4. Tech stack at bakit ito ang gamit

| Bahagi | Current implementation | Simpleng paliwanag |
|---|---|---|
| Platform | Native Android, isang `:app` module | Isang Android application ang dini-develop at dini-deploy. |
| Production code | Java 11 | Dito nakasulat ang app logic. |
| UI | XML layouts at Material Components | Dito nakadefine ang screens at controls. |
| Local storage | Room over SQLite | Room ang Android library na kumakausap sa local SQLite database. |
| Data access | Repositories at DAOs | Repository ang app-facing operations; DAO ang database queries. |
| Identity | Local accounts at session | Ang sign in ay para sa accounts na nasa installation na iyon. |

Sa current Gradle config: minimum Android SDK **23**, target/compile SDK **37**, at application ID `com.example.grouptrack`. Ang Room database class ay kasalukuyang naka-**version 10**.

> [!important] Architecture wording
> Ang `docs/02-system-architecture.md` ay nagmumungkahi ng lightweight MVVM na may ViewModel. Sa current production source na na-inspect, ang visible implementation ay **Activity → Repository → Room DAO → SQLite**. Huwag sabihing may ViewModel layer na sa lahat ng screens kung hindi ito naipapatupad.

## 5. Paano dumadaloy ang data?

**Halimbawa: mag-create ng task**

1. Naglalagay ng task details ang leader sa Android screen.
2. Ipinapasa ng Activity ang action sa task repository.
3. Sinusuri ng repository ang relevant rules, gaya ng project role at assignee eligibility.
4. Ginagamit ng repository ang Room DAO para i-save ang task sa SQLite.
5. Ipinapakita ng screen ang result o error; kapag nire-load ang list, binabasa muli ang saved data.

Ito ang general pattern; sa Phases 3–6 natin aalamin ang eksaktong validations, status transitions, at calculations.

## 6. Mahahalagang limitasyon na dapat sabihin nang tama

- **Same-device collaboration:** Magkikita ang local accounts sa parehong app installation; hindi mag-si-sync ang project sa magkaibang phones.
- **Offline/local storage:** Walang Firebase o cloud server sa current production path. Hindi kailangan ng internet para sa local core data.
- **Local access checks:** May repository rules para sa roles at membership, pero hindi ito katumbas ng server-side security laban sa modified client o access sa mismong device data.
- **Progress:** Galing sa stored tasks ang counts; hindi hiwalay na user-entered percentage.

## 7. Posibleng panel questions

**Q: Firebase ba ang backend ninyo?**  
A: Hindi sa current MVP. Room over SQLite ang local data layer. Walang remote backend o sync.

**Q: Puwede bang mag-collaborate mula sa dalawang phone?**  
A: Hindi pa. Same-device installation lang ang accounts, projects, at join requests. Cross-device sync ay future scope.

**Q: Bakit Room ang ginamit?**  
A: Pinapasimple nito ang access sa SQLite gamit ang entities, DAOs, at migrations habang local pa rin ang data.

**Q: Paano ninyo hinihiwalay ang UI at database?**  
A: Activities ang nagpapakita ng screen at tumatanggap ng input; repositories ang naghahandle ng app operations; Room DAOs ang gumagawa ng database queries.

**Q: Ano ang ibig sabihin ng progress?**  
A: Nakabatay ito sa dami ng `DONE` tasks kumpara sa kabuuang tasks. Detalyadong formula at edge cases ang Phase 6.

## 8. One-minute practice script

“Ang GroupTrack ay Android app para sa student teams na gustong mas malinaw ang project work at contributions. May local accounts, projects na puwedeng i-create o salihan, tasks na may assignee at deadline, review workflow, at progress view. Java 11 at XML ang production stack, at Room over SQLite ang data storage. Sa kasalukuyang MVP, local sa isang device installation ang data; wala pa kaming cloud synchronization. Pinaghiwalay namin ang UI, repository operations, at Room DAOs para mas madaling sundan ang flow at ma-check ang project rules.”

## Sources at review note

- Project: `README.md`, `docs/01-project-overview.md`, `docs/04-feature-specifications.md`
- Architecture: `docs/02-system-architecture.md`, `app/src/main/java/com/example/grouptrack/`
- Config/data: `app/build.gradle.kts`, `app/src/main/java/com/example/grouptrack/data/database/GroupTrackDatabase.java`

Ang ilang project docs ay may lumang implementation details; ang specific version at layer claims sa reviewer na ito ay sinuri laban sa current source noong **November 3, 2026**. Hindi pa ito substitute sa live demo o device QA.
