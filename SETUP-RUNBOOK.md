# CR Athens — Online Presence Setup Runbook

Work top to bottom. Phases build on each other — Phase 1 needs the accounts
from Phase 0, Phase 4 needs the DNS from Phase 1.

Total recurring cost: **~$33/year** (three domains). Everything else is free.

Web UIs change. Where a screen doesn't match these steps, follow the
service's own current instructions and correct this file afterward.

---

## Phase 0 — Ownership

Nothing gets purchased until this phase is done. The point is that no account
here depends on any one person's continued involvement.

### 0.1 Root email account

- [ ] Go to accounts.google.com and create `crathensga@gmail.com`
      (or the closest available variant).
- [ ] Use a long random password. Write it down on paper for now — it goes
      into the vault in 0.3.
- [ ] Set up 2FA on this Google account with your authenticator app.
      **Reveal and copy the text secret before scanning the QR code**
      (see 0.4 — this applies everywhere).
- [ ] Save Google's backup codes on paper for now.

This address is the registration email for every service below. It is never a
person's mailbox.

### 0.2 Bitwarden accounts

- [ ] You: create a free account at bitwarden.com under **your own personal
      email**.
- [ ] Your CR leader: same, under **their own personal email**.
- [ ] Both of you: Settings → Security → Two-step Login → Authenticator App.
- [ ] Both of you: install the Bitwarden browser extension.

Your Bitwarden identity is yours. The shared vault is a separate thing you
both have access to — that's what makes removing someone clean.

### 0.3 Shared vault

- [ ] From your account: Settings → New organization → select the **Free**
      plan. Name it `CR Athens`. No payment details are required.
- [ ] Open the Admin Console → Collections → New collection →
      name it `CR Athens — shared`.
- [ ] Admin Console → Members → Invite member → your leader's email.
- [ ] Leader accepts the emailed invitation.
- [ ] **You then go back to Members and confirm them.** The invite is a
      two-step handshake and it is not finished until you do this.
- [ ] Move the root Google account password from paper into the shared
      collection as a login item.
- [ ] Add Google's backup codes as a secure note in the same collection.

Free tier is two users and two collections. When a third leader needs
access, that's Bitwarden Families, $40/yr for six people.

### 0.4 The 2FA rule (applies to every service below)

Every time a service offers to set up 2FA:

1. Choose **authenticator app**, not SMS.
2. On the QR screen, click the "can't scan this" / "enter code manually"
   link to reveal the **text secret**.
3. Copy that secret into the shared vault as a secure note, labeled with
   the service name.
4. *Then* scan the QR with your app. Microsoft Authenticator: use
   "Add account" → "Other account."
5. Your leader adds the same secret to their own app. Any TOTP app works —
   same seed produces the same codes.
6. Save the service's recovery codes into the vault too.

Microsoft Authenticator will not show you the secret again after you add it.
If you scan before copying, your leader cannot be added without resetting
2FA on that service.

### 0.5 Master password insurance

- [ ] Write your Bitwarden master password on paper. Seal it. Give it to
      your CR leader.
- [ ] Have them do the same and give it to you.

Bitwarden cannot reset a master password. Nobody at the company can recover
it. Emergency Access is a paid feature, so paper is the free answer.

### 0.6 GitHub

- [ ] Create a GitHub account at github.com using `crathensga@gmail.com`.
- [ ] Set up 2FA per 0.4. Save credentials and seed to the vault.
- [ ] From that account: Settings → Organizations → New organization →
      **Free** plan. Try the name `crathens`.
- [ ] Organization → People → Invite member → your CR leader's **personal**
      GitHub login.
- [ ] Change their role to **Owner** (Settings → People → their row → Role).

Two owners means either of you can be removed later without a password
handoff. That is the whole reason for using an org instead of a personal repo.

### 0.7 Write down who has what

- [ ] Create a plain document listing: who owns which account, who has
      admin, and who takes over if you step back. Store it in the root
      account's Google Drive.

---

## Phase 1 — Domains (~$33/year)

### 1.1 Register

- [ ] Create a Porkbun account using `crathensga@gmail.com`.
- [ ] Set up 2FA per 0.4. Save to vault.
- [ ] Register all three:
      - `crathens.com` ← primary
      - `cr-athens.com`
      - `celebraterecoveryathens.com`
