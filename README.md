# crathens.com

The public page for Celebrate Recovery in Athens, GA. One file, `index.html`.
No build step. Editing the file and pushing it publishes the site.

## To change something on the page

1. Open `index.html` here on GitHub and click the pencil icon.
2. Make your edit.
3. Scroll down, write one line describing what you changed, click **Commit changes**.
4. Wait about a minute, then reload crathens.com.

If it looks wrong, go to the **Commits** tab, open the commit before yours,
and use **Revert**. Nothing here can be permanently broken.

## What must not go on this page

- Meeting times or an address that isn't confirmed
- Attendee names or photos of attendees
- Anything about what is shared in meetings

## One-time setup (already done — recorded here in case it needs redoing)

**GitHub Pages:** Settings → Pages → Source: Deploy from a branch → `main` / root.
Set the custom domain to `crathens.com` and check **Enforce HTTPS** once the
certificate is issued.

**DNS at Porkbun** — apex A records:

    185.199.108.153
    185.199.109.153
    185.199.110.153
    185.199.111.153

Plus a CNAME on `www` pointing to `<account>.github.io`.
Verify these against GitHub's current published IPs before relying on them.

Leave the MX, SPF, DKIM, and DMARC records alone — those are Zoho email
and are unrelated to the website. Deleting them breaks info@crathens.com.

## Accounts

Everything is registered to the shared ministry account, with credentials in
the team password manager. Nothing here is tied to a personal address.

- Domain + DNS: Porkbun
- Email: Zoho Mail
- Contact list and texting: Flocknote
- Site: GitHub Pages
