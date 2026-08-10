# Android TV (users)

Phone layout: [../user/guide.md](../user/guide.md). This page is what differs on TV.

## Navigation

Leanback-style shell (D-pad / remote):

| Area | Role |
|------|------|
| **Servers** | List, add, open a server, start/stop, QR |
| **Settings** | App-wide options (theme, language, permissions, …) |
| **About** | App info |
| **Exit** | Leave the app |

Open a server for its settings, users, logs, and certificates as focused panels/overlays.

## Same as phone

- Multi-server fleet, ports, folders/users, SSL library, autostart, logging  
- Free caps (2 servers / 2 users)  
- Connect / QR to get the address onto another device  

## Different from phone

| Phone | TV |
|-------|-----|
| Tabs Home / Settings / Status / Logs | Panels and overlays; **no Status metrics tab** like on phone |
| Bell inbox for notices | Mostly **blocking** announcement dialogs when needed |
| Touch cards + FAB | D-pad focus; long-press / Menu for rename/delete |
| Ads / rewarded flows | Free flavor may exist; ad UX is phone-oriented |

## Tips

- Grant storage (and notifications if you want the ongoing notification) the same as on phone.  
- Prefer a wired/Ethernet or stable Wi‑Fi TV for a LAN server.  
- Battery / “don’t kill background apps” still matters on some TV OEMs.

Developer focus map (screens): keep reading below only if you maintain TV UI.

---

## Screen map (maintainers)

Entry: TV main shell → Servers / Settings / About / Exit.

Servers → list + detail settings; overlays for Users, Logs, Certificates.

Parity checklist when changing phone-only UX: share address, notification reminder, SAF folder pick.
