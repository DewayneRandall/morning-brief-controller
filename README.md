# Morning Brief controller

Reviewed public orchestration only. The application and encrypted data repositories
remain private. No private application code, source bodies, reports, keys or credentials
are stored in this repository.

The controller connects to an immutable private-application commit, validates protected
`main` and the exact-main `cloud-discovery` Environment, then permits manually selected
checks, read-only snapshot preview, no-send source preparation, or a separately approved
ciphertext-only import and isolated synthetic state acceptance. Import is not authorized
by installation and has not run.

Semantic extraction and briefing generation belong to **Codex Cloud** (ChatGPT plan, no
API key): a Codex Cloud task runs `python -m investment_research.codex_cloud_step` against
the encrypted handoff and publishes a validated, sealed response. No model, paid
inference API or automatic Telegram brief send runs here.

## Required settings

Protect `main`: require pull requests, the `controller-validation` check and an up-to-date
branch; dismiss stale approvals; include administrators; prohibit force pushes/deletions;
require conversation resolution. For a sole owner, zero mandatory external approvals
avoids an impossible self-approval requirement; still review changes before merging.
Use a second reviewer when one is available. No production credentials belong in PR jobs.

Create public Environment `cloud-discovery` with **selected branch `main` only**, no tags
or wildcards. Add `APPLICATION_READ_TOKEN` there: a fine-grained, expiring token selecting
only the private application repository with Contents **Read-only**. Never put it in
chat or this repository. It is used only for privacy verification/checkout. The pinned
commit is explicit in the workflow; changing it requires a reviewed controller change.

For the authorized synthetic state test, create a separate **private** encrypted-only
`DewayneRandall/investment-research-data` repository, empty with Actions disabled. Add
Environment variable `DISCOVERY_STATE_REPOSITORY` naming it, and two separate expiring
fine-grained data-only tokens as Environment secrets `DISCOVERY_STATE_READ_TOKEN`
(Contents read) and `DISCOVERY_STATE_WRITE_TOKEN` (Contents read/write). Neither token
may select the application repository or have other permissions. Never add values to
chat or Git. Keep backend unset/drive and scheduling disabled. The synthetic test
receives no production key; leave the existing original key unchanged. Future production
key provisioning and ciphertext migration require separate approval and complete original
history verification. There is no implicit bootstrap, backend switch or delivery route.

For delivery (after separate approval only), add Environment secrets
`TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` to `cloud-discovery`. They are passed to
the gated `deliver` step and the owner-command step `telegram-commands` and nowhere
else; validation rejects any other use.

## Manual modes

Every mode runs only on protected `main`, in the exact-main `cloud-discovery`
Environment, against the immutable application pin. Write modes require their exact
approval phrase and the first run attempt; logs carry fixed status labels only.

- `check`: source checkout and public-controller security checks only.
- `preview`: read-only rendering from an existing authorized encrypted snapshot.
- `dry-run`: no-send acquisition on a disposable read-only-state copy.
- `state-test` / `import`: completed one-time migration tooling; do not rerun without
  separate authorization.
- `handoff-export` (`EXPORT_MARKET_TASKS_TO_CODEX`) / `handoff-submit`
  (`VALIDATE_CODEX_MARKET_SUBMISSIONS`): recipient-encrypted exchange with the AI
  workspace. Submit validates extractions and, when present, the editorial draft; any
  rule violation rejects the whole response before a save.
- `aaa-refresh-export` (`REFRESH_LATEST_AAA_AND_EXPORT`): the used one-time bound refresh.
- `cycle-prepare` (`PREPARE_MORNING_BRIEF_CYCLE`): acquire all configured sources into
  a copy, prove existing history unchanged, CAS checkpoint, independent readback and,
  with a recipient, a fresh encrypted handoff.
- `autonomous-cycle` (`PREPARE_MORNING_BRIEF_CYCLE`): one scheduled-cycle step. If the
  newest handoff carries a Codex Cloud response for the current state, it is validated
  exactly like `handoff-submit` and a PREPARED brief is saved; otherwise, when the 72-hour
  cycle is due, all sources are acquired and a handoff is exported for the persistent Codex
  Cloud recipient (variables `CODEX_RECIPIENT_PUBLIC_KEY`, `CODEX_RECIPIENT_SHA256`; the
  private key exists only in the Codex Cloud environment). It never sends.
- `telegram-commands` (`PROCESS_TELEGRAM_COMMANDS`): applies the owner's private-chat
  commands (watchlist, own buys/sells, Deep Dive requests) exactly once to the encrypted
  state. No trades. Replies only with `MORNING_BRIEF_TELEGRAM_REPLIES=enabled`.
- `delivery-status`: read-only ledger gates (no send, no write).
- `delivery-activate` (`ACTIVATE_BRIEF_DELIVERY`, `delivery_target=brief-<id>`): owner
  activation after reviewing the private preview of exactly that brief.
- `deliver` (`DELIVER_PREPARED_MORNING_BRIEF`): sends the prepared brief's two
  messages exactly once; the only step that receives Telegram credentials.
- `delivery-reconcile` (`RECONCILE_BRIEF_DELIVERY`,
  `delivery_target=brief-<id>:<1|2>:<DELIVERED|NOT_DELIVERED>`): owner-confirmed outcome
  of an uncertain send, from the chat history. Never guessed or retried automatically.
- `portfolio-import` (`IMPORT_ORIGINAL_PORTFOLIO`, `snapshot_commit`/`snapshot_sha256` =
  the owner-pushed `portfolio-recovery` ciphertext commit/checksum): validates the
  ORIGINAL portfolio bundle and imports it once under `personal/`; never overwrites.

Schedules are inert until the owner sets the Environment variables:
`MORNING_BRIEF_AUTONOMOUS=enabled` (6-hourly `autonomous-cycle`: export when due, submit
when answered) and `MORNING_BRIEF_TELEGRAM=enabled` (15-minute `telegram-commands`).
Telegram brief delivery stays the separately approved `deliver` mode.
