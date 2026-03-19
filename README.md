# KFC GES Portal - Security Bug Report

**Bug ID:** KFC-GES-001  
**Reported By:** Aarsh Chauhan  
**Date Observed:** Jan 2026  
**Organization:** KFC, Sapphire Foods India, Nagpur  
**Portal:** GES (Guest Experience System) Management Portal  

---

## Title
Managers can generate unlimited fake customer review links by altering URL parameters in the browser

---

## Environment
- **Application:** KFC GES Management Portal
- **Access Level:** Manager login
- **Browser:** Google Chrome
- **Type:** Web-based internal management portal

---

## Severity and Priority

| Field | Value |
|---|---|
| Severity | Critical |
| Priority | High |
| Bug Type | Security + Business Logic |

---

## What I Found

While working as a team member at KFC, I noticed something off with how the GES portal generates customer review links. The portal is meant to let managers create one-time review links to share with customers for genuine feedback.

I noticed that the link had a random text string in the URL. Out of curiosity, I copied the URL, changed that random text manually in the browser, and opened it in a new tab. It worked. A fresh valid review link was generated.

This means any manager can keep generating unlimited review links just by tweaking the URL, with no restriction or validation from the server. Those links can then be used to post fake positive reviews and inflate the restaurant rating.

---

## Preconditions
- User is logged in as a Manager on the GES portal
- User has access to the review link generation section

---

## Steps to Reproduce
1. Log in to the KFC GES portal as a Manager
2. Go to the customer review link generation section
3. Generate a review link and look at the URL structure
4. Copy the URL and change the random text part manually in the address bar
5. Open the modified URL in a new browser tab
6. A new valid review link is generated without any error

---

## Expected Result
The system should validate that the review link request is coming from the proper authorized flow. Any manually modified URL should be rejected with an error. The server should not generate a link just because the URL looks similar.

---

## Actual Result
The server accepts the modified URL without any checks. A valid new review link is generated every single time. This can be repeated as many times as needed, with no rate limit or block in place.

---

## Business Impact

| Area | Impact |
|---|---|
| Rating Integrity | Ratings can be inflated artificially |
| Customer Trust | Fake reviews mislead real customers |
| Platform Compliance | Goes against Zomato/Swiggy review policies |
| Brand Risk | Serious reputation damage if discovered publicly |

---

## Why This Happens

The server is not validating whether the review link request came from a proper in-app action. It is trusting the URL as-is. There is no token, no session check, and no rate limit on how many links a manager can generate. The server basically does whatever the URL tells it to do.

---

## How to Fix It
1. Add server-side token validation so only legitimate in-app requests can generate links
2. Set a daily limit on how many review links a manager can generate
3. Log every link generation with manager ID and timestamp for audit purposes
4. Make sure modified or repeated URLs return an error instead of a new link

---

## Bug Classification

| Field | Value |
|---|---|
| Testing Type | Security Testing + Functional Testing |
| STLC Phase Missed | Test Case Design + Test Execution |
| OWASP Category | Broken Access Control (A01:2021) |

---

## Discovered By
Aarsh Chauhan, Team Member at KFC Sapphire Foods India, Nagpur  
GitHub: github.com/aarshnomorecool
