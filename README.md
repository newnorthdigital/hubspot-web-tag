# HubSpot Tracking GTM Web Tag

A Google Tag Manager template for the **HubSpot Tracking Code** that goes well beyond a simple script loader.

Installs the HubSpot tracking script and exposes the four actions you actually use day-to-day from inside GTM — initialisation, visitor identification, SPA page views, and consent revocation — with regional routing for NA1 and EU1 portals.

[![Maintained by New North Digital](https://img.shields.io/badge/Maintained%20by-New%20North%20Digital-455CE9)](https://newnorth.nl?utm_source=github&utm_medium=gtm-template&utm_campaign=hubspot-web-tag)

## Features

- **Regional routing** — Auto, NA1 (`js-na1.hs-scripts.com`), or EU1 (`js-eu1.hs-scripts.com`)
- **`setContentType`** classification — standard page, landing page, blog post, listing page, knowledge article
- **`identify`** with email + arbitrary custom contact properties via a key/value table
- **SPA page views** with optional `setPath` for virtual URLs
- **Consent controls** — `doNotTrack` and `revokeCookieConsent` for CMP integrations
- **Debug mode** that gates all logging behind a checkbox
- **13 test scenarios** covering every action and every failure path

## Why this instead of the existing one?

The previously available HubSpot template in the Gallery is a five-line script loader with one input field. It ignores `_hsq` entirely — no identify, no SPA support, no consent, no region selector. This template uses the full HubSpot tracking API and matches how teams actually configure HubSpot in 2026.

## Installation

### From the Community Template Gallery

1. In GTM, go to **Templates** → **Tag Templates** → **Search Gallery**
2. Search for **HubSpot Tracking** by New North Digital
3. Click **Add to workspace**
4. Create a new tag using this template

### Manual installation

1. Download `template.tpl` from this repository
2. In GTM, go to **Templates** → **Tag Templates** → **New**
3. Click the kebab menu → **Import** and select `template.tpl`
4. Save

## Setup guide

### 1. Initialisation tag (required, fires once per page)

- **Action type:** Initialisation
- **Portal ID:** your HubSpot Hub ID (numeric, find under Settings → Account Defaults)
- **Region:** Auto, unless you know your portal is hosted on NA1 or EU1 specifically
- **Content type:** optional, only set if you want HubSpot Analytics to classify the page
- **Trigger:** All Pages (Page View)

For traditional multi-page sites this is the only tag you need. HubSpot records page views automatically.

### 2. Identify visitor (optional, after sign-up or form submit)

- **Action type:** Identify visitor
- **Visitor email:** the email you've captured (e.g. from a dataLayer push)
- **Custom contact properties:** add any HubSpot property name (internal name) and value
- **Trigger:** form submit, login event, etc.

HubSpot attaches the identification on the *next* page view or event — it does not fire its own request.

### 3. Track page view (SPA only)

- **Action type:** Track page view
- **Set virtual path:** enable for SPAs where the URL changes without a reload
- **Virtual path:** a Page Path variable that updates on route change
- **Trigger:** route-change event from your SPA framework

### 4. Consent revocation

- **Action type:** Consent
- **Consent action:** `doNotTrack` (stops tracking + clears cookies) or `revokeCookieConsent` (clears cookies, allows future opt-in)
- **Trigger:** consent-revoke event from your CMP

## Field reference

| Field | Type | Action | Notes |
|---|---|---|---|
| Portal ID | text | init | Required, numeric |
| Region | select | init | auto / na1 / eu1 |
| Content type | select | init | optional |
| Visitor email | text | identify | Required |
| Custom contact properties | table | identify | key/value rows |
| Set virtual path | checkbox | page | enables Path field |
| Virtual path | text | page | required when checkbox is on |
| Consent action | select | consent | doNotTrack / revokeCookieConsent |
| Debug | checkbox | all | gates console logging |

## Permissions

- **Logs to console** during preview/debug
- **Accesses `_hsq`** to push tracking commands
- **Injects script** from `js.hs-scripts.com`, `js-na1.hs-scripts.com`, `js-eu1.hs-scripts.com`

## Consent

This template does not read the GTM Consent State directly — use the standard **Consent Settings** on the tag itself in GTM to require `analytics_storage` / `ad_storage` before firing. The built-in Consent action is for *pushing* a consent change into HubSpot's own `_hsq` queue, typically wired to your CMP's revoke event.

## Resources

- [HubSpot Tracking Code API](https://developers.hubspot.com/docs/api/events/tracking-code)
- [How to find your Hub ID](https://knowledge.hubspot.com/account/manage-multiple-hubspot-accounts#find-your-hub-id)
- [GTM Community Template Gallery](https://tagmanager.google.com/gallery/)

## Author

Built and maintained by **[New North Digital](https://newnorth.nl?utm_source=github&utm_medium=gtm-template&utm_campaign=hubspot-web-tag)** — an analytics implementation studio specialising in GTM, sGTM, GA4, and ad-platform conversion tracking.

## License

Apache 2.0 — see [LICENSE](LICENSE).
