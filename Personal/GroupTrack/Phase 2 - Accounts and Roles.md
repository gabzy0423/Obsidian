# GroupTrack Reviewer — Phase 2: Accounts at Roles

> [!summary] Pinakaimportanteng tandaan
> **Account** ang local identity ng tao. **Session** ang kasalukuyang naka-sign-in na account sa device. **Project role** ang pahintulot niya sa isang partikular na project. Hindi awtomatikong leader sa lahat ng projects ang isang user.

## 1. Account flow

**Welcome → Create Account / Sign In → Dashboard → Profile / Settings → Logout**

Sa registration, hinihingi ang full name, email, at password. Tina-trim ang name, at tina-trim at ginagawang lowercase ang email. Tinatanggihan ang kulang na fields at duplicate email. Gumagawa ang repository ng local user row na may unique ID at **password hash at salt**, hindi plaintext password. Pagkatapos ng successful registration, naka-sign in agad ang bagong user at bubukas ang Dashboard.

Sa sign in, chine-check ang required email/password at email format sa screen. Hinahanap ng repository ang normalized email at vine-verify ang password laban sa stored hash. Pareho ang generic na “Invalid email or password” para sa unknown email at wrong password. Kapag successful, sine-save ang user ID sa local session at bubukas ang Dashboard; kapag failed, nananatili sa sign-in screen at may error.

**Presentation line:** “Local account system ang GroupTrack. Ang credentials ay nasa SQLite gamit ang password hashing, habang ang active session ay tumutukoy sa user ID ng kasalukuyang naka-sign in.”

## 2. Session at logout

- Ang `SessionManager` ay nagtatago ng `current_user_id` sa private `SharedPreferences`.
- Ang protected screens ay nagche-check kung may signed-in session at nagre-redirect sa welcome flow kung wala.
- Ang logout ay nag-aalis ng current user ID sa session. **Hindi nito binubura ang account o project rows** sa SQLite.
- Kaya sa iisang installation, maaaring mag-sign out ang isang user at mag-sign in ang ibang local account. Doon gumagana ang local join approval demo.

> [!warning] Limitasyon
> Hindi ito cloud authentication. Ang “same project” sa dalawang magkahiwalay na phone ay hindi magsi-sync sa kasalukuyang MVP. Ang local session at repository role checks ay hindi server-side security laban sa modified app o device compromise.

## 3. Profile at password

Ang current profile screen ay nagpapakita ng name at email. Sinusuportahan din ng current source ang **pag-edit ng full name at avatar** at **pagpalit ng password**. Kapag nagbago ang full name, ina-update din ang linked project-member display names sa isang database transaction. Hindi ipinapakita ang stored password.

Sa change password: kailangan ang current password, valid new password, at matching confirmation. Vine-verify muna ang current password; hindi rin puwedeng pareho ang old at new. Ang password policy sa repository ay **minimum 8 characters na may uppercase, lowercase, at number**.

## 4. Account vs project membership

Magkaibang record ang `User` at `ProjectMember`:

| Term | Ibig sabihin | Halimbawa |
|---|---|---|
| Account/User | Local identity na ginagamit sa sign in. | Si Ana ay may email at password sa device. |
| Session | Aling account ang kasalukuyang gumagamit ng app. | Si Ana ang naka-sign in ngayon. |
| Project membership | Link ng account sa isang project; may role at status. | Si Ana ay leader sa Project A at member sa Project B. |
| Join request | Hiling na makasali; **hindi pa membership**. | Pending pa ang request ni Ben sa Project A. |

Ang project creator ay awtomatikong **leader at connected member** ng ginawa niyang project. Ang ibang account ay makakasali sa pamamagitan ng local join request at approval ng leader. Ipapaliwanag nang buo ang request lifecycle sa Phase 3.

## 5. Mga role sa isang project

| Role | Pangunahing sakop |
|---|---|
| `LEADER` | Project creator/manager: project settings, membership/join review, task creation at assignment, at review ng eligible submitted tasks. |
| `MEMBER` | Nakakakita ng accessible project data at gumagawa ng sarili niyang assigned tasks; hindi puwedeng mag-manage ng project o mag-review nang walang reviewer role. |
| `REVIEWER` | Connected project member na itinalaga para tumulong sa task review; hindi siya kapalit ng leader bilang project manager. |

Sa current code, ang leader ang maaaring mag-promote ng eligible member bilang reviewer at magbalik nito sa member role. Ang authorization ay sinusuri sa repository/policy, hindi lang sa kung visible ang UI button.

**Mahalagang review rule:** walang self-review. Ang task ng ordinary member ay puwedeng i-review ng ibang leader o reviewer; ang task ng leader ay reviewer ang magre-review; ang task ng reviewer ay leader ang magre-review. May espesyal na solo-project case: kung iisang member lang ang project at leader din ang assignee, maaaring kumpletuhin ang `WORKING` task nang diretso. Detalyadong transitions at exceptions sa Phase 5.

## 6. Halimbawang scenario na madaling ikuwento

1. Nag-register si Ana at gumawa ng Project A. Leader siya ng Project A.
2. Nag-sign out si Ana; nag-register o nag-sign in si Ben sa **parehong device installation**.
3. Nag-submit si Ben ng join request. Wala pa siyang project access habang pending.
4. Nag-sign in muli si Ana at in-approve ang request. Connected member na si Ben.
5. Maaaring mag-assign si Ana ng task kay Ben. Si Ben ang gagawa; ang eligible reviewer ang magdedesisyon sa submitted work.

Ang isang taong leader sa Project A ay puwedeng member sa Project B. **Per-project** ang roles at permissions.

## 7. Posibleng panel questions

**Q: Firebase Authentication ba ang gamit?**  
A: Hindi. Local SQLite accounts ang current MVP; ang active user ID ay naka-save sa private SharedPreferences.

**Q: Plaintext ba ang password sa database?**  
A: Hindi. Password hash at salt ang ini-store; vine-verify ang entered password laban sa hash.

**Q: Ano ang epekto ng logout?**  
A: Inaalis ang active session, kaya kailangang mag-sign in muli. Nananatili ang local account at project data.

**Q: Lahat ba ng registered users ay makakakita ng lahat ng projects?**  
A: Hindi. Kailangan nilang maging creator o approved/connected project member para sa project access.

**Q: Pareho ba ang leader at reviewer?**  
A: Hindi. Leader ang namamahala sa project at assignments. Reviewer ay project member na binigyan ng task-review role.

**Q: Puwede bang i-approve ng leader ang sarili niyang task?**  
A: Hindi sa normal review flow. May separate direct-completion exception para sa solo leader na siya lang ang member ng project.

## 8. One-minute practice script

“Sa GroupTrack, local ang user accounts. Magre-register gamit ang name, email, at password; hindi plaintext ang password sa database. Pag successful ang registration o sign in, ang user ID ang itinatago sa local session. Ang logout ay session lang ang inaalis, kaya puwedeng lumipat sa ibang local account sa parehong device. Hiwalay sa account ang project membership: puwede akong leader sa project na ginawa ko at member sa ibang project. May reviewer role din para sa task review, at may repository checks para hindi basta makagawa ang ibang user ng restricted actions.”

## Sources at scope

Sinuri noong **November 3, 2026**: `AuthRepository`, `SessionManager`, `PasswordHasher`, `CreateAccountActivity`, `SignInActivity`, `ProfileActivity`, `ProjectMember`, `ProjectRepository`, `TaskReviewPolicy`, at `docs/04-feature-specifications.md`. Ang role matrix rito ay presentation summary; ang eksaktong project at task operations ay nasa Phases 3–5. Hindi ito live device QA.
