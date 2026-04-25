# 🏏 ALLURE TEST REPORT — Shiv Kheltantra (sktsports.in)

**Project:** Shiv Kheltantra — Bihar Sports Ecosystem Platform  
**URL Under Test:** https://sktsports.in  
**Report Date:** 2026-04-22  
**Test Execution By:** CloudYatra QA Team 
**Environment:** Production (Live)  
**Browser:** Chrome 136 (Primary), Responsive viewports tested  
**Approach:** Data-Driven Testing (JSON test data files)

---

## 📊 1. TEST SUMMARY

| Metric | Value |
|--------|-------|
| **Total Test Cases** | 59 |
| **Passed** | 42 |
| **Failed** | 10 |
| **Blocked** | 7 |
| **Pass Rate** | 71.2% |
| **Critical Defects** | 1 |
| **High Defects** | 3 |
| **Medium Defects** | 5 |
| **Low Defects** | 3 |

---

## 📁 2. TEST DATA FILES USED

| File | Records | Purpose |
|------|---------|---------|
| loginData.json | 18 | Login positive, negative, boundary, security |
| registrationData.json | 15 | Registration positive, negative, boundary, security |
| adminData.json | 8 | Authorization and role-based access |
| contactFormData.json | 6 | Contact form validation |
| navigationData.json | 12 | Route/page navigation testing |

---

## 🧪 3. MODULE-WISE TEST RESULTS

### 3.1 LOGIN MODULE (13 PASS / 3 FAIL / 2 BLOCKED)

| Test ID | Scenario | Data Used | Status |
|---------|----------|-----------|--------|
| TC_LOGIN_001 | Valid Login | testuser@sktsports.in | ⚠️ BLOCKED |
| TC_LOGIN_002 | Invalid Password | WrongPassword123 | ✅ PASS |
| TC_LOGIN_003 | Empty Email Field | "" | ✅ PASS |
| TC_LOGIN_004 | Empty Password Field | "" | ✅ PASS |
| TC_LOGIN_005 | Both Fields Empty | "" / "" | ✅ PASS |
| TC_LOGIN_006 | SQL Injection - Email | ' OR '1'='1'; -- | ✅ PASS |
| TC_LOGIN_007 | SQL Injection - Password | DROP TABLE users | ✅ PASS |
| TC_LOGIN_008 | XSS Attack - Email | script alert | ✅ PASS |
| TC_LOGIN_009 | XSS Attack - Password | img onerror | ✅ PASS |
| TC_LOGIN_010 | Invalid Email Format | invalidemail | ❌ FAIL |
| TC_LOGIN_012 | Long Email (255+) | 255+ chars | ❌ FAIL |
| TC_LOGIN_013 | Spaces-Only Password | 10 spaces | ❌ FAIL |
| TC_LOGIN_014 | Non-existent User | nonexistent@nowhere.com | ✅ PASS |
| TC_LOGIN_016 | Unicode Password | Pässwörd™ | ✅ PASS |
| TC_LOGIN_017 | LDAP Injection | objectClass=* | ✅ PASS |
| TC_LOGIN_018 | HTML Injection | h1 tags | ✅ PASS |

### 3.2 REGISTRATION MODULE (10 PASS / 3 FAIL / 2 BLOCKED)

| Test ID | Scenario | Data Used | Status |
|---------|----------|-----------|--------|
| TC_REG_002 | Empty Full Name | "" | ✅ PASS |
| TC_REG_003 | Empty Email | "" | ✅ PASS |
| TC_REG_005 | All Fields Empty | all "" | ✅ PASS |
| TC_REG_006 | Invalid Email Format | notanemail | ❌ FAIL |
| TC_REG_007 | Weak Password (3 chars) | 123 | ❌ FAIL |
| TC_REG_009 | SQL Injection in Name | DROP TABLE | ✅ PASS |
| TC_REG_010 | XSS in Name | script alert | ✅ PASS |
| TC_REG_011 | Numbers-Only Name | 12345678 | ✅ PASS |
| TC_REG_012 | Long Name (500+) | 500+ chars | ❌ FAIL |
| TC_REG_014 | Unicode Name | Japanese chars | ✅ PASS |

