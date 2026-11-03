# GroupTrack Reviewer — Phase 9: Settings and Legal

> [!summary] Pinakaimportanteng tandaan
> Sa **Settings** makikita ang profile, change password, Android notification toggle, Privacy Policy, Terms & Conditions, at logout. **Local SQLite** ang live app data at walang GroupTrack cloud sync sa current MVP. Hiwalay rito ang posibleng Android system backup ng app data.

## 1. Settings screen at navigation

May profile preview ang Settings: current name, email, at avatar. Mula rito, mabubuksan ang **Profile** para i-edit ang name/avatar, **Change Password**, **Privacy Policy**, **Terms & Conditions**, at **Logout**. May bottom navigation papunta sa Projects, Tasks, at Progress. Ang account creation, profile validation, session, at password rules ay detalyado sa Phase 2.

## 2. Android notification setting

Ang Notifications row/switch ay nag-o-on o off ng **Android system notification display** para sa current local account. Kapag in-enable sa Android 13 pataas at kailangan pa ng `POST_NOTIFICATIONS` permission, hihingin ito sa user. Kung tinanggihan, mananatiling off ang switch. Isinasaalang-alang din ng displayed switch ang system-level notification availability. Kapag in-enable, sini-schedule ang local deadline check.

**Hindi binubura ng switch ang in-app notification records sa SQLite.** Puwede pa ring buksan ang Notifications screen at basahin ang mga record; nasa Phase 8 ang buong notification flow. Walang cloud push service sa current MVP.

## 3. Change Password at Logout

Ang Change Password dialog ay humihingi ng current password, bagong password, at confirmation. May field validation at confirmation bago i-save. Vine-verify ng account repository ang current password at password policy; hindi ipinapakita ang stored password. Ang eksaktong password requirements ay nasa Phase 2.

Ang Logout ay may confirmation dialog at processing state bago i-clear ang local session at bumalik sa Sign In. **Hindi nito dine-delete ang local account o project records**; puwedeng mag-sign in muli sa parehong device. Ang session rule ay nasa Phase 2.

## 4. Privacy Policy at Terms & Conditions

Mababasa ang dalawang text dialog sa **Create Account** at sa **Settings**. Hindi kailangan munang mag-register para mabasa ang mga ito sa Create Account screen.

| Entry | Ano ang ipinapaliwanag sa current text? |
|---|---|
| Privacy Policy | Account, profile, project, membership, at task data ay local sa SQLite; walang cloud sync sa current MVP; approved local members ay maaaring makakita ng project data; notification permission ay para sa task/project updates; maaaring mawala ang data kapag na-clear ang app data o na-uninstall ang app. |
| Terms & Conditions | Gamitin ang app para sa lawful academic/team work; maglagay lamang ng impormasyong puwedeng ibahagi; ingatan ang credentials; panagutan ang accuracy ng project/task data; maaaring magbago ang MVP habang pinapahusay. |

Ang mga entry ay **readable dialogs**, hindi hiwalay na web pages o cloud-hosted documents. **Hindi tinatalakay ng kasalukuyang Privacy Policy text ang Android OS backup.** Naka-`allowBackup="true"` ang manifest at walang explicit exclusions sa kasalukuyang backup rules. Ayon sa Android Auto Backup documentation, maaaring maisama sa system backup ang app database, internal files, at shared preferences depende sa device at user settings. Kaya huwag sabihing “walang cloud backup”; ang tumpak na claim ay **walang sariling GroupTrack cloud sync o backup service**. Dapat pagpasiyahan ng team kung pahihintulutan ang OS backup at iayon doon ang app configuration at privacy text bago release.

**Presentation line:** “Mababasa ang privacy at terms bago gumawa ng account at muli sa Settings. Sa current MVP, SQLite sa device ang live data at walang GroupTrack cloud sync. Naka-enable pa ang Android backup capability sa manifest, kaya hindi namin ipapangakong walang posibleng system backup.”

## 5. Sample demo scenario

1. Buksan ang Create Account screen at ipakita ang Privacy Policy at Terms & Conditions links.
2. Mag-sign in at buksan ang Settings; ipakita ang profile preview at parehong legal entries.
3. Ipaliwanag ang Android notification toggle at device permission. Kung hindi kailangang i-demo ang permission prompt, huwag baguhin ang device setting sa live presentation.
4. Ipakita ang Change Password dialog nang hindi binabago ang demo credential.
5. Ipakita ang Logout confirmation; ituloy lamang kung iyon ang huling hakbang ng demo.

## 6. Posibleng panel questions

**Q: Kapag nag-logout, mabubura ba ang project data?**  
A: Hindi. Local session lang ang inaalis; nananatili ang account at project records sa app data ng device.

**Q: Kapag tinanggihan ang notification permission, hindi na ba gagana ang Notifications screen?**  
A: Gumagana pa rin ang in-app local records. Hindi lang maipapakita ang Android system notifications kung walang permission.

**Q: May cloud backup ba ang personal at project data?**  
A: Walang sariling GroupTrack cloud backup o sync service. Pero naka-enable ang Android backup capability sa current manifest, kaya maaaring ma-back up ang app data ng OS depende sa device at user settings. Hindi pa ito na-device QA.

**Q: Saan mababasa ang privacy at terms bago mag-register?**  
A: Sa Create Account screen. Mababasa muli ang mga iyon sa Settings pagkatapos mag-sign in.

## 7. One-minute practice script

“Ang Settings ang entry point para sa profile, password, Android notification display, privacy, terms, at logout. Ang profile at password changes ay dumadaan sa local account repository. Ang notification switch ay para sa system display; hindi nito binubura ang in-app notification records. Sa Create Account at Settings, nababasa ang Privacy Policy at Terms & Conditions. Local SQLite ang live data at walang GroupTrack cloud sync, pero maaaring magkaroon ng Android system backup dahil naka-enable ito sa manifest. Ang logout ay session lang ang inaalis, kaya nananatili ang local records.”

## Sources at scope

Sinuri noong **November 3, 2026**: `SettingsActivity`, `CreateAccountActivity`, `ProfileActivity`, `AuthRepository`, `SessionManager`, `AndroidNotificationHelper`, `DeadlineNotificationReceiver`, `strings.xml`, `AndroidManifest.xml`, `backup_rules.xml`, at `data_extraction_rules.xml`. Android backup behavior reference: [Android Developers — Auto Backup](https://developer.android.com/identity/data/autobackup). Presentation reviewer ito batay sa current source; hindi live device QA o backup/restore test. Ang detalye ng account at password rules ay nasa Phase 2; notification list at routing ay nasa Phase 8.
