# Mediroza Hospital — Web Application Penetration Testing

## 📌 Project Overview

This project presents an authorized web application penetration testing assessment conducted against the **Mediroza General Hospital** web application.

The objective of the assessment was to identify security weaknesses that could potentially affect authentication, authorization, sensitive information, patient reports, backup files, and database confidentiality.

The assessment followed a structured penetration testing methodology covering reconnaissance, vulnerability identification, exploitation in an authorized environment, evidence collection, impact assessment, and remediation recommendations.

> **Disclaimer:** This project was performed for authorized cybersecurity training and educational purposes within the provided penetration-testing environment. No unauthorized systems or third-party infrastructure were targeted.

---

## 🎯 Objectives

The primary objectives of this penetration testing project were:

- Identify vulnerabilities in the hospital web application.
- Assess the security of the login functionality.
- Test for username enumeration.
- Test authentication mechanisms for SQL injection vulnerabilities.
- Assess access controls protecting patient reports.
- Examine the security of encrypted PDF documents.
- Identify sensitive information exposed through PDF metadata.
- Search for forgotten or publicly accessible backup directories.
- Assess database backup exposure.
- Identify sensitive staff and shareholder information stored in plain text.
- Provide practical remediation recommendations.

---

## 🏥 Target

**Target Application:** Mediroza General Hospital

**Target URL:**

https://medirozahospital.com

**Application Areas Tested:**

- Login functionality
- Patient portal
- Patient reports
- PDF documents
- Backup directories
- Database backup files
- Publicly accessible resources

---

## 🔐 Testing Scope

The assessment focused on the following security areas:

1. Authentication Security
2. Username Enumeration
3. SQL Injection
4. Access Control
5. Sensitive File Exposure
6. PDF Security
7. Metadata Leakage
8. Directory Listing
9. Backup File Exposure
10. Sensitive Database Information
11. Confidentiality of Staff Information
12. Confidentiality of Shareholder Information

---

# 🧪 Penetration Testing Methodology

The assessment was performed using a structured penetration testing workflow.

### Phase 1 — Reconnaissance

The first stage involved gathering information about the target application and identifying publicly accessible resources.

Activities included:

- Identifying the target application.
- Examining accessible web pages.
- Identifying login functionality.
- Discovering accessible directories and files.
- Identifying potentially sensitive resources.

The purpose of reconnaissance was to understand the application's attack surface before performing further testing.

---

### Phase 2 — Authentication Testing

The login page was examined to determine whether it properly handled invalid usernames and passwords.

The assessment identified a **username enumeration vulnerability**, where the application's responses could reveal whether a particular username existed.

This information could help an attacker identify valid accounts before attempting further attacks.

---

### Phase 3 — SQL Injection Testing

The login functionality was tested for SQL injection.

The assessment identified an SQL injection vulnerability that could potentially allow authentication bypass.

This was classified as a **Critical** vulnerability because authentication controls could potentially be bypassed.

The root cause was the unsafe handling of user-controlled input in SQL queries.

---

### Phase 4 — Access Control Testing

After identifying the authentication weakness, access controls protecting sensitive patient reports were assessed.

The testing showed that encrypted PDF reports could become accessible after authentication controls were bypassed.

This demonstrates why authentication and authorization must be enforced independently for sensitive resources.

---

### Phase 5 — PDF Security Assessment

Patient PDF reports were examined to determine whether their protection mechanisms were sufficiently strong.

The assessment identified:

- Weak PDF passwords.
- Potentially guessable passwords.
- Sensitive metadata remaining inside PDF documents.

These issues can expose information even when a document appears to be protected.

---

### Phase 6 — Directory and Backup Testing

The application was examined for forgotten directories and backup files.

An old directory was discovered with directory listing enabled.

The directory exposed an old database backup:

`old/mediroza_db_backup_2019.sql`

This was a serious security issue because database backups should never be directly accessible through a public web directory.

---

### Phase 7 — Database Analysis

The exposed SQL backup was examined in the authorized testing environment.

The database contained tables including:

- `staff`
- `shareholders`

The `staff` table contained information such as:

- Employee names
- Job titles
- Departments
- Email addresses
- Phone numbers
- National identification information
- Salary information
- Joining dates

The `shareholders` table contained:

- Shareholder names
- Share percentages
- Number of shares
- Share classes

This demonstrated the potential impact of publicly exposing database backups.

---

# 🔎 Key Findings

| # | Vulnerability | Location | Risk |
|---|---|---|---|
| 1 | Username Enumeration | `patient/login.php` | Medium |
| 2 | SQL Injection / Login Bypass | `patient/login.php` | Critical |
| 3 | Encrypted PDFs Accessible After Login Bypass | `patient/reports/` | High |
| 4 | Weak PDF Passwords | `patient_report_*.pdf` | High |
| 5 | Sensitive PDF Metadata | `patient_report_3.pdf` | Medium |
| 6 | Directory Listing / Forgotten Backup Folder | `old/` | Critical |
| 7 | Confidential Database Backup Exposure | `old/mediroza_db_backup_2019.sql` | Critical |

---

# 🔴 Finding 1 — Username Enumeration

**Risk:** Medium

**Location:**

`patient/login.php`

### Description

The login page provides distinguishable responses that can reveal whether a username exists.

An attacker could use this behavior to determine valid usernames before attempting password attacks.

### Impact

Username enumeration can:

- Reveal valid accounts.
- Reduce the attacker's guessing effort.
- Assist password attacks.
- Provide useful information for further attacks.

### Recommendation

The application should return the same generic error message for invalid usernames and invalid passwords.

For example:

```text
Invalid username or password.