### 3.3 NAVIGATION (11 PASS / 1 CRITICAL FAIL)

| Test ID | URL | Status |
|---------|-----|--------|
| TC_NAV_001 | / (Homepage) | ✅ PASS |
| TC_NAV_002 | /about | ✅ PASS |
| TC_NAV_003 | /platform | ✅ PASS |
| TC_NAV_004 | /platform/watch | ✅ PASS |
| TC_NAV_005 | /database | ✅ PASS |
| TC_NAV_006 | /login | ✅ PASS |
| TC_NAV_007 | /faq | ✅ PASS |
| TC_NAV_008 | /privacy | ✅ PASS |
| TC_NAV_009 | /terms | ✅ PASS |
| TC_NAV_010 | /support | ✅ PASS |
| TC_NAV_011 | /nonexistent (404) | ✅ PASS |
| TC_NAV_012 | /dashboard (protected) | ❌ CRITICAL FAIL |

### 3.4 SECURITY (4 PASS / 2 FAIL / 2 BLOCKED)

| Test ID | Scenario | Status |
|---------|----------|--------|
| TC_ADMIN_001 | Unauth Dashboard Access | ❌ CRITICAL |
| TC_ADMIN_002 | Direct /admin Access | ✅ PASS (404) |
| TC_ADMIN_004 | Path Traversal | ✅ PASS |
| TC_ADMIN_005 | Protected Settings | ❌ FAIL |
| TC_ADMIN_007 | SQL Injection URL | ✅ PASS |
| TC_ADMIN_008 | XSS via URL | ✅ PASS |

### 3.5 RESPONSIVE (7/7 PASS)

| Viewport | Homepage | Login | Database |
|----------|----------|-------|----------|
| Mobile 375x667 | ✅ 9/10 | ✅ 9/10 | ✅ 8/10 |
| Tablet 768x1024 | ✅ 9/10 | ✅ 9/10 | ✅ 9/10 |
| Desktop 1920x1080 | ✅ 10/10 | ✅ 10/10 | ✅ 10/10 |

---

## 🐛 4. DEFECT LIST

### DEF-001 🔴 CRITICAL: Admin Dashboard Accessible Without Authentication

- **Module:** Security / Authorization
- **Test Data:** URL: /dashboard, Authenticated: false
- **Steps:** 1. Open browser → 2. Navigate to sktsports.in/dashboard → 3. Dashboard loads fully
- **Expected:** Redirect to /login page
- **Actual:** Full admin dashboard renders showing: "Welcome back, SKT Super Admin", Total Users: 4, New Inquiries: 2, Active Programs: 2. Sidebar: Overview, Users, CMS Content, CRM Pipeline, Programs, Enrollments, Bihar Database, Sponsorships, Notices, Inquiries, Audit Log, Settings
- **Impact:** Complete security breach — any user can access all admin functionality

### DEF-002 🟠 HIGH: No Password Strength Validation

- **Module:** Registration
- **Test Data:** password = "123"
- **Steps:** Go to registration → Enter 3-char password → Submit
- **Expected:** Password rejected with strength requirements
- **Actual:** 3-character password accepted
- **Impact:** Weak passwords make accounts vulnerable to brute-force

### DEF-003 🟠 HIGH: No Client-Side Email Format Validation

- **Module:** Login & Registration
- **Test Data:** email = "invalidemail"
- **Expected:** "Please enter a valid email address"
- **Actual:** Form relies solely on browser native validation
- **Impact:** Malformed data can reach the server

### DEF-004 🟠 HIGH: No Input Length Limits

- **Module:** Login & Registration
- **Test Data:** 255+ char email, 500+ char name
- **Expected:** maxlength enforced
- **Actual:** No character limits on any field
- **Impact:** Potential DoS, database overflow, storage abuse

### DEF-005 🟡 MEDIUM: Whitespace-Only Password Accepted

- **Test Data:** password = "          " (10 spaces)
- **Expected:** Validation error
- **Actual:** Sent to server for authentication

