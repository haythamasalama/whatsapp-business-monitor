# Privacy

WhatsApp Business Monitor does not collect, transmit or sell any data. There is no server of ours behind it, and
no analytics or tracking in the app or on the website.

## The app

The app runs on your computer or your own server. Everything it keeps stays in its database there (by default in
`~/.config/whatsapp-templates`):

- your WhatsApp Business accounts and their Meta access tokens;
- your settings, and the users who can sign in, with their passwords stored as salted scrypt hashes;
- a copy of what Meta last answered, so pages open without asking Meta again.

The access token is never sent to the browser or to anyone but Meta. Signing in sets one cookie on the app's own
address to keep you signed in. Deleting the data folder deletes all of it.

### Network access

| Host | Why |
| --- | --- |
| `graph.facebook.com` | Meta's WhatsApp Business APIs: your accounts, templates, numbers and costs |
| `api.github.com` | Once an hour, whether a new version is out. Turn it off with `WHATSAPP_TEMPLATES_UPDATE_CHECK=false` |
| `ghcr.io` | With the Docker install, the updater pulls new versions of the image |

Your browser loads profile photos and template media straight from Meta's servers, as WhatsApp Manager does.

## The website

[wbm.haythamasalama.me](https://wbm.haythamasalama.me) is a static site on GitHub Pages. It sets no cookies, loads
nothing from other sites, and only remembers your light or dark choice in your browser. GitHub hosts it, so
[GitHub's privacy statement](https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement)
covers its server logs.

## Questions

Write to the maintainer through the contact details on [haythamasalama.me](https://haythamasalama.me).
