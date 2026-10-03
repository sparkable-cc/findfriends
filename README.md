# Find friends on Sparkable

Find your LinkedIn and Facebook friends on Sparkable, and follow them in a few clicks. Sparkable shares its open network with Bluesky, so the tool finds people on either.

Live at **https://findfriends.sparkable.cc**. Source code: https://github.com/sparkable-cc/findfriends

## How it works

1. You download your contact list from LinkedIn or Facebook and drop it on the page.
2. You sign in with your Sparkable or Bluesky account. Sign-in happens on your own account server's page, and the tool never sees your password.
3. The tool looks up each contact by name and suggests people only when their full name matches **and** something else confirms it's them: people you both follow, or a bio that fits their LinkedIn job. Every suggestion says why.
4. You choose who to follow. Every follow can be undone.

## Privacy and security

- **Your files never leave your device.** They're read inside the browser tab. Only names are sent, as searches to Bluesky's public search.
- **Nothing is stored.** Not by Sparkable, not in the browser. Close the tab and it's gone.
- **Narrow permissions.** Sign-in uses atproto OAuth and asks only to create and delete follows and to read profiles. It can't post, delete posts or change your account.
- **No outside code.** One HTML file with no libraries, fonts or analytics. Its Content-Security-Policy only lets this exact script run, and the page refuses to run inside another website's frame.

## Check that the live page matches this code

Each release publishes the SHA-256 checksum of `index.html`. Current version 1.0.17:

```
064caf0aabbf667f4945f73d877bbe6037ad4397c0ba479a9c6a5de8b7e8b053
```

To check it yourself:

```
curl -s https://findfriends.sparkable.cc/ | shasum -a 256
```

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole app |
| `client-metadata.json` | The OAuth client description. Its web addresses must match the hosting address. |
| `_headers` | Security headers for Netlify or Cloudflare |
| `wrangler.jsonc` | Cloudflare settings |
| `.assetsignore` | Files that stay off the website (README, LICENSE, the `.git` folder) |
| `og.png` | The picture shown when someone shares the link |
| `LICENSE` | GNU AGPL-3.0 |

## Hosting

Live on Cloudflare Workers, which builds from `main` using `wrangler.jsonc`. Only `index.html` and `client-metadata.json` are published.

Any static host that serves these files at the root of an HTTPS address works. Don't publish the `.git` folder. On Netlify: import this repository, leave the build command empty and set the publish directory to the repository root.

## Local development

Serve the folder at `127.0.0.1` (not `localhost`), for example:

```
python3 -m http.server 8000 --bind 127.0.0.1
```

Then open http://127.0.0.1:8000/. Sign-in automatically uses atproto's development client there.

## Making a change

1. Edit `index.html`.
2. The browser console will report a Content-Security-Policy error with the new script fingerprint (`sha256-...`). Paste it into the Content-Security-Policy line at the top of the file.
3. Raise the version number in the footer and in the change log at the top of the file.
4. Publish the new checksum in this README.

## License

GNU Affero General Public License v3.0. See `LICENSE`.
