# marinachiropractic.com

Static website for Marina Chiropractic — Dr. Kirsti Neely, D.C. A mobile chiropractic
practice serving San Francisco's Marina District and southern Marin.

Plain HTML and CSS. No framework, no build step, no dependencies, no hosting bill.

---

## Editing and deploying

1. Edit the `.html` files directly. Open them in a browser to preview — there is nothing to compile.
2. Commit and push to `main`.
3. GitHub Pages redeploys automatically, usually within a minute.

That is the entire workflow. If you are changing the phone number, email, or service
area, read [`content.md`](content.md) first — it lists every file those values appear in.

## Structure

```
index.html               Home — leads with the mobile practice
how-it-works.html        What a mobile visit is actually like
services.html            What she offers (and what she no longer offers)
bio.html                 Dr. Neely's background
about-chiropractic.html  General background on chiropractic care
contact.html             Booking and service area
styles.css               All styling
CNAME                    Custom domain for GitHub Pages — do not delete
content.md               Index of every business fact and where it appears
forms/                   New patient intake PDF
images/                  Photos (currently empty — see placeholders)
archive/                 The previous abandoned site attempt, kept for reference
```

There is no templating, so the header and footer are duplicated in each page. Adding a
nav link means editing all six files. This is a deliberate trade: a build step would be
one more thing to break in three years when nobody remembers how it worked.

---

## Two rules, both deliberate

### 1. Never publish a street address

The practice is mobile and has no location patients can visit. The Sausalito address is a
mail drop, used only for the domain registrar and business registration.

If an address goes on the site, patients will drive to it and find nothing. There is also
no `PostalAddress` in the structured data and no embedded map — the JSON-LD uses
`areaServed` instead, which is the correct pattern for a service-area business.

### 2. Never collect health information through this website

The site links to a downloadable blank PDF. Patients fill it in and hand it over on paper.

This matters legally. Patient health information is PHI, and a chiropractic practice is a
HIPAA covered entity, so any service that stores or transmits it must sign a Business
Associate Agreement. A blank form is not PHI, so the current setup has zero compliance
surface and costs nothing.

**Do not** add a serverless function, a Formspree endpoint, or any form with a "describe
your symptoms" field. A free-text box is how patients end up typing their medical history
into a service with no BAA. If a contact form is ever added, restrict it to name, email,
and phone only.

Practice-management platforms (Jane, SimplePractice) were evaluated and rejected: at
roughly 3–12 intake forms per year they work out to over $100 per form.

---

## Decisions waiting on Dr. Neely

- **The "Know your spine" organ chart** from the old Blogger site — mapping individual
  vertebrae to specific organs — has been **omitted** from `about-chiropractic.html`. It is
  not supported by current evidence, and publishing it as fact is the kind of claim that
  draws scrutiny under FTC advertising rules and California Board of Chiropractic Examiners
  regulations. It is commented out in the HTML and easily restored if she wants it.
- **The Monavie recommendation** has been removed from `services.html` and should not be
  restored. Monavie was an açaí-juice MLM that went bankrupt around 2015 amid FTC and
  class-action scrutiny over health claims.
- **"Proven effective" claims** for dizziness, carpal tunnel, low energy, stress, and
  scoliosis have been softened to describe what patients commonly seek care for.
- **The unsourced "30–40% more effective" exercise claim** has been dropped.
- **Insurance.** She does not take insurance, but the site still says to call about it, per
  her preference. Stating the cash price plainly would likely serve her better.
- **Parking-lot operations.** Worth confirming with the City of Sausalito whether treating
  patients commercially in public parks such as Dunphy Park requires a permit.

## Still to add

See the checklist at the bottom of [`content.md`](content.md). The highest-value items are
a photo of Dr. Neely and a photo of the van interior — for a practice where patients meet
an unmarked van in a parking lot, those images do the trust-building that a storefront
used to.

---

## Infrastructure

**Hosting:** GitHub Pages, `main` branch, root. Free, with automatic HTTPS.

**Domain:** registered at GoDaddy. Nameservers stay at `ns45/ns46.domaincontrol.com`.
DNS records pointing the apex at GitHub Pages:

| Type | Name | Value |
| --- | --- | --- |
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| AAAA | @ | 2606:50c0:8000::153 |
| AAAA | @ | 2606:50c0:8001::153 |
| AAAA | @ | 2606:50c0:8002::153 |
| AAAA | @ | 2606:50c0:8003::153 |
| CNAME | www | robregansf.github.io. |

**GoDaddy domain forwarding must be off.** It silently overrides the A records, and it is
what pointed the domain at the old Blogger site.

**Google Business Profile** should be configured as a *service-area business* with the
address hidden and service areas set to the Marina, Cow Hollow, Pacific Heights, Sausalito,
Mill Valley, and Tiburon. For a solo practice this matters more than the website does — it
is where patients actually find her.

## Keeping this from going stale

- A GitHub Actions workflow opens a review issue every six months
  ([`.github/workflows/site-review.yml`](.github/workflows/site-review.yml)).
- Set up a free UptimeRobot check on `https://marinachiropractic.com` to catch certificate
  expiry and silent breakage.
- Keep the registrar's WHOIS contact email current. It was previously GoDaddy's
  `nocontactsfound@secureserver.net` placeholder, which meant renewal notices went nowhere —
  the single most likely way this domain gets lost. Confirm auto-renew is on.