- [ ] For each domain: confirm **WHOIS privacy** is on and **auto-renew**
      is on. Check all three individually.
- [ ] Save the Porkbun login to the shared vault.

`crathens.com` is the one you print. Short, no hyphen to mistype.

### 1.2 Redirects

- [ ] For `cr-athens.com` and `celebraterecoveryathens.com`: use Porkbun's
      URL Forwarding to point each at `https://crathens.com`.
- [ ] Leave DNS for all three at Porkbun. Do not move nameservers.

---

## Phase 2 — Email (free)

Do this early. DNS propagation is the longest wait in the plan.

### 2.1 Zoho account

- [ ] Sign up at zoho.com/mail, choose the **Forever Free** plan.
- [ ] Register with `crathensga@gmail.com`.
- [ ] Enter `crathens.com` as the domain to host.
- [ ] Set up 2FA per 0.4. Save to vault.

Free tier: 5 mailboxes, one domain, webmail and mobile app only. No
Outlook/IMAP unless you pay ~$1/user/month.

### 2.2 Verify the domain

- [ ] Zoho gives you a TXT verification record.
- [ ] In Porkbun: Domain Management → `crathens.com` → DNS → add the TXT
      record exactly as Zoho specifies.
- [ ] Return to Zoho and click Verify. If it fails, wait 15 minutes and retry.

### 2.3 Mailboxes

- [ ] Create `info@crathens.com` — the public address on the website.
- [ ] Create `leaders@crathens.com` — internal.
- [ ] Save both to the shared vault.
- [ ] Stay under 5 total.

### 2.4 Mail DNS records

In Porkbun DNS, add each of these from Zoho's setup wizard:

- [ ] **MX records** — Zoho gives you several with different priorities.
      Add all of them. Delete any pre-existing MX records first.
- [ ] **SPF** — a TXT record at the root. Use Zoho's exact string.
- [ ] **DKIM** — a TXT record at the hostname Zoho specifies. Enable DKIM
      in Zoho's admin console first to generate it.
- [ ] **DMARC** — a TXT record at host `_dmarc` with the value:
      `v=DMARC1; p=none; rua=mailto:info@crathens.com`

`p=none` means monitor, don't reject. Correct for now. Tighten it later
once you've confirmed nothing legitimate is failing.

### 2.5 Test

- [ ] Send from `info@crathens.com` to a Gmail address. Confirm it arrives
      and check whether it landed in spam.
- [ ] Reply from Gmail back to `info@`. Confirm it arrives in Zoho.
- [ ] Open the received message in Gmail → three dots → Show original.
      Confirm SPF, DKIM, and DMARC all show PASS.

Don't move on until all three pass. Email that lands in spam is worse than
no email, because you won't know it's happening.

---

## Phase 3 — Flocknote (free under 40 members)

### 3.1 Account

- [ ] Sign up at flocknote.com using `crathensga@gmail.com`.
- [ ] Register the ministry as `Celebrate Recovery — Athens, GA`.
- [ ] Set up 2FA if offered. Save to vault.

### 3.2 Groups

- [ ] Create a group named `Attendees`.
- [ ] Create a separate group named `Leaders`.

Keep these separate from day one. You will send different things to each,
and merging them later is harder than splitting them now.

### 3.3 Text-to-join

- [ ] Claim your free text-to-join keyword. Something short and easy to say
      out loud — `CRATHENS` or similar.
- [ ] Point the keyword at the `Attendees` group.
- [ ] Note the keyword and the number — both go on the website.

### 3.4 Privacy settings — do not skip

- [ ] Turn on **double opt-in** for signups.
- [ ] Find the member directory / visibility settings. Confirm that
      non-admin members **cannot** see other members' names, emails, or
      phone numbers. Verify this yourself in the settings; do not assume
      the default is private.
- [ ] Confirm replies to broadcasts go only to admins, not to the group.

### 3.5 Test

- [ ] Text the keyword from your own phone. Confirm you get a
      confirmation back and land in `Attendees`.
- [ ] Send one test broadcast to `Leaders` only.
- [ ] Remove your test entry if you don't want to be on the attendee list.

