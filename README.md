# R&O v0.4.0 Alpha Test Tracker

A lightweight web app for coordinating structured alpha testing of [Requests & Offers](https://github.com/happenings-community/requests-and-offers) — a peer-to-peer resource sharing application built on [Holochain](https://holochain.org).

## What It Does

This tool guides testers through 57 steps across 12 test areas, collecting structured feedback that feeds directly into our development workflow.

**For testers:**
- Step-by-step test scenarios with clear instructions and expected outcomes
- One-click result recording (Pass / Fail / Partial / Skip)
- Screenshot upload for visual evidence
- Direct GitHub issue creation for bugs and problems

**For developers:**
- Live results dashboard in Google Sheets
- Screenshots organised by tester and step in Google Drive
- Failed/partial tests automatically filed as GitHub issues with full context
- Summary view showing pass/fail rates across all test areas

## Test Areas

| # | Area | Steps | Description |
|---|------|-------|-------------|
| 1 | Installation & First Launch | 4 | Download, install, launch, and connection status |
| 2 | Profile Creation | 5 | Create profile with markdown support |
| 3 | Profile Editing | 4 | Edit and update existing profile |
| 4 | Creating a Request | 6 | Create and verify a resource request |
| 5 | Creating an Offer | 6 | Create and verify a service offer |
| 6 | Archive & Reactivate | 4 | Lifecycle management of listings |
| 7 | Search | 5 | Search across requests, offers, users, orgs |
| 8 | Peer Discovery | 8 | **Key test** — two-person discovery and sync |
| 9 | Organizations | 5 | Create and manage organisations |
| 10 | Service Types | 3 | Browse and assign categories |
| 11 | Navigation & UI | 4 | Menu, responsiveness, accessibility |
| 12 | Network Resilience | 3 | Offline behaviour and reconnection |

## Architecture

```
┌─────────────────────────┐
│   GitHub Pages (UI)     │  ← Testers interact here
│   index.html            │
└──────────┬──────────────┘
           │ HTTPS POST
           ▼
┌─────────────────────────┐
│   Google Apps Script    │  ← API backend
│   (Web App)             │
└──────┬─────┬────────┬───┘
       │     │        │
       ▼     ▼        ▼
┌────────┐ ┌──────┐ ┌────────┐
│ Sheets │ │Drive │ │GitHub  │
│ (data) │ │(imgs)│ │(issues)│
└────────┘ └──────┘ └────────┘
```

- **Frontend:** Single HTML file served via GitHub Pages — no build step, no dependencies
- **Backend:** Google Apps Script handles registration, result saving, screenshot uploads, and GitHub issue creation
- **Data:** Google Sheets stores tester info, test steps, and results with a live summary dashboard
- **Screenshots:** Uploaded to a shared Google Drive folder, linked back to the relevant test step
- **Issues:** Failed/partial steps can be reported directly to the [main repo](https://github.com/happenings-community/requests-and-offers/issues) with full context

## For Testers

1. Go to **[happenings-community.github.io/alpha-testing](https://happenings-community.github.io/alpha-testing/)**
2. Register with your name and platform details
3. Work through the test scenarios at your own pace
4. Record results and observations as you go
5. Upload screenshots for anything notable
6. Use **🐛 Report Issue** for failures — this creates a GitHub issue automatically

Your progress saves automatically. Use **Returning Tester** to pick up where you left off.

## For Maintainers

### Setup Requirements

The backend requires a Google Apps Script deployment with access to:

- **Google Sheets** — stores all test data
- **Google Drive** — stores uploaded screenshots
- **GitHub API** — creates issues from failed tests

Configuration is in the Apps Script file (lines 7–10):

```javascript
const SPREADSHEET_ID = '...';
const SCREENSHOTS_FOLDER_ID = '...';
const GITHUB_TOKEN = '...';
const GITHUB_REPO = 'happenings-community/requests-and-offers';
```

### Updating Test Scenarios

Test steps are defined in the **Templates** tab of the Google Sheet. Each row has:

| Column | Content |
|--------|---------|
| A | Step ID (e.g. `1.1`) |
| B | Test Area name |
| C | Step instructions (pipe `\|` delimited for bullet formatting) |
| D | Expected outcomes (pipe `\|` delimited) |

To update: edit the Templates tab, then have testers re-register to pick up changes.

### Updating the Frontend

Edit `index.html` in this repo. Changes deploy automatically via GitHub Pages within a few minutes.

## Related

- [Requests & Offers](https://github.com/happenings-community/requests-and-offers) — the application being tested
- [Install Guide](https://github.com/happenings-community/requests-and-offers/releases/latest) — download and installation instructions
- [Homebrew Tap](https://github.com/happenings-community/homebrew-requests-and-offers) — macOS Homebrew installation

## License

Internal testing tool for the hAppenings community. Not intended for redistribution.
