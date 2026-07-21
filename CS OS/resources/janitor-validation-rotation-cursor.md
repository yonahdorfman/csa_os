# Janitor Validation Rotation Cursor

last_batch_end_index: 15
last_run: 2026-07-21
last_full_cycle_completed: —
rotation_batch_size: 15
total_accounts: 46

notes: Fresh cursor as of 2026-07-16 — formalizes what had been an ad hoc daily
"spot-check 2-3 accounts" or "scope to accounts touched by harvest" judgment call
in janitor.md Jobs 2 & 4 into an explicit stalest-first rotation, mirroring
`slack-harvest-rotation-cursor.md`. Priority order is computed fresh each tick from
each account's "last validated" date (stalest first), so this index refers to a
position in that computed order, not a fixed account list. Priority-set accounts
(see `slack-priority-accounts.md`) are validated every cycle, uncapped, and never
consume this rotation's budget — see janitor.md's "Validation Rotation" section.

| Account | Last Validated |
|---------|-----------------|
| Duda | 2026-07-21 |
| Papaya Global {Formerly Azimo} | 2026-07-21 |
| mbg-digital.com | 2026-07-21 |
| Bookaway | 2026-07-21 |
| Pango Pay & Go ltd | 2026-07-21 |
| AutoDS IL | 2026-07-21 |
| HAAT Delivery | 2026-07-21 |
| Riverside.fm | 2026-07-21 |
| SimilarWeb | 2026-07-21 |
| StakeMate | 2026-07-21 |
| runwayer.com | 2026-07-21 |
| DuxGroup | 2026-07-21 |
| Guard.io | 2026-07-21 |
| HoneyBook | 2026-07-21 |
| GameStory Israel | 2026-07-21 |

Not yet validated this cycle (next up): Nexus Mods, Solaredge, Stillfront Group AB, shortical.com.

Priority-set accounts validated every cycle (uncapped, not tracked here — see slack-priority-accounts.md):
AT&T Israel, Altshuler Shaham, Appcharge LTD, Base44, BlueThrone, ClickSoftware, Elementor, Ferryhopper,
Fibi - Israeli International Bank, Forex Club International LLC, Gamdom, GETT, Kape Technologies PLC (Israel),
Lemonade, Loora.AI, Movement-Group, MyHeritage Ltd. main account, Nanit, PayBox - Discount Bank, PeerPlay,
RubyLabs, Simply, Tabnine (Previously Codota), Taboola, Talon Cyber Security, Whalo, boinkers.io BY Acid Labs.