---

## Phase 4 — Website (free)

### 4.1 Repository

- [ ] In the `crathens` GitHub org: New repository.
- [ ] Name it `crathens.com`.
- [ ] Set visibility to **Public**. GitHub Pages requires this on the free
      plan.
- [ ] Check "Add a README file" so the repo initializes.

### 4.2 Files

- [ ] Upload `index.html` to the repo root (Add file → Upload files).
- [ ] Replace the generated README with the provided `README.md`.
- [ ] Add this line near the top of the README, in bold:
      **Nothing sensitive in this repo, ever. No contact lists, exports,
      meeting notes, or testimonies. This repository is public.**

### 4.3 Enable Pages

- [ ] Repo → Settings → Pages.
- [ ] Source: **Deploy from a branch**. Branch: `main`, folder: `/ (root)`.
      Save.
- [ ] Wait for the first deploy. Confirm the site loads at the
      `github.io` URL before touching DNS.

### 4.4 Custom domain

- [ ] Settings → Pages → Custom domain → enter `crathens.com` → Save.
- [ ] Check GitHub's current published Pages IP addresses in their docs.
      As of this writing they are:
      `185.199.108.153`, `185.199.109.153`,
      `185.199.110.153`, `185.199.111.153`
- [ ] In Porkbun DNS for `crathens.com`, add an **A record** at the root
      for each of those four IPs.
- [ ] Add a **CNAME** at host `www` pointing to `<org>.github.io`.
- [ ] **Do not touch the MX, SPF, DKIM, or DMARC records.** Those are email.
      Deleting them breaks `info@crathens.com`.
- [ ] Wait for DNS. Can be minutes, can be a few hours.
- [ ] Once GitHub shows the certificate as issued, check **Enforce HTTPS**.

### 4.5 Domain verification

- [ ] Org Settings → Verified and approved domains → Add `crathens.com`.
- [ ] Add the TXT record GitHub provides, in Porkbun.
- [ ] Verify.

This stops anyone else from pointing a Pages site at your domain if the DNS
is ever misconfigured.

---

## Phase 5 — Fill in and check

### 5.1 Content

- [ ] Open `index.html`. Search for `[[` and replace every placeholder:
      - `[[KEYWORD]]` and `[[FLOCKNOTE NUMBER]]` — from 3.3
      - `[[FLOCKNOTE JOIN LINK]]` — the public join URL from Flocknote
      - `[[LEADER NAME]]` and `[[PHONE]]` — your CR leader's name and
        real cell number
- [ ] Delete the `.todo` CSS rule from the stylesheet block.
- [ ] Decide: keep the `mailto:` link for church inquiries, or swap in a
      Formspree free-tier form.

### 5.2 Review

- [ ] Confirm the trademark footer wording with your CR representative.
- [ ] Have your CR leader read the whole page, with attention to the tone
      of the paragraph about the previous church.
- [ ] Confirm there are **no** attendee names, no photos of attendees, no
      meeting content, and no meeting time or address until a venue is
      signed.

### 5.3 End-to-end test

- [ ] Load `crathens.com` on a phone. Most of your traffic will be mobile.
- [ ] Confirm the padlock — HTTPS working.
- [ ] Text the keyword from a phone that isn't already subscribed.
- [ ] Click the email signup link, complete double opt-in, confirm you
      land in `Attendees`.
- [ ] Submit a church inquiry. Confirm it arrives at `info@crathens.com`.
- [ ] Tab through the page with a keyboard. Confirm links show a visible
      focus outline.

### 5.4 Handoff

- [ ] Sit with your CR leader. Have **them** make one real edit through the
      GitHub web editor and commit it.
- [ ] Have them revert it using the Commits tab.
- [ ] Confirm they can log into Bitwarden and see the shared collection.
- [ ] Confirm they can log into Flocknote and send a broadcast.

The site is not finished until someone other than you has successfully
changed it and put it back.

---

## After launch

- Announce the URL and the text keyword through whatever channels still
  reach the group.
- Revisit DMARC `p=none` once you've seen a few weeks of clean reports.
- When a venue is confirmed: add the meeting night and address to the page,
  and send one broadcast to `Attendees`. That broadcast is the whole reason
  this exists.
