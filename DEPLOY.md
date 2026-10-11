# Deploying westpointswitzerland.org

Static site on Netlify. One-time setup, about an hour including DNS propagation.

## 1. Deploy to Netlify

1. Sign in at app.netlify.com. Add new site, then "Deploy manually" (drag and drop) or "Import an existing project" if you keep the folder in a Git repository.
2. Drop the site folder (the one containing `index.html` and `netlify.toml`). Netlify reads `netlify.toml` for the publish folder and the security headers.
3. The site goes live at a random `something.netlify.app` address. Open it and click through every page.
4. Rename the site (Site configuration, Site details, Change site name) to `westpointswitzerland`. Not required, but it keeps the fallback address readable.
5. Forms: open Site configuration, Forms, and confirm `join` and `contact` were detected. Under Form notifications add an email notification for each, pointed at the inbox that should receive submissions. Submit a test on each form and confirm both the thank-you page and the email arrive.

Git deploys are the better long-term choice for a webmaster: push a change, Netlify redeploys. Drag and drop is fine for launch.

## 2. Connect the domain

In Netlify: Domain management, Add a domain, enter `westpointswitzerland.org`. Netlify adds both the apex and `www`. Set the apex (no www) as the primary domain; Netlify then redirects `www` to it automatically.

Then choose one of two DNS approaches.

**Option A: Netlify DNS (simplest).** Netlify shows four nameservers (`dns1.p0X.nsone.net` and so on, the exact ones are listed in Domain management). At your registrar, replace the existing nameservers with those four. Netlify then manages all records. If you ever need email on the domain, you add the MX records in Netlify's DNS panel.

**Option B: Keep DNS at the registrar.** Add two records in the registrar's DNS manager:

| Type | Host | Value |
|---|---|---|
| A | `@` (apex) | `75.2.60.5` |
| CNAME | `www` | `westpointswitzerland.netlify.app` |

Delete any existing A or CNAME records on `@` and `www` first (registrars usually park a placeholder there). Keep everything else.

Propagation takes minutes to a few hours. Check with `dig westpointswitzerland.org` or whatsmydns.net. Netlify's Domain management page shows a green check once it sees the records.

Recommendation: Option A if the domain has no email or other services attached yet. Option B if you want all your domains managed in one registrar panel, as with piercingpoints.

## 3. HTTPS

Nothing to install. Once DNS resolves to Netlify, it requests a Let's Encrypt certificate automatically (Domain management, HTTPS). If the panel says "Waiting on DNS verification", wait and refresh; if it stays stuck for more than a few hours, click Verify DNS configuration, then Provision certificate.

After the certificate is issued, confirm:

- `https://westpointswitzerland.org` loads with the padlock.
- `http://westpointswitzerland.org` and `https://www.westpointswitzerland.org` both redirect to the primary. Netlify's "Force HTTPS" is on by default for sites with a certificate.

Optional hardening: once everything works, add HSTS by appending `Strict-Transport-Security = "max-age=31536000; includeSubDomains"` to the headers block in `netlify.toml`. Only do this after you are sure HTTPS is stable, since browsers will refuse plain HTTP for a year afterwards.

## 4. Google Search Console

1. At search.google.com/search-console, Add property. Choose the **Domain** property type and enter `westpointswitzerland.org`. This covers http, https, www, and non-www in one property.
2. Google gives a TXT record (`google-site-verification=...`). Add it to DNS: in Netlify DNS if you chose Option A, at the registrar if Option B. Host `@`, type TXT, value as given.
3. Wait a few minutes, then click Verify. DNS verification can take up to a day on a slow registrar; it usually works within 15 minutes.
4. Sitemaps (left menu): enter `sitemap.xml` and submit. Status should read "Success" with 7 discovered URLs within a day.
5. URL inspection: paste `https://westpointswitzerland.org/` and click Request indexing. Do the same for `events.html` and `join.html`. The rest follows from the sitemap.
6. Expect the first impressions in Search Console a week or two after launch. "Discovered, currently not indexed" in the first days is normal.

## 5. Structured data

The site carries two JSON-LD blocks: `Organization` on the home page and `Event` on the events page. Check them with Google's Rich Results Test (search.google.com/test/rich-results) by pasting each page URL after deploy. The `Event` block will warn about placeholder values until the venue and time are filled in. Fill the placeholders listed in `README.md` first.

Keep the `Event` block current: update it whenever the next event changes, or remove it when there is no upcoming event. Stale event markup is worse than none.

## 6. Social previews

Paste the home page URL into LinkedIn's Post Inspector (linkedin.com/post-inspector) once live. It should show the black preview card with "Home of the Long Gray Line in Switzerland." If LinkedIn shows an old or empty preview after a later change, run the inspector again to clear its cache.

## 7. Email on the domain

The site has no contact email; both forms forward to the inbox set in Netlify's form notifications. If the society later wants an address on the domain, most registrars offer free forwarding to an existing mailbox. That adds MX records in DNS and does not affect the website records above.

## Routine updates

1. Edit the HTML files locally.
2. Update `lastmod` in `sitemap.xml` for pages you changed.
3. Drag the folder onto the Netlify site's Deploys page (or push to Git). Netlify keeps every deploy; a bad one can be rolled back from the same page.
