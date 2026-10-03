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
- **No outside code.** One HTML file with no libraries or analytics, plus the Inter font served from the same site. Its Content-Security-Policy only lets this exact script run, and the page refuses to run inside another website's frame.

## Check that the live page matches this code

Each release publishes the SHA-256 checksums of the two files the page loads. Current version 1.0.17:

```
0e7de31545da39a5b1fd3fe356568ba9c8800877ab1cd4ba363b60799a6f8841  index.html
fc7b8c858bf1106a57a9c5c66d457e9dc4bb047cc93105ce61cc3ccb714d2564  inter.woff2
```

To check it yourself:

```
curl -s https://findfriends.sparkable.cc/ | shasum -a 256
curl -s https://findfriends.sparkable.cc/inter.woff2 | shasum -a 256
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
| `inter.woff2` | The Inter font, cut down to Latin letters with accents and weights 400-800 |
| `OFL.txt` | Inter's license (SIL Open Font License 1.1) |
| `LICENSE` | GNU AGPL-3.0 |

## Hosting

Live on Cloudflare Workers, which builds from `main` using `wrangler.jsonc`. The site is `index.html`, `inter.woff2`, `client-metadata.json` and `og.png`.

Any static host that serves these files at the root of an HTTPS address works. Don't publish the `.git` folder. On Netlify: import this repository, leave the build command empty and set the publish directory to the repository root.

## Publishing

Every merge into `main` starts a Cloudflare build that publishes the site. Other branches get preview builds only. In the Cloudflare dashboard (Worker `findfriends` → Settings → Build → Branch control), the production branch must be `main`; if the live site doesn't change after a merge, check that setting and the build history first.

## Local development

Serve the folder at `127.0.0.1` (not `localhost`), for example:

```
python3 -m http.server 8000 --bind 127.0.0.1
```

Then open http://127.0.0.1:8000/. Sign-in automatically uses atproto's development client there.

## Making a change

1. Edit `index.html`.
2. The browser console will report a Content-Security-Policy error with the new script fingerprint (`sha256-...`). Paste it into the Content-Security-Policy line at the top of the file.
3. Raise the version number at the top of the file and in the footer, and add a line to the version history below.
4. Publish the new checksums in this README.

## Version history

- 1.0.1 first prototype. 1.0.2 strict matching, evidence rule, trust copy.
- 1.0.3 fast search, sections, works with every AT Protocol server.
- 1.0.4 Sparkable wording, match badges, jump bar, bulk select per section.
- 1.0.5 "how it finds people" up top, icons, blog styling, round buttons, "Multiple matches".
- 1.0.6 profile links, translated job words, "you are here" jump bar, inactive = 2 years.
- 1.0.7 question-style intro, page stays put while results load, inactive = never active.
- 1.0.8 twice the searches at once, Spanish and Portuguese job words, lighter intro.
- 1.0.9 compact intro, network footnote, LinkedIn line on top of each card.
- 1.0.10 followed people move to "Following" with undo, back-to-top button, footnote numbers.
- 1.0.11 LinkedIn line back under the bio, friendlier wording, phone rail fix.
- 1.0.12 the bottom bar follows the section you're in: "Select all", then "Follow".
- 1.0.13 no more bouncing at the page bottom, icons in the bottom bar, smarter "inactive".
- 1.0.14 security: only this script may run, app passwords only, confirm unknown servers, paced follows.
- 1.0.15 OAuth sign-in with the narrowest permissions: no passwords at all.
- 1.0.16 Facebook friends lists too; both files can be combined.
- 1.0.17 Sparkable look (blue header, logo, colors), Inter font, link previews with image, GitHub and legal links, "How can I check?" removed.

## License

GNU Affero General Public License v3.0. See `LICENSE`. The Inter font is by The Inter Project Authors under the SIL Open Font License 1.1, see `OFL.txt`.
