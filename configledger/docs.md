# ConfigLedger documentation

**App:** ConfigLedger, an Atlassian Forge app for Jira Cloud
**Vendor:** Lensdelta (https://lensdelta.com/)
**Applies to:** version 2.7.0 and later

ConfigLedger keeps a journal of changes to your Jira configuration: fields, screens, workflows, schemes, permissions, project roles, priorities, resolutions, filters, statuses, work types and spaces. It tells you who made each change and when, scores its risk, shows what it affects and keeps the evidence for audits.

**The app is read-only on Jira.** It never changes, approves, blocks or reverts your configuration. It only stores its own journal and settings, in Atlassian Forge hosted storage, and makes no calls outside Atlassian.

Inside Jira the app appears as **Site Change Journal for Jira**.

## Contents

1. [Getting started](#1-getting-started)
2. [Who sees what](#2-who-sees-what)
3. [Where the app appears in Jira](#3-where-the-app-appears-in-jira)
4. [Change history and review](#4-change-history-and-review)
5. [Impact, drift and state on a date](#5-impact-drift-and-state-on-a-date)
6. [Investigation](#6-investigation)
7. [Observations and acknowledgements](#7-observations-and-acknowledgements)
8. [Change control and evidence](#8-change-control-and-evidence)
9. [Email notifications](#9-email-notifications)
10. [Settings](#10-settings)
11. [Rovo agent and Jira Automation](#11-rovo-agent-and-jira-automation)
12. [FAQ](#12-faq)

---

## 1. Getting started

1. Install ConfigLedger from the [Atlassian Marketplace](https://marketplace.atlassian.com/apps/1314318485/configledger?hosting=cloud&tab=overview). You need the Jira **Administer Jira** permission.
2. Open **Jira settings → Apps → Site Change Journal for Jira**.
3. On the first visit the app takes a snapshot of your configuration. From then on, every scan is compared with the previous one, and whatever differs is recorded as a **change**. Jira events and the Jira audit log tell the app who made it.
4. Scans run on their own (daily or weekly, per source). You can also press **Scan now**.

Key terms:

| Term | Meaning |
|---|---|
| **Snapshot / scan** | A picture of the Jira configuration. Each new scan is compared with the previous one. |
| **Change origin** | **Administered change** (a person is recorded), **No human actor** (no person on record) or **Undetermined** (the audit log could not be read; the app never attributes a change without evidence). |
| **Risk** | A score from 0 to 100 shown as **high**, **medium** or **low**, based on object type, change type, origin, reach and time of day. |
| **Reviewed / pending** | An administrator marks a change as reviewed after checking it. Pending changes form the review queue. |
| **Points** | Jira REST API usage spent by scans. There is a daily budget; when it runs out, scans wait for the next day (UTC). |

## 2. Who sees what

| Role | What they can do |
|---|---|
| **Jira administrator** | Everything: the full console, who made each change, before and after values, review, scan, export and settings. |
| **Project administrator** | Changes that affect their project, with person and values, if the site allows it in **Access control**. Cannot change app settings. |
| **Team member** | **What** changed and **when** in their project, never the person or the values, if the site allows it. |
| **No permission** | A restricted-access message. |

Permissions are always checked on the server, not in the browser.

## 3. Where the app appears in Jira

| Place | Name | For |
|---|---|---|
| Jira settings → Apps | **Site Change Journal for Jira** (the console) | Jira administrators |
| Project settings | **Site Change Journal**: Overview, Changes, Configuration in use, Inbox | Project administrators |
| Project navigation | **Configuration Journal** | The whole team (role-filtered) |
| Issue view | **Site Change Journal** panel, **Site Changes** tab under Activity, **Audit configuration drift** in the ••• menu | Depends on role and site settings |
| Dashboards | **Configuration changes** widget and **Site change journal** gadget | Jira administrators |
| Command palette (Ctrl/Cmd + K) | **What changed in the configuration yesterday?** | Jira administrators |
| Rovo and Jira Automation | **Site Change Journal analyst** agent and Automation actions | Depends on permissions |

The issue modules work in the new Jira issue view.

---

## 4. Change history and review

Open **Change history** in the console. It has three tabs:

- **Change history**: every detected change, newest first. Filter by **Period** (24 hours up to 1 year), **Change origin** and **Object type**. Each card shows the origin, the risk, who made it and when.
- **Review queue**: changes that no administrator has reviewed yet, with a counter.
- **Period summary**: totals by origin, deletions and pending reviews.

**View details** opens the change:

- **Evidence and context**: the real time of the change, how it was attributed (Jira event or audit log), how many projects it affects and which objects use it.
- **Difference**: a table of what was modified, added or removed, with the before and after values. You can also see the full JSON.
- **Review**: add a review comment and, optionally, a change-request issue key, then **Mark reviewed**. The site can make the change request mandatory.
- **How to undo it (manually)**: when the previous value is complete, the app tells you what to set back. It never restores anything itself.
- **Owner and follow**: assign an owner to the object or follow it (see **My objects**).

**Review inbox** in the side menu lists everything that still needs your attention.

## 5. Impact, drift and state on a date

**Change impact**: use it *before* changing a shared object. Pick an object type and an object to see whether it is shared, the affected spaces with their work types and approximate issue counts, and the risk if it is updated or deleted. **View full relationship map** shows a graph and a tree of the schemes and projects that use it.

**Baseline and drift**:

- **Approve current configuration** saves a named baseline of every object.
- The page then lists what was **Changed**, **Added** and **Removed** since that baseline, with the last change, who made it and the risk.
- **Compare with** another baseline to see what changed between two approvals.
- **Adopt this drift** turns the current configuration into a new baseline that records where it came from.
- **Export current configuration (CSV or PDF)** downloads the current inventory without approving a baseline.

**State on a date**: how one object looked at the end of a given day (UTC), rebuilt from the journal. You can also type the ID of a deleted object. What cannot be rebuilt is said, never filled in.

**Project configuration on a date** (project settings → **Configuration in use**): compare how a project was configured on a past date with how it is today.

## 6. Investigation

- **Investigate changes**: answer "who did what, where, when and with which permission". Filter by person, current admin permission, space, object name or ID, change type, review status and date range. Save an investigation as a reusable control that other administrators can run.
- **Stored audit log**: the Jira audit log records that the app keeps (Jira itself keeps only a few months). Filter and **export to CSV** exactly what you filtered.
- **Jira events**: configuration events that Jira sends to the app, with the exact time and who triggered them.
- **Automation audit**: what the Jira audit log says about Automation rules. It states its coverage; it does not read the rules themselves.
- **Detection quality**: how quickly the app learns about changes and the gaps between scans.
- **Field usage**: fields with no recent use, on no screen or with no context. A field that Jira does not report usage for is never marked unused.
- **Project access**: who can see, work in and administer a project, and through which permission. **Sign review** records an access review until the access changes.
- **Configuration health**: Jira limits (workflows per space, statuses per workflow, grants per permission) and orphaned configuration.
- **Governance health**: five indicators measured from the app's own data. Trust the breakdown more than the single score.

## 7. Observations and acknowledgements

An **observation** is a saved watch on configuration changes: which object types, in the whole site or one space, minimum risk, an owner and the people involved.

- **Reviewer**: sees the detail and acknowledges changes.
- **Observer**: sees what changed, not who or the values.
- **Notifications only**: receives digests only.

An **acknowledgement** records that a person knows about a change. It does **not** approve, block or revert anything. The closing rule decides when a change leaves "To review": one acknowledgement, all reviewers, or the owner of that change type. Acknowledgements can be revoked with a reason and annotated; nothing is deleted.

Project administrators find the same inbox in project settings → **Inbox**.

**My objects** lists the objects you own or follow, with their changes in the last 30 days.

## 8. Change control and evidence

Open **Freeze and integrity**:

- **Freeze windows** mark changes made during a period when nothing should change. They never block a change.
- **Unusual activity** alerts on bursts of changes by one person, changes out of hours and first-time changers.
- **Journal integrity**: each closed day is sealed with a SHA-256 hash linked to the previous day, so a deleted or altered entry is detected. This is integrity evidence, not authorship or a signature.
- **Legal hold** stops the daily retention purge from deleting anything detected since a date, until you release it with a reason.

**App administration log** records who changed the app's settings, rules, baselines, freeze windows, holds or owners, and when.

Reports:

- **Compliance reports**: export the journal for a date range to CSV, or download an **evidence package** (ZIP with the journal, the control report, acknowledgements and a manifest with the SHA-256 of each file). Exports carry a SHA-256 hash that you can check later in the app.
- **Control report**: which changes map to each SOC 2 or ISO 27001 control, how many were reviewed and by whom. The mapping is indicative; it is not a certification.
- **Handover report**: how the installation is set up, for a new administrator.

## 9. Email notifications

Emails are sent by Jira from a **channel issue** that you choose. Only people on a recipient list who can see that issue receive them. Set them up under **Configuration**:

1. **Email channel and digest**: turn emails on, pick the channel issue and the template language (English or Spanish), and choose a daily or weekly digest. Optionally create one issue per alert.
2. **Per-issue notifications** (in the same page): pick up to 10 issues and notify their project lead, reporter, assignee or a specific user whose access to the issue is verified.
3. **Recipients**: lists of active people and groups. The app validates that each recipient can see the channel issue.
4. **Email templates**: subject and HTML body for alerts and digests. Default templates can be duplicated and edited.
5. **Alert rules**: which changes send an alert (minimum risk, object type, change type, origin, space), to which list and with which template. **Preview** shows what the rule would have sent in the last 7 and 30 days before you save it.

**Follow-up notices** (off by default) email administrators when an object they own or follow changes. The **delivery log** shows every email sent or failed.

## 10. Settings

All under **Configuration** in the console; they affect the whole site except **User preferences**.

| Page | What it controls |
|---|---|
| **Retention and budget** | How many days the journal is kept, the site time zone used for risk and alerts, the daily API points budget, the weekly privacy report to Atlassian and whether a change request is required when marking reviewed. |
| **Recorded change types** | Record creations, modifications and deletions. What you untick is not stored and cannot be recovered. |
| **Sources and scan cadence** | Turn each source on or off and choose daily or weekly scans. |
| **Access control** | Let project administrators audit their project and let team members see change summaries. |
| **User preferences** | Your own default filters, period and page size. |
| **Integration health** | The status of each source in its last scan. |
| **Diagnostics** | Grouped errors with a request ID for support, and **Copy support bundle** to send to us. No personal data is stored there. |
| **Service objectives** | Scan success, data freshness and detection time against the targets you set. |

## 11. Rovo agent and Jira Automation

- **Site Change Journal analyst** (Rovo) answers in plain language who changed what, when, where it applies and what is still pending. It is read-only except for marking a change reviewed, which it asks you to confirm.
- **Jira Automation actions**: **Get pending risky configuration changes**, **Mark change reviewed** and **Baseline drift**, with outputs you can use in the rest of the rule.

## 12. FAQ

**Does the app change my Jira configuration?** No. It only reads it.

**Why does a change say "Undetermined"?** The Jira audit log was not available for that period. The app prefers not to name anyone rather than guess.

**Why can't I see who made a change?** Only administrators see people and values. Team members see what changed and when.

**When is the data updated?** On each scheduled scan, when you press **Scan now**, and right away for Jira events.

**How long is the journal kept?** The period set in **Retention and budget**. Older entries are deleted by the daily purge, unless a legal hold covers them. Everything is deleted when you uninstall the app.

**Do freeze windows or legal holds block anything?** No. Freeze windows mark changes; legal holds only stop the purge.

**Emails are not arriving.** Check that emails are turned on, that the list has valid recipients and that they can see the channel issue, then look at the delivery log.

**Does the app read issue content?** It does not store it. Per-issue notifications read the issue's summary, project lead, reporter and assignee at the moment you configure or send them.

---

Need help? Write to support@lensdelta.com. See [Support](support.html), [Privacy Policy](privacy.html) and [Security Policy](security.html).
