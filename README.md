# Penetration Test Report — Maisie Web Application 🔍

> Black-box manual penetration test conducted on a vulnerable PHP/MySQL web application in a controlled lab environment.

---

## Overview

This project involved performing a full manual black-box penetration test on the Maisie web application, hosted at `http://172.16.252.239` in a university cybersecurity lab. The test was conducted without any knowledge of the application's source code or internal structure, simulating how a real external attacker would approach the target.

No automated vulnerability scanners were used — all findings were discovered and exploited manually, in line with ethical hacking methodologies and the OWASP Top 10 framework.

---

## Vulnerabilities Discovered

### 🔴 Critical
- **SQL Injection** — Extracted 155 user records including admin credentials using UNION-based payloads. No input sanitisation or parameterised queries in place.
- **Cross-Site Scripting (XSS)** — Injected an obfuscated SVG payload into the `img` URL parameter on `friends.php`, successfully executing JavaScript in the browser.

### 🟠 High
- **Brute Force — Hidden Admin Page** — Discovered `/maisies_stash.php` via `robots.txt`. Built a custom brute-force script to gain access using credentials extracted from the SQL injection.
- **HTTP (No Encryption)** — All traffic transmitted over unencrypted HTTP, exposing credentials and session data to interception.
- **Cookie Poisoning / Session Hijacking** — Session cookies transmitted in plain text with no `Secure` or `HttpOnly` flags. Successfully hijacked a user session by copying and injecting another user's cookie.
- **Missing Login Rate Limiting** — No lockout, CAPTCHA, or delay after repeated failed login attempts, enabling brute-force attacks.

### 🟡 Medium
- **Verbose Error Messages** — 404 errors expose Apache version, OS, and internal IP address.
- **Weak Password Policy** — Application accepts single-character passwords with no complexity requirements.
- **No Two-Factor Authentication (2FA)** — All accounts protected by password only, no secondary verification available.
- **No Password/Username Change Functionality** — Users cannot update credentials after account creation.

### ℹ️ Info
- **Missing Terms & Conditions** — No privacy policy or user agreement present anywhere on the site.

---

## Methodology

- **Approach:** Black-box, manual testing only
- **Framework:** OWASP Top 10
- **Severity Scoring:** CVSS v3.1
- **Tools used:** Burp Suite (intercept only), browser developer tools, custom Python brute-force script

### Testing techniques
- SQL payload injection into search and login fields
- URL parameter tampering for XSS
- Custom brute-force script against Basic Auth
- Cookie capture and manual session injection
- HTTP header and error message inspection

---

## Key Findings Summary

| Severity | Count |
|----------|-------|
| Critical | 2 |
| High | 4 |
| Medium | 4 |
| Info | 1 |

---

## Files

- `Penetration_Test_Report_Safa.pdf` — Full technical report including CVSS scores, proof of exploitation, and remediation recommendations for each finding

---

## Scope & Constraints

- Testing restricted to internal lab environment (University of Roehampton, DB117)
- No automated scanners permitted
- No DoS attacks permitted
- Admin account credentials not provided — access gained through SQL injection
- No remote access — all testing conducted on-site

---

## Disclaimer

This penetration test was conducted in a controlled academic lab environment with explicit authorisation. All findings were reported responsibly. This project is shared for educational and portfolio purposes only.
