# Morning Brief controller

Reviewed public orchestration only. The application and encrypted data repositories
remain private. No private application code, source bodies, reports, keys or credentials
are stored in this repository.

The controller connects to an immutable private-application commit, validates protected
`main` and the exact-main `cloud-discovery` Environment, then permits manually selected
checks, read-only snapshot preview, no-send source preparation, or a separately approved
ciphertext-only import and isolated synthetic state acceptance. Import is not authorized
by installation and has not run.

Semantic extraction and briefing generation belong to **Codex**. Codex Cloud cannot
schedule a repository-environment task, so the unattended path runs the official Codex
CLI with the owner's ChatGPT-plan login inside the protected `autonomous-cycle` job.
No local model, model container, paid inference API or automatic Telegram send exists
here. The manual Codex handoff remains the fallback.

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
the gated `deliver` step and nowhere else; validation rejects any other use.

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
- `autonomous-cycle` (`PREPARE_MORNING_BRIEF_CYCLE`): the unattended cycle in one job:
  acquisition as in `cycle-prepare`, then the AI step by the Codex CLI running as a
  separate `agent` OS user (no sudo, no state key, tokens or Telegram credentials; it sees
  only the market-only handoff), then the `handoff-submit` validation and a PREPARED brief.
  It never sends. The Codex login is the owner's ChatGPT-plan `auth.json` (no paid API):
  seeded once as Environment secret `CODEX_AUTH_SEED`, then kept Fernet-encrypted on ref
  `agent-credential` of the private data repository and refreshed after every run.
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

Schedule: the reviewed 6-hourly cron is inert until the owner sets the Environment
variable `MORNING_BRIEF_AUTONOMOUS=enabled`; without it every scheduled run is skipped.
When enabled, only the no-send `autonomous-cycle` steps react to it: one acquisition per
72-hour cycle, resuming an interrupted AI step within the cycle. Telegram delivery stays
the separately approved `deliver` mode.
