# Ved Transfer

**Your Files. Your Key. Your Control.**

Send big files anywhere in the world with a secure sharing key. Upload, get a key, share it, and your recipient downloads when they're ready, until the transfer expires. Free to start, no app to install.

**Live site:** https://devtiwari-bit.github.io/ved-transfer-/

> **Status: early development.** The website (all pages and the interface) is live, but **sending and receiving files do not work yet**. The backend that stores and delivers files is still to be built. Nothing you add on the Send page is uploaded anywhere, and the pages say so.

---

## What it does

Ved Transfer combines the simplicity of a link-based file sender with a sharing-key receiving system:

1. **Send:** choose files, pick an expiry, and get a unique key like `VET-K7MQ-3RPX-9DWT`.
2. **Share:** pass the key (or a link) to the people you choose.
3. **Receive:** they enter the key, see the file list, and download.

## Features

### Working now (frontend)

- Responsive, accessible interface with a dark navy theme, light animations, and reduced-motion support
- **Homepage** with a clear explanation of who it's for, how it works, and an FAQ
- **Send page:**
  - Pick from photos and videos, or from files (on a phone this includes iCloud, Google Drive and other file apps connected to the Files app)
  - Drag and drop on computers
  - File list with name, type and size, remove buttons, and totals
  - Checks for empty files, duplicates, and size and count limits
  - Optional message, expiry (24 hours, 3 days, 7 days) and password
  - Terms acceptance before sending
  - Safe handling of unusual file names
- **Receive page:** key entry that cleans up pasted keys and prefills from a shared link
- **Report abuse** page with a form for reports and copyright complaints
- **Terms of Service, Acceptable Use Policy and Privacy Policy** (drafts, see [Legal](#legal))
- Favicon set, social sharing preview image and web manifest

### Planned (backend, not built yet)

| Area | Plan |
|---|---|
| Transfer creation | Server-generated, cryptographically random sharing keys; only a keyed digest is stored |
| File storage | Private Cloudflare R2 buckets, direct uploads with short-lived signed URLs |
| Large files | Chunked, resumable uploads and downloads with integrity checks |
| Access control | Key verification, optional password, rate limiting and lockouts, human verification (Cloudflare Turnstile) |
| Accounts | Google sign-in (Firebase), plus limited guest sessions |
| Expiry | Automatic deletion of expired files, manual revoke, cleanup of unfinished uploads |
| Dashboard | Transfer list, copy key or link, revoke, delete |
| Admin | Abuse report review, restrictions, audit logs |
| Google Drive | Pick-from-Drive button (code written, needs your own Google keys, untested) |

## Project structure

```
index.html            Homepage
send.html             Send files page
receive.html          Receive files page
terms.html            Terms of Service (draft)
aup.html              Acceptable Use Policy (draft)
privacy.html          Privacy Policy (draft)
report.html           Report abuse / copyright complaints
site.webmanifest      Web app manifest
robots.txt            Crawler rules
og-image.png          Social sharing preview (1200x630)
favicon.*, icon-*.png, apple-touch-icon.png   Icons
```

Each page is a single self-contained HTML file (styles and scripts included). There is no build step.

## Run it locally

```bash
git clone https://github.com/devtiwari-bit/ved-transfer-.git
cd ved-transfer-
python3 -m http.server 8000
```

Then open http://localhost:8000. You can also open `index.html` directly in a browser, though links between pages work best from a local server.

## Configuration

Settings live at the top of each page's script. **Never put secrets in this repository.**

| Setting | File | Purpose |
|---|---|---|
| `MAX_FILES`, `MAX_FILE`, `MAX_TOTAL` | `send.html` | Placeholder limits shown to users. The server must enforce the real limits. |
| `API_BASE` | `send.html`, `receive.html`, `report.html` | URL of the backend API. Empty means "not connected". |
| `GOOGLE.CLIENT_ID`, `GOOGLE.API_KEY` | `send.html` | Enables the Google Drive picker. Create these in Google Cloud Console. |

Site URLs in the page tags currently point to the GitHub Pages address. When you add your own domain, replace `https://devtiwari-bit.github.io/ved-transfer-` in the HTML files.

## Security and privacy notes

- **A sharing key is a bearer credential.** Anyone who has it can open the transfer (plus the password, if one is set). Share keys only with intended recipients.
- **Not end-to-end encrypted.** Files will be encrypted in transit and at rest by the storage provider, but the service operator and its providers can technically access stored files.
- **No malware scanning** is performed at this time. Only open files from people you trust.
- Design rules for the backend: private storage only, no permanent public file URLs, server-side authorization on every protected action, no secrets in frontend code, and no passwords or full keys in logs.
- The Send, Receive and Report pages never pretend to succeed. Until the backend exists they say clearly that nothing was sent.

To report a security problem, email **devtiwari007dt@gmail.com**. Please don't open a public issue for vulnerabilities.

## Roadmap

- [x] Homepage, Send, Receive, Report and legal pages
- [x] Logo, favicons, social preview
- [ ] Cloudflare Worker API (transfer creation, key verification)
- [ ] R2 private storage with signed uploads and downloads
- [ ] D1 database for transfer metadata, expiry and security events
- [ ] Human verification (Turnstile) and rate limiting
- [ ] Resumable large-file uploads and downloads
- [ ] Google sign-in and guest sessions
- [ ] Automatic expiry and cleanup jobs
- [ ] User dashboard
- [ ] Admin and abuse management
- [ ] Automated tests for keys, permissions, expiry and abuse controls
- [ ] Custom domain and production deployment

## Legal

The Terms, Acceptable Use Policy and Privacy Policy are **drafts** written as a starting point for the planned service. They are not legal advice. Before a public launch they should be reviewed by a qualified lawyer, including against Indian IT Act, intermediary rules and Digital Personal Data Protection Act, 2023 requirements, and updated to match what the service actually does.

## Contact

Ved Tools, New Delhi, India
Email: devtiwari007dt@gmail.com

---

Made by **Ved Tools** with love ♥

