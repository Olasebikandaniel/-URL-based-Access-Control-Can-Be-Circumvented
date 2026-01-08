 Lab Write-Up: URL-based Access Control Can Be Circumvented

## Overview
Back at it with another classic from PortSwigger's Web Security Academy. This lab shows how fragile URL-based access controls are when devs forget that paths are just strings—easily tweaked by anyone with a proxy. In the spirit of timeless wisdom: never trust the client to enforce security. Date crushed: January 8, 2026.

The app hides the admin panel behind an "obscure" URL and assumes low-privilege users won't guess it. Spoiler: we do. One tiny path change → full admin access → delete the target user `carlos`. Poetic, isn't it? Forward-thinking defense demands server-side checks, not hide-and-seek.

**Lab Link:** [Web Security Academy - URL-based Access Control](https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented)

## Tools Used
- Burp Suite Professional (Repeater + Proxy = GOAT combo)
- Kali Linux VM (traditional setup, modern results)
- Browser for recon

## Steps to Solve
1. **Recon Phase:** Logged in as low-privilege user `wiener:peter`. Explored the site—nothing admin-looking in the UI.

2. **Spot the Pattern:** Noticed URLs follow `/admin-panel` style but it's hidden. Hypothesis: maybe there's a literal `/admin` or `/administrator` path?

3. **Burp Intercept:** Proxed traffic and tried manually navigating to potential admin paths. Key moment—changed a normal request URL from something innocuous to `/admin` → server happily served the admin panel!

4. **Escalate & Pwn:** Once in `/admin`, browsed to `/admin/delete?username=carlos` → target user gone → lab solved banner drops.

   Pro move: Use Burp Repeater to test multiple paths quickly (`/admin`, `/administrator`, `/admin-panel`, etc.).

## Key Request Example (Captured in Burp)
Server responds with full admin interface instead of 404/403. Classic failure to enforce access at the backend.

## What I Learned
- URL obscurity ≠ security. It's security theater.
- Real access control must happen server-side—check user privileges on every sensitive endpoint.
- Strong opinion: If you're building apps in 2026, implement proper RBAC + authorization checks. Don't make me come tamper with your paths 😏
- Practical tip: Tools like Burp's "Match and Replace" rules can automate path testing in bigger apps

## Final Thoughts
Quick lab, deep lesson. These "old-school" vulns are still everywhere because devs keep repeating history. Stay skeptical, verify everything, and keep grinding these labs. Your future pentesting self is proud.

Got feedback or wanna collab on more write-ups? Open an issue—let's talk shop