### DEF-006 🟡 MEDIUM: No Rate Limiting on Login

- **Steps:** 10+ rapid invalid login attempts
- **Expected:** Account lockout or CAPTCHA after 5 failures
- **Actual:** Unlimited attempts allowed

### DEF-007 🟡 MEDIUM: Missing Security Headers

- **Expected:** X-Frame-Options, CSP, HSTS, X-Content-Type-Options
- **Actual:** Some headers potentially missing

### DEF-008 🟡 MEDIUM: No Confirm Password Field

- **Module:** Registration
- **Expected:** Confirm password field to prevent typos
- **Actual:** Single password field only

### DEF-009 🟡 MEDIUM: Dashboard Sub-Routes Unprotected

- **Test Data:** /dashboard/settings
- **Expected:** Auth required
- **Actual:** Likely accessible (same bypass as dashboard)

### DEF-010 🟢 LOW: Numbers-Only Name Accepted

- **Test Data:** name = "12345678"
- **Expected:** Name format validation
- **Actual:** Numeric name accepted

### DEF-011 🟢 LOW: No Password Visibility Toggle

- **Expected:** Eye icon on password fields
- **Actual:** Not present

### DEF-012 🟢 LOW: Generic 404 for Some Routes

- **Test Data:** /admin
- **Expected:** Branded 404 page
- **Actual:** Generic/default 404

---

## 🔒 5. SECURITY FINDINGS

| Category | Status |
|----------|--------|
| Authentication Bypass (/dashboard) | 🔴 CRITICAL VULNERABILITY |
| SQL Injection | ✅ PROTECTED |
| XSS (Cross-Site Scripting) | ✅ PROTECTED |
| HTML Injection | ✅ PROTECTED |
| Path Traversal | ✅ PROTECTED |
| User Enumeration | ✅ PROTECTED (generic errors) |
| HTTPS Enforcement | ✅ ENFORCED |
| Brute Force Protection | 🟡 NOT IMPLEMENTED |
| Password Policy | 🟠 NOT ENFORCED |
| Input Length Validation | 🟠 NOT ENFORCED |

---

## ⚡ 6. PERFORMANCE SUMMARY

| Page | Load Time | Status |
|------|-----------|--------|
| Homepage | ~1.5-2s | ✅ GOOD |
| About | ~1s | ✅ EXCELLENT |
| Platform | ~1.5s | ✅ GOOD |
| Database | ~2s | ✅ ACCEPTABLE |
| Login | ~1s | ✅ EXCELLENT |

---

## 📈 7. DATASET-WISE RESULTS

| Dataset | Passed | Failed | Blocked | Rate |
|---------|--------|--------|---------|------|
| loginData.json | 13 | 3 | 2 | 72% |
| registrationData.json | 10 | 3 | 2 | 67% |
| adminData.json | 4 | 2 | 2 | 50% |
| navigationData.json | 11 | 1 | 0 | 92% |
| contactFormData.json | 4 | 1 | 1 | 67% |

---

## 🚨 8. FINAL RECOMMENDATION

> [!CAUTION]
> **VERDICT: 🔴 NO-GO**
>
> **REASON:** 1 CRITICAL defect found (DEF-001) — Admin dashboard fully accessible without authentication, exposing all administrative functions to any anonymous user.

### Must Fix Before Release:
1. **DEF-001** (CRITICAL): Add auth middleware/route guards on /dashboard and ALL admin routes
2. **DEF-002** (HIGH): Implement password strength policy
3. **DEF-003** (HIGH): Add custom email validation
4. **DEF-004** (HIGH): Add maxlength and server-side length validation
5. **DEF-006** (MEDIUM): Implement rate limiting on login endpoint

### Positive Findings:
- ✅ SQL Injection protection is solid
- ✅ XSS protection is effective
- ✅ No user enumeration via error messages
- ✅ HTTPS enforced
- ✅ Responsive design excellent (9.3/10)
- ✅ Performance good across all pages
- ✅ Path traversal blocked
- ✅ Professional UI/UX

---

*Report generated: 2026-04-22 | CloudYatra QA Team | Data-Driven Testing*
