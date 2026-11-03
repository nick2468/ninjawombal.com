# ninjawombal.com

My personal homepage: who I am, what I build, and links to my GitHub and LinkedIn.

It is one plain HTML page with no build step. GitHub Pages serves it from the `main`
branch at **https://ninjawombal.com**, and `www.ninjawombal.com` redirects there.

## Layout

| Path | What |
|---|---|
| `index.html` | The whole page, with its styles inline. |
| `img/ninja-wombal.jpg` | The Ninja Wombal, the site's mascot, shown beside the intro. |
| `img/ninja-wombal-face.jpg` | His face, cropped, for the round icon beside the name in the header. |
| `favicon.png` | The browser tab icon: the same face crop. |
| `apple-touch-icon.png` | The icon an iPhone or iPad uses for a Home Screen link. |
| `og.jpg` | The picture a link shows when it is pasted into LinkedIn, Slack or a message. |
| `CNAME` | Tells GitHub Pages which domain this site answers on. Do not delete it. |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are, without Jekyll. |

The colours match the apps site at [nick2468.github.io](https://nick2468.github.io), so
the two read as one family. That site holds the screenshots; this page links to it.

## Preview on this machine

```bash
python -m http.server 8097 --bind 127.0.0.1
```

Then open `http://localhost:8097`.

## Change the page

Edit `index.html`, preview it, then commit and push to `main`. GitHub Pages republishes a
minute or so later.

When an app is added to the apps site, add a card for it under "Projects" here too.

## How the domain is set up

Porkbun runs DNS for `ninjawombal.com`. Three names are in use:

| Name | Points at | Serves |
|---|---|---|
| `ninjawombal.com` | GitHub Pages, by four A and four AAAA records | This page |
| `www.ninjawombal.com` | `nick2468.github.io`, by a CNAME record | A redirect to `ninjawombal.com` |
| `conquest.ninjawombal.com` | `conquest-bih.pages.dev`, by a CNAME record | The Conquest game, on Cloudflare Pages |

GitHub Pages hosts the bare domain because Cloudflare Pages can only serve a bare domain
when Cloudflare runs the domain's DNS, and it does not here.

### The records at Porkbun

The bare domain needs these eight records. The Host field is left blank.

| Type | Host | Answer |
|---|---|---|
| A | (blank) | `185.199.108.153` |
| A | (blank) | `185.199.109.153` |
| A | (blank) | `185.199.110.153` |
| A | (blank) | `185.199.111.153` |
| AAAA | (blank) | `2606:50c0:8000::153` |
| AAAA | (blank) | `2606:50c0:8001::153` |
| AAAA | (blank) | `2606:50c0:8002::153` |
| AAAA | (blank) | `2606:50c0:8003::153` |

And `www` needs one:

| Type | Host | Answer |
|---|---|---|
| CNAME | `www` | `nick2468.github.io` |

Porkbun's parking records for the bare domain and for `www` must be deleted first. They
are an ALIAS or A record on the bare domain and a CNAME on `www`, both pointing at a
`porkbun.com` name. Leave the `conquest` CNAME, the MX mail records and the wildcard `*`
record as they are.

### Verify the domain with GitHub

A verified domain stops anybody else's GitHub Pages site from claiming
`ninjawombal.com` if this site is ever switched off.

1. On GitHub, open your profile **Settings**, then **Pages**, then **Add a domain**.
2. Enter `ninjawombal.com`. GitHub shows a TXT record.
3. At Porkbun, add that TXT record. Its host starts `_github-pages-challenge-nick2468`.
4. Back on GitHub, press **Verify**.

### Turn on HTTPS

GitHub Pages issues the certificate after DNS points at it, usually within an hour.

1. In this repository, open **Settings**, then **Pages**.
2. If **Enforce HTTPS** is greyed out, wait for the certificate and refresh.
3. Tick **Enforce HTTPS**.

### If something goes wrong

**The page shows Porkbun's parking page.** A parking record is still there. Delete it.

**No certificate arrives, and Enforce HTTPS stays greyed out.** GitHub may not retry when
the custom domain was saved before DNS pointed at it. That happened on 2026-11-03. Remove
the custom domain in Settings > Pages, save, then add `ninjawombal.com` back. GitHub
requests the certificate again, and it arrived within two minutes. GitHub records the
change as two commits to `CNAME`, so pull afterwards.

**Settings > Pages says "DNS check unsuccessful".** DNS takes a while to spread. Wait,
then press **Check again**. If the CNAME file was deleted, the custom domain was lost with
it: put `ninjawombal.com` back in Settings > Pages > Custom domain.
