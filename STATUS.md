# Deployment Status — Maruti Builders site

Last updated: 2026-09-08

## Summary
Static HTML/CSS/JS site, deployed to Vercel via GitHub auto-deploy, custom domain in progress.

## Repo
- GitHub: https://github.com/sapl-shreshta/maruthi-builders (branch `main`)
- Push access: only accounts with write access to `sapl-shreshta` org/user can push directly.
  (The `harika-reddy-thoom` GitHub account got a 403 trying to push — had to be pushed
  from an account with access instead.)
- Local working copy: `C:\Users\ADMIN\Documents\GitHub\Maruti Builders` (git initialized, remote `origin` set to the repo above).

## Vercel
- Account: `sapl-shreshta` (team/scope `shreshta1`)
- Project: `maruthi-builders` — auto-deploys on every push to `main` (no vercel.json needed, static site).
- Production URL (Vercel subdomain): https://maruthi-builders.vercel.app
- Note: Vercel is linked to the GitHub account at the account level, but only repos with
  an explicit Vercel **Project** created get auto-deployed. Other repos under the same
  account are unaffected.

## Custom domain — maruthi-builders.com (registered at GoDaddy)
DNS is managed at GoDaddy (nameservers NOT switched to Vercel — records added directly instead).

Records added at GoDaddy:
| Type  | Host | Value                  |
|-------|------|------------------------|
| A     | @    | 76.76.21.21            |
| CNAME | www  | cname.vercel-dns.com   |

Status as of last check:
- `www.maruthi-builders.com` — ✅ live, HTTPS working (200 OK)
- `maruthi-builders.com` (apex) — DNS resolves correctly to 76.76.21.21, but SSL cert was
  still being issued by Vercel as of last check (schannel/SNI error, not a DNS problem).
  This is expected to resolve on its own within ~1 hour of DNS propagating; no action needed
  unless it's still failing after that window.

## Next steps / how to resume
1. Check apex domain cert status:
   ```
   curl -v https://maruthi-builders.com
   ```
   or
   ```
   npx vercel domains inspect maruthi-builders.com
   ```
   Looking for a clean TLS handshake / HTTP 200 instead of the SNI/cert error.
2. If still not resolved after a few hours, check https://vercel.com/shreshta1/maruthi-builders/settings/domains
   for the domain's actual verification/cert status, or re-run `vercel domains verify maruthi-builders.com`.
3. Once apex cert is confirmed, optionally set a redirect so one of apex/www is canonical
   (Vercel project settings → Domains → set redirect direction).

## Tooling notes for whoever picks this up
- Vercel CLI is used via `npx vercel ...` (no global install). Requires `npx vercel login`
  (device-code browser login) before any project/domain commands will work in a new session.
- `gh` CLI was already authenticated as `harika-reddy-thoom` in the original session (read access
  confirmed; write access to the target repo was NOT available under that account).
