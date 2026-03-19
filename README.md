# KFC GES Portal — Security Bug Report

**Reported By:** Aarsh Chauhan  
**Date Observed:** 2025–2026  
**Organization:** KFC — Sapphire Foods India, Nagpur  
**Portal:** GES (Guest Experience System) Management Portal  
**Document Version:** v1.0  

---

## Bug ID: KFC-GES-001

## Title
Managers can generate unlimited fake customer review links via URL parameter manipulation

---

## Environment
- **Application:** KFC GES Management Portal
- **Access Level:** Manager login
- **Browser:** Google Chrome
- **Type:** Web-based internal management portal

---

## Severity & Priority
| Field | Value |
|---|---|
| **Severity** | Critical |
| **Priority** | High |
| **Bug Type** | Security + Business Logic |

---

## Description
A security vulnerability was discovered in the KFC GES (Guest Experience System) management portal used by restaurant managers at Sapphire Foods India.

The portal allows managers to generate customer review links for collecting genuine feedback. However, by manipulating URL parameters directly in the browser address bar, a manager can generate unlimited fake review links without any server-side validation blocking the request.

This allows artificial inflation of restaurant ratings, directly compromising the integrity of the customer feedback system.

---

## Preconditions
- User must be logged in as a Manager
- Must have access to the review link generation feature in GES portal

---

## Steps to Reproduce
1. Log in to the KFC GES Management Portal as a Manager
2. Navigate to the customer review link generation section
3. Generate a legitimate review link and observe the URL structure
4. Copy the URL and manually alter the random text/parameter in the browser address bar
5. Open the altered URL in a new browser tab
6. Observe that a new valid review link is generated

---

## Expected Result
The system should:
- Validate the authenticity of the request server-side
- Reject any manually manipulated URLs
- Restrict review link generation to the authorized flow only
- Return an error or redirect for tampered URLs

---

## Actual Result
- System accepts the manipulated URL without any validation
- A new valid customer review link is generated successfully
- This process can be repeated unlimited times
- Each generated link can be used to submit fake positive reviews

---

## Business Impact
| Impact Area | Description |
|---|---|
| **Rating Integrity** | Restaurant ratings can be artificially inflated |
| **Customer Trust** | Fake reviews mislead genuine customers |
| **Platform Compliance** | Violates Zomato/Swiggy review authenticity policies |
| **Brand Risk** | If discovered publicly, severe reputational damage |

---

## Root Cause Analysis
The vulnerability exists due to missing server-side access control validation on the review link generation endpoint. The system relies only on the client-side URL structure without verifying:
- Whether the request is coming from an authorized generation flow
- Rate limiting on how many links can be generated per session
- Token-based validation to prevent URL tampering

---

## Recommended Fix
1. Implement server-side token validation for every review link generation request
2. Add rate limiting — maximum N links per manager per day
3. Log all link generation requests with manager ID and timestamp
4. Invalidate URLs that do not match the authorized generation pattern
5. Implement CAPTCHA or session-bound tokens for review link generation

---

## Classification
| Field | Value |
|---|---|
| **Testing Type** | Security Testing + Functional Testing |
| **STLC Phase Failed** | Test Case Design + Test Execution |
| **OWASP Category** | Broken Access Control (A01:2021) |

---

## Discovered By
Aarsh Chauhan — Team Member, KFC Sapphire Foods India, Nagpur  
GitHub: github.com/aarshnomorecool
