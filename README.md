# West Point Society of Switzerland website

Static site, no build step, no dependencies. Upload the folder to Netlify (drag and drop, or connect a Git repo) and it works.

## Files

| File | Purpose |
|---|---|
| `index.html` | Home: hero, who we are, pillars, next event, stay connected |
| `about.html` | Mission and history |
| `events.html` | Upcoming and past events |
| `switzerland.html` | Guide for graduates visiting or moving to Switzerland |
| `join.html` | Eligibility, what membership involves, sign-up form (Netlify Forms) |
| `contact.html` | Contact details and contact form (Netlify Forms) |
| `privacy.html` | Privacy notice (Swiss FADP and GDPR wording, review before launch) |
| `thanks.html` | Confirmation page both forms redirect to |
| `css/style.css` | All styling. Colors and fonts are tokens at the top of the file |
| `assets/logo.png` | Placeholder (gray circle). Overwrite with the WPAOG society logo from the Society Leader toolkit in Sallyport, same file name, square, at least 192x192 |
| `netlify.toml` | Security headers and a commented-out redirect for a secondary domain |
| `sitemap.xml`, `robots.txt` | Search engine files, see below |

Every page carries the same header and footer. When you change the navigation or footer, change it in all eight files (a search-and-replace across the folder does it). There is no contact email on the site; all contact runs through the Netlify forms, which forward to whatever inbox you set in Netlify's form notifications.

## Placeholders to fill before launch

Search the folder for `[` to find every placeholder. In order of importance:

1. Logo: overwrite `assets/logo.png` with the WPAOG society logo (keep the file name).
2. 7 November: venue, time, registration link in `events.html` (page and JSON-LD) and `index.html`.
3. Founders Day 2027: date and city in `events.html`.
4. April 2026 Founders Day: exact day, short recap, photo in `events.html`.
5. "Where we meet" line in `index.html`.
6. Postal address, data protection contact name, and "last updated" date in `privacy.html` (the Swiss data protection act requires a responsible party to be named).
7. Dues once set, in `join.html`.

Remove the "Recognition by WPAOG is in progress" line in `index.html` once recognition is granted, and add the Sallyport society page link to the footer. Leadership and governance sections can be added to `about.html` when the society has officers and bylaws.

## Forms

Both forms use Netlify Forms with a honeypot field. After the first deploy:

1. In Netlify, open Site configuration, then Forms, and confirm `join` and `contact` are detected.
2. Add an email notification so submissions reach the leadership inbox.
3. Optionally enable Netlify's spam filtering.

Submissions are stored in the Netlify dashboard; the privacy notice already says so.

## Domain

Primary: `westpointswitzerland.org` (registered). Every canonical tag, og:url, the sitemap, robots.txt, and the JSON-LD use it. If a short redirect domain (e.g. `wpsch.org`) is added later, add it to the Netlify site first, then uncomment the redirect block in `netlify.toml`.

## SEO files

| File | Purpose |
|---|---|
| `sitemap.xml` | Lists the six public pages. Update `lastmod` when a page changes. |
| `robots.txt` | Allows all crawlers, hides `thanks.html`, points to the sitemap. |
| `assets/og-image.png` | 1200x630 preview image for LinkedIn, WhatsApp, Slack links. |
| JSON-LD in `index.html` | `Organization` schema. Fill in the email and LinkedIn URL placeholders. |
| JSON-LD in `events.html` | `Event` schema for the next event. Update the date, time, venue for each new event, or remove it when there is no upcoming event. |

## Design notes

- Palette: black, cadet gray, gold, off-white. Tokens at the top of `style.css`.
- Type: a Palatino-class serif for headings, Helvetica for body text. Both are system fonts, so nothing loads from Google or any other third party (this keeps the privacy notice true).
- The gray band under the header is the one recurring visual device: the Long Gray Line.
- No cookies, no analytics, no external scripts. If you add analytics later, update `privacy.html` and the Content-Security-Policy in `netlify.toml`.

## Things to confirm with WPAOG

- Whether a society may use the West Point crest or the WPAOG society logo template before formal recognition. The site ships with an original placeholder mark for that reason.
- The exact URL for graduates to update their contact record (the site currently links to westpointaog.org).
- Whether the society should link its Sallyport community page once created.
