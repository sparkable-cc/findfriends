# Find friends on Sparkable

Find your LinkedIn and Facebook friends on Sparkable, and follow them in a few clicks. Sparkable shares its open network with Bluesky, so the tool finds people on either.

Live at **https://findfriends.sparkable.cc**. Source code: https://github.com/sparkable-cc/findfriends

## How it works

Everything happens in your browser. There's no Sparkable server behind this page.

1. **Get your friend list** from LinkedIn or Facebook and drop the file on the page. It's read on your device and never uploaded.
2. **Sign in** with your Sparkable or Bluesky account (Eurosky, Blacksky and others work too), on your own account's sign-in page.
3. **See who's here.** The page searches for each contact by name, across the open network that Sparkable and Bluesky share.
4. **Choose who to follow.** Nothing is selected for you, and you can undo every follow.
5. **Tell your friends, if you like.** The page writes a post for LinkedIn or Facebook. Its link puts you first in the results of anyone who opens it. A reminder button adds a calendar entry to search again in a month, made on your device.

### Who gets suggested, and who never does

Finding people can go wrong. Someone may keep their work and private accounts apart on purpose, use a different name to stay out of someone's sight, or simply share a name with a stranger. So the matching is strict. It would rather miss a friend than suggest the wrong person.

- **Full real name only.** An account is considered only if its name contains your contact's complete first and last name. Nicknames, initials, usernames and other surnames never match. Someone who goes by a different name on Sparkable won't be found, and that's on purpose.
- **A name is never enough.** There has to be a second sign that it's the same person, and every suggestion says what that sign is.
- **Blocks are respected.** Nobody who blocked you, or whom you blocked, is ever suggested.
- **Nothing new is revealed.** The page only finds public accounts, and only uses what you could see yourself by searching each name by hand.
- **Your contacts' details stay with you.** Their job, employer, LinkedIn link and email never leave your device. The tool never emails or invites anyone, and it keeps nothing about them.

<details>
<summary>How the signs are scored</summary>

| Sign | Points |
| --- | --- |
| Their bio links to the same LinkedIn profile | 10 |
| They follow you | 5 |
| People you follow who follow them ("people in common") | 2 for one, 4 for three, 6 for six, up to 8. At most 3 for accounts with over 5,000 followers, since big accounts collect shared followers anyway |
| Their bio mentions the company from LinkedIn | 3 or 4 |
| Their bio mentions their job from LinkedIn, also in German, French, Italian, Spanish or Portuguese | 2 or 3 |

A bio that links to a *different* LinkedIn profile rules the account out. Facebook files only contain names, so for Facebook friends the signs are people in common and whether they follow you. If you already follow someone with your contact's name, the page takes that as your contact.

- **Strong matches:** 6 points or more.
- **Likely matches:** 3 to 5 points.
- **Multiple matches:** several people with that name fit and none clearly stands out.
- **Probably inactive:** probably them, but the account has never posted, replied or reposted.
- **Not shown:** under 3 points. "Why isn't someone here?" on the page says how many contacts that was.

</details>

## Why it's safe and private

- **Your file stays on your device.** The page reads it inside your browser and never uploads it.
- **Only names go out.** To search, the page sends each contact's name to Bluesky's public search. The accounts it finds are then looked up through your own account server, to see who you have in common.
- **Nothing is saved.** No database, no cookies, nothing kept in your browser. Close the tab and it's gone.
- **Invite links send nothing.** A link like `findfriends.sparkable.cc/?from=name` only shows that handle. It's looked up after you sign in, never before.
- **It can do very little with your account.** Sign-in uses atproto OAuth and only allows following, unfollowing and reading profiles. The page can't post, send messages, delete posts or change your account, and it never sees your password.
- **No tracking.** No analytics and no code from other companies. The only other file the page loads is its font, from the same site.

## Why it's built this way

- **No server, so nothing to leak.** Many "find your friends" features upload your address book to the company's servers. This one can't: the whole tool is one web page, and there is no server to send your list to.
- **Locked to its own code.** The page tells your browser to run one exact script, identified by its fingerprint, and to refuse everything else. Even if someone slipped extra code into the page, your browser wouldn't run it. The page also refuses to load inside another website.
- **You can check it.** The code is public, and the checksums below let anyone confirm that the live page is exactly this code.
- **One search covers the open network.** Sparkable, Bluesky and other apps share the same open network (the [AT Protocol](https://en.wikipedia.org/wiki/AT_Protocol)), so one search finds people on all of them, and an account from any of them can sign in.
- **Small and quick.** Under 100 KB, fonts included.

## Check that the live page matches this code

Each release publishes the SHA-256 checksums of the files the page loads. Current version 1.0.26:

```
07f8b2c6cafc8875a428aa46c451ff5cc9e486618dc32ce125271f25a703db07  index.html
23d9a7372e637c1e534a28a3f0cfc5201499525bda7d3d858aec47f22e84bfe6  inter.woff2
79b5e33258fc2fc2017df94055a97b7729c132c229ffe4d3ba3da961f9ec7c94  inter-bold.woff2
a95d2e22d10445edc11987b9ca33d2adc4814b63123aeaa9ca805026a847da9a  inter-ext.woff2
18fca7ec756dd5d737f6c4989779c35a33f79110e9c98e719561e069ee4a40ce  inter-bold-ext.woff2
```

To check it yourself:

```
curl -s https://findfriends.sparkable.cc/ | shasum -a 256
curl -s https://findfriends.sparkable.cc/inter.woff2 | shasum -a 256
curl -s https://findfriends.sparkable.cc/inter-bold.woff2 | shasum -a 256
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
| `inter.woff2`, `inter-bold.woff2` | The Inter font in regular and bold, cut down to Latin letters with accents. Two fixed weights on purpose: Safari's Lockdown Mode shows variable fonts in regular only. |
| `inter-ext.woff2`, `inter-bold-ext.woff2` | Extra letters such as ł, č or ş. Browsers load them only when a name on the page needs them. |
| `OFL.txt` | Inter's license (SIL Open Font License 1.1) |
| `LICENSE` | GNU AGPL-3.0 |

## Hosting

Live on Cloudflare Workers, which builds from `main` using `wrangler.jsonc`. The site is `index.html`, the four `inter*.woff2` font files, `client-metadata.json` and `og.png`.

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
3. Raise the version number at the top of the file and in the footer.
4. Publish the new checksums in this README.

## License

GNU Affero General Public License v3.0. See `LICENSE`. The Inter font is by The Inter Project Authors under the SIL Open Font License 1.1, see `OFL.txt`.
