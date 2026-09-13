# CLAUDE.md — working rules for this repository

These rules apply to every person and every AI assistant working here. They are the
operational form of the ObiLabs engineering principles (canonical, private:
`obilabs/north-star` → `PRINCIPLES.md`, Engineering & Security). Do not copy the canon into
this repo; point at it.

## Commits and identity

- Commit as `Michael Agu <36439190+openmoto@users.noreply.github.com>` (or your own GitHub
  noreply address). Never a personal email: GitHub push protection and the ObiLabs git hooks
  reject it.
- **No AI co-author trailers** (`Co-authored-by: Claude …`) and no "Generated with Claude
  Code" lines in commits or PR bodies. AI use is disclosed once, in the README footer.
  `.claude/settings.json` in this repo turns Claude Code's attribution off; keep it.
- Work on a branch and land it through a pull request. Never commit to, force-push, or
  `git branch -f` `main`. Main is protected where the plan allows it, and the ObiLabs
  pre-push hook refuses direct pushes everywhere.

## What never goes in the repo

- Secrets, tokens, private keys, `.env` files (only `.env.example`), service-account JSON.
- Absolute paths or anything containing a username (`C:\Users\…`, `/home/…`, `/Users/…`): <!-- hygiene:allow -->
  use relative paths or configuration.
- Real tenant, customer or person data in fixtures: use `example.com` / `example.org`.
- Internal IPs and hostnames in public repositories.

The shared hygiene workflow (`.github/workflows/hygiene.yml`) checks all of this on every
push and pull request.

## Product rules this repo must follow

- **Admin bootstrap:** the first admin is claimed with a one-time setup token printed to the
  logs at first boot. Never ship admin credentials in `.env`, seed files or docs. Account
  creation never accepts a role from the client; elevation is a separate audited action by an
  existing admin; the last admin cannot be removed.
- **Secrets fail closed:** the service refuses to start without real secrets. No hard-coded
  fallback values outside tests.
- **Feature maturity:** every feature flag is `stable`, `preview` or `experimental`.
  Development runs everything; a versioned release ships stable on, preview off (an admin can
  enable it), experimental absent. See `docs/RELEASING.md`.
- **Honest status codes:** a failure never returns HTTP 200 with `success: false`.
- **Under-promise:** README and UI copy describe what works today; AI features are optional
  enhancements, never the engine.

## Public repositories

Commit messages, PR titles and PR bodies use routine engineering wording. Never narrate a
security finding in public; the reasoning lives in north-star.

## Tooling

- Install the ObiLabs git hooks once per machine: `sh tooling/git-hooks/install.sh` from a
  clone of `obilabs/.github`.
- Build locally for the loop; CI is for verification and releases.
