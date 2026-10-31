# Privacy Policy

**App:** ConfigLedger ("the App")
**Vendor:** Lensdelta, formerly PGMZ ("we", "us")
**Contact:** see "Contact" below
**Effective date:** 7 October 2026 (contact details last updated 30 October 2026)

### 1. What the App is
The App is an Atlassian Forge app for Jira Cloud that keeps a journal of configuration changes (fields, screens, workflows, schemes, filters, permission schemes) so administrators can see what changed, who changed it and what it affects.

### 2. What data the App accesses
Using the Jira REST API with the scopes listed in the app manifest, the App reads Jira **configuration**: field, screen, workflow and scheme definitions, filters (including their JQL), permission schemes, project names and keys, and the Jira audit log. It also receives Jira configuration events.

The App **does not read issue content**: no summaries, descriptions, comments or attachments. Issue-level views only read the issue key and the configuration that applies to it.

### 3. What data the App stores
Everything is stored in Forge hosted storage (Forge SQL) inside Atlassian's infrastructure, in the same data-residency region as your Jira site:
- the structural state of each configuration object and the differences between states;
- for each change: the Atlassian account ID and display name of the person who made it, as reported by your Jira audit log or Jira events, plus the time, the audit summary and the changed values;
- review marks (who reviewed a change and when) and optional free-text change notes entered by users;
- app settings, notification delivery records and operational job records.

We do not store IP addresses from the audit log, user email addresses or project lead data.

### 4. Why we process it
Only to provide the App's features: change journal, attribution, impact analysis, baseline drift, notifications and reports. We do not use it for advertising, profiling or sale.

### 5. Where data goes
- **No egress.** The App makes no calls to servers outside Atlassian. No analytics, external logging or telemetry.
- **No third parties.** We do not share, sell or transfer your data. The vendor does not have access to your site's stored data.
- **Notifications.** Emails and alerts are sent through Jira's own notification API; recipients are users of your own site.
- **Atlassian Intelligence / Rovo.** If an administrator enables the AI summary (off by default), change descriptions are sent to the Claude model hosted by Atlassian through Forge LLMs. The Rovo agent, if used, reads the journal inside Atlassian. In both cases processing stays under Atlassian's terms.

### 6. Retention and deletion
- Journal, snapshots, audit evidence, events and notification records are kept for the period the administrator configures (default 90 days) and then deleted automatically every day. Operational records are kept 30 days.
- When the App is uninstalled, a cleanup handler deletes the App's stored data in batches, and Atlassian's platform removes whatever remains.
- The App reports the account IDs it stores to the Atlassian Personal Data Reporting API and, when a user's account is closed or changed, anonymizes that user's name and ID in the journal while keeping the change entry.

### 7. Your rights
Because the App acts as a processor for your organization's Jira site, requests from individuals (access, correction, erasure) should go to the site administrator. We will help administrators fulfil them. See "Contact" below.

### 8. Security
Data is protected by Atlassian's Forge platform (encryption in transit and at rest, tenant isolation). The App only requests the scopes it needs, performs read operations on Jira configuration (its only write is sending notification emails/alerts), and restricts the admin console to Jira administrators. Vulnerabilities: see our [Security Policy](security.html).

### 9. Changes
We will post changes to this policy at https://lensdelta.com/configledger/privacy.html and update the effective date.

### 10. Contact
Lensdelta
- Privacy requests: privacy@lensdelta.com
- Support: support@lensdelta.com
- Security reports: security@lensdelta.com
