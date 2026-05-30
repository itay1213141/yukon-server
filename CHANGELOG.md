# Changelog — Yukon Server

Human-readable, organized history of the Yukon **game server** (Node + Socket.IO + Sequelize/MySQL).
Synthesized from the full git history (408 commits, May 2020 → Aug 2025). Grouped by release.
Highlights only — see `git log` for every commit.

Versioning: `MAJOR.MINOR.PATCH-beta`.

---

## [1.11.0-beta] — 2025-08-23 (current)

Theme: **dependency refresh, rate-limiting/cooldown system, user-class cleanup.**

- Added a per-event **cooldown system** (`config.cooldowns`, defaults `send_emote`/`send_frame`
  = 250ms) enforced in `Server.onMessage`/`BaseHandler`.
- Cleaned up **rate limiting** (`rate-limiter-flexible`): per-IP connects, per-IP events,
  per-user events.
- Simplified the User/GameUser class hierarchy; added an `anonymous` getter on the user object
  (safe public fields, no coins/password).
- Better server error messages.
- Major dependency updates: Sequelize, bcrypt, pm2, uuid, rimraf, rate-limiter-flexible, etc.

---

## [1.10.1-beta] — 2024-10-20

- Login fixes.

## [1.10.0-beta] — 2024-10-06

- Added new rooms to `rooms.json` (incl. missions/Agent-related), wired missions to rooms.
- Send tour/agent postcards when receiving an item; handle `send_joke` and `send_tour`.

---

## [1.9.0-beta] — 2024-03-21

Theme: **Pets / Puffles system (complete).**

- Full puffle system: adopt (with coin/limit/type validation), pet model + collection,
  stats (energy/health/rest, happiness virtual field, draining over time, "ran away"),
  interactions (play, feed, rest, bath, gum, cookie), movement/frames, walking a pet
  (sets pet as hand item), "feed me" postcards (with `feedPostcardId` to prevent re-sending).
- Send all pet updates in one event; fix offline `getPets`; date-precision fix on postcards.

---

## [1.8.0-beta] — 2024-01-06

Theme: **Mail / Postcard system.**

- Send/read/delete mail; full postcard data in `receive_mail`; system mail
  (`addSystemMail`); Card-Jitsu award postcards; allow delete of system mail; sender names.
- SQL updates for system mail (`postcards` model changes).

---

## [1.7.0-beta] — 2023-11-17

Theme: **Sled Racer + Puck + crash hardening.**

- Reimplemented `SledInstance` (movement, game-over coin handling); moved `handleLeaveGame`
  to `BaseInstance`.
- Added the **Puck** (ice rink hockey) plugin.
- Merged a server-crash exploit fix (community PR #5). Fixed `!jr` command.

---

## [1.6.0-beta] — 2023-10-25

Theme: **Card-Jitsu + Sensei (complete) + waddle/instance/matchmaking framework.**

- **Waddle** system (game lobbies) and generic **instance** system (`BaseInstance`,
  `InstanceFactory`).
- **Card-Jitsu**: dealing, picking, win logic (element + 3-element + color), power cards &
  effects (replace element, discard effects, no-pow when can't beat sensei), real user decks,
  starter deck plugin, `cards` table + `CardCollection`, ranking system (`ninjaRank`/`ninjaProgress`).
- **Card-Jitsu Sensei**: AI opponent (`SenseiInstance`/`SenseiNinja`), matchmaking
  (`CardMatchmaker`, ranked by ninja rank, tick-based), sensei room.
- User **dynamic events** system (per-user `EventEmitter` for transient in-game listeners).
- Server-side validation fixes to prevent invalid dealing.

---

## [1.5.0-beta] — 2022-11-07

Theme: **Board games + plugin/handler refactor + security.**

- Board games: **Mancala** and **Find Four** (`BaseTable`, `FourTable`, `MancalaTable`,
  `TableFactory`); turn validation, game over, `toJSON` table representations.
- Refactored to the plugin architecture: `BaseHandler`/`GameHandler`/`LoginHandler`,
  `PluginManager`, moved login logic into a plugin, renamed/relocated plugin dirs.
- `babel-plugin-module-resolver` import aliases; moved static game data to JSON
  (`@data/data`); SQL/table renames.
- Validation/security fixes; rate-limit fixes; limit by userID when available; rimraf build.
- Merged "digest login key before hashing" (PR #1) and require-vs-import data.js fix (PR #3).

---

## [1.4.0-beta] — 2022-08-22  (and 1.3-beta — 2022-03-09)

Theme: **Igloos, furniture, moderation, world infra.**

- Igloos: join igloo, add igloo, update igloo/flooring/music, save/load furniture, furniture
  inventory, open-igloo directory (`OpenIgloos`), `PurchaseValidator`.
- Moderation: kick, temp bans + perma bans, rank system (int ranks), `!ac`/`!ai`/`!jr`/`!id`
  test commands.
- World infrastructure: **PM2** multi-world startup (`ecosystem.config.js`), per-world ports,
  server population tracked in the `worlds` table, full-room handling, preferred spawn.
- Auth: merged `auth_token` with `game_auth`; rate limiting on new connections + toggle.
- HTTPS support; server path set to `/`.
- Crumbs merged into a single object; many items handled client-side.

---

## [1.1.x–1.2 / early] — 2021

Theme: **core multiplayer foundation.**

- Login server split from game server; login validation; login-key flow.
- Rooms (join/leave, filtered room broadcasts), chat + safe chat + emotes + snowballs.
- Inventory & items (DB-persisted), coins, clothing slots.
- Buddy system (requests/accept/reject/remove, online/offline, find), ignore list.
- Sequelize models + associations, `BaseModel`, per-user DB **collections**.
- Auth tokens ("remember me"), rate limiting, furniture tables.
- Sled game basics, waddle networking basics.

---

## [Initial] — 2020-05-20

- Initial commit: server + database files, `PluginManager`, first plugins (login, join,
  actions), inventories, coins, user system, basic room/chat handling.

---

### Notes for maintainers
- One OS process per world (`src/World.js <WorldName>`); dev runs `Login`+`Blizzard` together
  via `babel-watch`, prod via PM2 from `dist/`.
- Tags exist through `1.10.1-beta`; `1.11.0-beta` is set in `package.json` (HEAD), possibly untagged locally.
- This fork (`itay1213141/yukon-server`) tracks upstream `wizguin/yukon-server` with no extra
  local commits yet.
- See `../docs/09-upstream-issues.md` for open upstream PRs/issues (security & content) worth merging.
