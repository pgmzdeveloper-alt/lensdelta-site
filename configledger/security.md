# Security Policy

**App:** ConfigLedger ("the App")
**Vendor:** Lensdelta ("we", "us")
**Security contact:** security@lensdelta.com
**Effective date:** 29 October 2026

This policy describes how ConfigLedger, an Atlassian Forge app for Jira Cloud, protects customer data, how we manage vulnerabilities and how we respond to security incidents. It complements our [Privacy Policy](privacy.html).

### 1. Scope and architecture
ConfigLedger is built on Atlassian Forge and runs on Atlassian's infrastructure. It keeps a journal of Jira configuration changes (fields, screens, workflows, schemes, filters, permission schemes) for Jira administrators.

- **No egress.** The App declares no external network permissions and makes no calls to servers outside Atlassian. No data leaves Atlassian's infrastructure.
- **No vendor access to customer data.** Data is stored in Forge hosted storage (Forge SQL) in the same data-residency region as the customer's Jira site. We do not have access to it.
- **No credentials.** The App does not ask for, store or process passwords, Personal Access Tokens or other shared secrets.

### 2. Security controls
**Platform.** We rely on Atlassian Forge for encryption in transit and at rest, tenant isolation and runtime isolation of functions.

**Least privilege.** The App requests only the Forge scopes it needs, and each scope has a documented justification. The App's code performs read operations on Jira configuration; its only write operations are delivering notifications (creating the alert issue and sending the notification email through Jira's own API).

**Authentication and authorization.** The identity of the caller is supplied by the Forge platform, never by the browser. Every request from the user interface is checked on the server: the request context is validated and, for the administration console, the caller must hold the Jira "Administer Jira" permission, verified against Jira with the caller's own identity. The user interface is never trusted for authorization.

**Input validation and injection.** Every payload from the user interface is validated against strict allow-lists (types, patterns, lengths and permitted values). Database access uses parameterized queries only; column and sort names come from fixed allow-lists. Jira API paths are built with Forge's route builder.

**Error handling and logging.** Errors returned to the browser are generic unless explicitly marked as safe to show. Application logs redact email addresses, account identifiers and tokens, omit stack traces, and never record SQL statements or their parameters.

**Front end.** The user interface contains no secrets and no authorization logic. Production builds ship without source maps, and the code base forbids unsafe HTML insertion, `eval` and browser storage of data.

**Supply chain.** Dependencies are installed under a strict policy: only versions published at least seven days earlier, protection against packages that lose their publishing provenance, installation scripts denied by default, and a frozen lockfile.

**Verification before release.** Each release goes through automated checks: type checking, linting, unit and security tests, a standards check (no external egress, only approved scopes, no unsafe code patterns), Forge lint and a dependency audit.

### 3. Vulnerability management
**Reporting.** Please report suspected vulnerabilities to security@lensdelta.com with reproduction steps, impact and the app version. Please do not open public issues with vulnerability details and give us a reasonable time to fix the issue before disclosing it.

**Handling.** We acknowledge reports within 2 business days and assess them within 7 days. We target fixes for high and critical severity issues within 30 days, and we follow Atlassian's Marketplace security requirements and Security Bug Fix Policy where they set shorter timelines.

**Dependencies.** We review dependency advisories, prioritise them by severity and by whether the affected code runs in the deployed App, and update affected packages. Fixes are released as app versions through Forge.

**Cooperation with Atlassian.** We cooperate with the Atlassian Marketplace security team and handle vulnerabilities raised through Atlassian's processes.

### 4. Incident response
If we detect or are notified of a security incident affecting the App:

1. **Triage and contain.** Confirm the incident, determine its scope and contain it (for example by deploying a fix or disabling the affected feature).
2. **Assess impact.** Determine which installations and which data are affected. Because customer data is held by Atlassian and not by us, our assessment relies on the App's code, Forge logs and information from Atlassian.
3. **Notify.** We notify Atlassian and affected customers without undue delay, through the Marketplace contact channels, with what happened, what is affected and what customers should do.
4. **Remediate.** Release a fixed version and verify the fix.
5. **Review.** Document the root cause and the changes made to prevent recurrence.

### 5. Data retention and deletion
Retention is configurable by the customer's administrator (default 90 days) and expired data is deleted automatically. When the App is uninstalled, a lifecycle handler deletes the App's stored data and Atlassian's platform removes the remainder. Details are in our [Privacy Policy](privacy.html).

### 6. Compliance and programs
The App currently holds no third-party security certifications and does not participate in the Marketplace Bug Bounty program. It is built to run entirely on Atlassian Forge.

### 7. Changes
We will publish changes to this policy at this address and update the effective date.

### 8. Contact
Lensdelta, security reports and questions: security@lensdelta.com
