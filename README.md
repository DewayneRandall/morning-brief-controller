# Morning Brief controller

Reviewed public orchestration only. The application and encrypted data repositories
remain private. No private application code, source bodies, reports, keys or credentials
are stored in this repository.

The controller connects to an immutable private-application commit, validates protected
`main` and the exact-main `cloud-discovery` Environment, then permits manually selected
checks, read-only snapshot preview, no-send source preparation, or a separately approved
ciphertext-only import and isolated synthetic state acceptance. Import is not authorized
by installation and has not run.

Semantic extraction and briefing generation belong to **Codex Cloud**. No local model,
model container, inference provider API, Telegram delivery or active schedule exists
here. GitHub Actions does not impersonate or launch a Codex model session. Until an
unattended Codex Cloud execution with the required environment/access is separately
verified, the operator starts the Codex step manually.

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

## Manual modes

- `check`: source checkout and public-controller security checks only.
- `key-check`: first-attempt approval `VERIFY_ORIGINAL_KEY_ONLY`; decrypts a benign
  challenge from the authenticated read-only Drive validator using the original
  Environment key. Prints only compatibility status, never key/plaintext. No data
  tokens, state reads/writes, key changes or artifacts. No production-key challenge
  is generated here; provide only the validator's fixed-test-phrase ciphertext.
- `preview`: read-only rendering from an existing authorized encrypted snapshot.
- `dry-run`: no-send acquisition on a disposable read-only-state copy; new semantic tasks
  remain explicitly pending for Codex Cloud. No state writer is supplied.
- `state-test`: exact approval `RUN_SYNTHETIC_STATE_TEST_ONLY`, first attempt only;
  generated temporary test key, isolated random branch, real encrypted write/readback,
  fresh-process restart, backup/history/CAS/irreversible checks. Both data credentials
  are supplied only to this explicit step; no production key or artifacts. Production
  refs must be absent/unchanged. Test history is ciphertext only and retained privately.
- `import`: first-attempt, exact-approval/checksum ciphertext import; do not run without
  separate migration authorization.

Native scheduling tools are visible in the Codex environment, but an unattended Cloud
run with the required access has not been established. Installation does not authorize
or create an automation. Existing source completeness, freshness, evidence, identity,
encryption and receipt checks remain the private application's responsibility.
