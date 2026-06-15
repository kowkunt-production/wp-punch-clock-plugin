# PROJECT MANAGEMENT PLAN

## *WP Punch Clock Plugin*
WordPress Punch Clock Plugin Development template.  WP Punch Clock Plugin is applicable to any organisation with field workers.

| Document Control | |
|-------------------|---|
| **Project Manager** | Nivan Mikhail |
| **Date** | 15/06/2026 |
| **Version** | 1.0 |
| **Status** | Draft for Approval |

---

## 1. PROJECT CHARTER

### 1.1 Project Purpose & Business Case

> Organisations with field-based or remote workers — construction firms, security patrols, home healthcare providers, delivery fleets, event staffing agencies, facilities management teams — face a common payroll problem: verifying that a worker was physically present at the correct location when they clocked in and out.
>
> Current manual processes rely on paper timesheets, SMS check-ins, or honour systems. These introduce a **3–5 day administrative lag** per pay cycle, carry a typical **error rate of 10–15%** (overpayments, underpayments, correction cycles), and offer no verifiable proof of attendance at the point of work. Disputes over hours worked are common and difficult to resolve without evidence.
>
> **WP Punch Clock** replaces this entirely. Using any modern smartphone browser, a worker visits the organisation's WordPress portal, clocks in or out via a shortcode-powered page, and the plugin simultaneously captures three things: a **photo of the worker** via the device camera, the **precise GPS coordinates** of the device, and a **date/time stamp**. This is all tied to the worker's **unique Member-ID**.
>
> On clock-out, the plugin calculates the **straight-line distance** between the worker's GPS fix and a **pre-configured site destination point** — verifying whether they were within an acceptable radius of the work site. It then **pairs the clock-in and clock-out records** for that Member-ID, computes total hours worked (with configurable break deductions), and **automatically emails a verified shift summary** to the worker, their assigned manager, and the payroll administrator. All data is stored in an immutable, exportable log.
>
> **Key outcomes for any adopting organisation:**
> - Administrative time for payroll processing **cut from days to minutes**
> - Payroll error rate reduced to **under 2%**
> - **GPS-verified attendance** replaces trust-based timesheets
> - **Photo + location evidence** resolves disputes instantly
> - **CSV export** feeds directly into existing payroll systems
> - Fully **self-hosted** on the organisation's own WordPress site — no third-party SaaS subscription, no per-user fees
> - Works on **any modern smartphone** — no app install required

### 1.2 Project Objectives (SMART)

| # | Objective | Success Measure |
|---|-----------|-----------------|
| 1 | Deliver a functional WordPress plugin that captures a camera image, GPS coordinates, timestamp, and Member-ID at clock-in and clock-out via a shortcode on a WordPress page | Plugin installed on staging, all 8 core test cases pass |
| 2 | Calculate straight-line (Haversine) distance between the worker's GPS fix and the pre-configured site destination coordinates, flagging any clock event where distance exceeds the configurable threshold radius | Distance logged correctly to 2 decimal places for 10 test coordinate pairs; out-of-range events trigger a visible warning and admin notification |
| 3 | Pair clock-in and clock-out records by Member-ID and date to calculate total hours worked, with configurable auto-deduction for unpaid breaks | Payroll export matches 10 manually verified timesheets to within 1 minute |
| 4 | On clock-out, automatically email a shift summary to the Member (worker), the Member's assigned Owner/Manager, and the site Admin | 100% email delivery in UAT; correct recipients; email content matches record |
| 5 | Provide CSV export of payroll-ready data filterable by Member-ID, date range, and site | 10-row CSV matches on-screen records; format accepted by standard payroll systems |

### 1.3 High-Level Scope

| In Scope | Out of Scope |
|----------|--------------|
| WP plugin: admin settings page, custom post type for punch records, shortcode `[punch_clock]` | Native iOS/Android app (responsive web only) |
| Camera access via browser MediaDevices API (`getUserMedia`) | Facial recognition or liveness detection |
| GPS capture via browser Geolocation API | Offline mode (requires active internet connection) |
| Image upload with metadata (Member-ID, lat, lng, timestamp, IN/OUT type) attached to record | Editing or deleting submitted punch records by workers |
| Haversine formula for distance calculation between two lat/lng pairs | Multi-site enforcement (worker can clock at any configured site) |
| Admin-configurable site destination points: Name, Lat, Lng, Radius tolerance (metres) | Tax withholding, pay rate application, or superannuation calculation |
| Clock-in/out pairing logic: finds open clock-in for same Member-ID, same calendar day | Employee scheduling, rostering, or shift planning |
| Working hours calculation, optional break deduction setting (minutes) | SMS or push notifications |
| Automated email on clock-out to: Member, Member's assigned Owner/Manager, Admin | Front-end user registration or member creation |
| Email: shift summary, total hours, distance check result, photo thumbnail | Biometric time clocks or hardware integration |
| CSV export: admin-only, filterable, all punch data + calculated hours | API for third-party HRIS/payroll systems (future phase) |
| Role-based access: Administrator (all), Manager (their members), Member (own only) | |
| Internationalisation ready (all strings translatable) | |
| Designed to meet WordPress Plugin Review Guidelines | |

### 1.4 Key Stakeholders

| Name/Role | Interest | Influence |
|-----------|----------|------------|
| PM: Alex Rivera | On-time, on-budget delivery | High |
| Lead Developer: TBC (contract) | Code architecture, plugin quality | High |
| Payroll Administrator (end-user rep) | Export format, accuracy, email receipt workflow | High |
| Site/Team Manager (end-user rep) | Geofence logic, team oversight dashboard, dispute resolution | Med |
| Field Worker (end-user reps: 2) | Mobile UX, camera flow, clock-in/out speed, ease of use | Med (High for UAT sign-off) |
| IT/DevOps (system administrator rep) | WordPress environment, HTTPS, email deliverability, SSL certificate, server disk space | Med |

### 1.5 Assumptions & Constraints

| Assumptions | Constraints |
|-------------|-------------|
| Adopting organisation's WordPress site runs on HTTPS (mandatory for camera/geolocation browser APIs) | Browser-only: no native app. UX is limited to what a mobile web browser can provide |
| Target devices are modern smartphones (iOS 15+ / Android 12+, Chrome or Safari) | Plugin must operate on a standard WordPress installation — no custom server configuration required |
| Organisation has an existing system for unique Member-IDs (HR system, employee database, or manual assignment) | Images stored in WordPress Media Library — adopting organisation must monitor server disk space |
| A WordPress user account per Member-ID exists or can be created programmatically | Email delivery depends on the host's wp_mail() function or a configured SMTP plugin |
| Site destination coordinates (lat/lng) are known or obtainable (e.g., Google Maps picker) | Haversine distance is straight-line only — does not account for terrain, buildings, GPS drift, or site perimeter shape |
| Workers use their own smartphones (BYOD) or organisation-provided devices with a modern browser | GPS accuracy depends on device hardware and environment (typically 5–20 metres with clear sky view) |
| Plugin is for self-hosted use — not initially distributed via the WordPress.org repository | |

### 1.6 High-Level Risks

| Risk | Potential Impact |
|------|------------------|
| Browser denies camera/geolocation permission (user declines, or browser too old) | Worker cannot clock in/out; organisation needs a manual fallback procedure |
| GPS accuracy poor (urban canyons, inside large structures, remote sites with weak signal) | Distance check produces false out-of-range flags; worker disputes arise; administrative overhead increases |
| Email delivery fails (hosting provider limits, spam filtering, misconfigured SMTP) | Shift records not received by Member/Manager/Admin; payroll processing delayed; trust in system erodes |
| Image uploads consume server disk space rapidly (Example: 100 workers × 2 photos/day × 20 working days = 4,000 images/month) | Server storage exhausted; plugin stops accepting clock events; WordPress site may fail |
| Worker forgets to clock out (phone battery dies, distraction, end-of-shift rush) | Open-ended shift record; hours cannot be calculated; payroll admin must manually intervene |
| WordPress core or PHP version update introduces a breaking change | Plugin stops functioning; all attendance tracking halts across the organisation |

### 1.7 Summary Milestones

| Milestone | Target Date |
|-----------|-------------|
| Project Kick-off | 01/07/2026 |
| Requirements & UX wireframes approved | 14/07/2026 |
| Development environment ready (local + staging WP) | 07/07/2026 |
| Core clock-in/out with camera + GPS capture working | 04/08/2026 |
| Distance calculation + geofence check implemented | 18/08/2026 |
| Clock pairing + hours calculation + break deduction | 01/09/2026 |
| Email notification system complete | 15/09/2026 |
| CSV export + admin dashboard | 22/09/2026 |
| Internal alpha testing (development team) | 29/09/2026 |
| UAT with end-user representatives (managers + field workers) | 13/10/2026 |
| Bug fixes + UAT sign-off | 27/10/2026 |
| Production deployment (first adopting organisation) | 03/11/2026 |
| First live payroll run verified using plugin data | 15/11/2026 |
| Project closure | 22/11/2026 |

### 1.8 Budget Summary

| Category | Amount |
|----------|--------|
| Lead Developer (contract, 16 weeks × 30 hrs/week @ $65/hr) | $31,200 |
| UX/UI Designer (wireframes + mobile-first interface, 2 weeks) | $4,800 |
| Project Manager (part-time, 4 months) | $8,000 |
| QA Testing (internal testing + UAT coordination, 3 weeks) | $4,500 |
| Staging server setup + SSL certificate (4 months) | $400 |
| Email delivery service (SMTP relay, 4 months development + 12 months production) | $600 |
| Contingency Reserve (10%) | $4,950 |
| **Total Budget** | **$54,450** |

---

## 2. SCOPE MANAGEMENT

### 2.1 Work Breakdown Structure (WBS)

```
1. WP Punch Clock Plugin
   1.1 Initiation
       1.1.1 Signed Project Charter
       1.1.2 Stakeholder Register Complete
       1.1.3 Detailed Requirements Document (this document)
       1.1.4 Technical Feasibility Confirmed (browser API tests on target devices)
       1.1.5 Development Environment (local dev + WP staging site, HTTPS, SMTP)
   1.2 Planning
       1.2.1 UX Wireframes (Mobile-First — clock-in/out screen, settings, dashboards)
       1.2.2 Technical Architecture Document (plugin structure, database schema, WordPress hooks)
       1.2.3 Risk Register Populated
       1.2.4 Test Plan (8 core test cases, UAT script)
       1.2.5 PM Plan Baselined
   1.3 Execution – Core Plugin Foundation
       1.3.1 Plugin bootstrap (main file, activation/deactivation hooks, uninstall routine)
       1.3.2 Custom Post Type: 'punch_record' registered with all required meta fields
       1.3.3 Admin Settings Page (Site Destinations CRUD, geofence radius, email templates, break deduction setting)
       1.3.4 Shortcode: [punch_clock] renders clock-in/out button, camera viewfinder, status display
       1.3.5 Camera Capture (MediaDevices.getUserMedia, canvas snapshot, Blob creation, AJAX upload)
       1.3.6 Geolocation Capture (Geolocation API, lat/lng/accuracy retrieval, error handling for denied/lost permission)
       1.3.7 Punch Record Save (image → WordPress Media Library, all metadata → post_meta, AJAX response to frontend)
   1.4 Execution – Distance Calculation
       1.4.1 Site Destination Selector (dropdown or auto-detect nearest configured site)
       1.4.2 Haversine Formula Implementation (PHP function, unit-testable, returns distance in metres)
       1.4.3 Distance Threshold Check (compare calculated distance to site's configured radius)
       1.4.4 Out-of-Range Warning (frontend notification to worker + flag stored on punch record)
       1.4.5 Admin Notification for Out-of-Range Events (email or dashboard alert)
   1.5 Execution – Working Hours Engine
       1.5.1 Clock-In/Out Pairing Logic (find open IN record for same Member-ID, same calendar day)
       1.5.2 Hours Calculation (OUT timestamp minus IN timestamp, minus configurable break deduction)
       1.5.3 Edge Case Handling (multiple INs without OUT, overnight shifts crossing midnight, missed clock-outs)
       1.5.4 Daily/Weekly Totals per Member-ID (displayed in Member and Manager dashboards)
   1.6 Execution – Notifications
       1.6.1 Email Template System (merge tags: {member_name}, {member_id}, {date}, {hours}, {distance}, {site_name}, {photo_url})
       1.6.2 Member Email Trigger (sent on successful clock-out, contains shift summary)
       1.6.3 Manager/Owner Email Trigger (sent to WordPress user assigned to that Member-ID)
       1.6.4 Admin Email Trigger (CC or separate email on all clock-out events)
       1.6.5 Email Logging (record of each email sent, stored with punch record for audit)
   1.7 Execution – Reporting & Export
       1.7.1 Admin Punch Log Table (filterable by Member-ID, date range, site, IN/OUT status, out-of-range flag)
       1.7.2 CSV Export (admin-only, respects current on-screen filters, payroll-ready column format)
       1.7.3 Member Dashboard (own punch history, total hours this pay period, current open-shift status)
       1.7.4 Manager Dashboard (team view — assigned Members, open clock-ins highlighted, team totals)
   1.8 Execution – Role & Access Control
       1.8.1 Administrator Role (full access: all settings, all records, CSV export, site destination management)
       1.8.2 Manager/Owner Role (view assigned Members only, team dashboard, no settings access)
       1.8.3 Member Role (clock in/out only, view own history, no access to other records or settings)
       1.8.4 WordPress Capability Mapping & Menu Visibility per Role
   1.9 Execution – Quality & Hardening
       1.9.1 WordPress Coding Standards Compliance (PHPCS WordPress-Extra: 0 errors)
       1.9.2 Input Validation & Sanitisation (all $_POST, $_GET, AJAX inputs, user-supplied data)
       1.9.3 Output Escaping (all frontend renders: esc_html, esc_attr, esc_url, wp_kses as appropriate)
       1.9.4 Nonce Verification (all forms, all AJAX endpoints)
       1.9.5 Capability Checks (current_user_can on all sensitive operations)
       1.9.6 Image Handling Security (file type validation server-side, EXIF metadata stripped, file size limits)
       1.9.7 Internationalisation (all user-facing strings in __(), _e(), esc_html__(), etc.; .pot file generated)
       1.9.8 Cross-Browser Testing (Chrome Android, Safari iOS, Chrome Desktop, Firefox Desktop)
   1.10 Execution – Testing & UAT
       1.10.1 Unit Tests (PHPUnit: Haversine formula, hours calculation, clock-pairing logic, break deduction)
       1.10.2 Internal Alpha Testing (development team uses plugin for 3 days, all bugs logged)
       1.10.3 UAT with End-User Representatives (2 managers + 5 field workers from target user base, 3 days of real use)
       1.10.4 Bug Fixes from UAT (triaged: critical → high → medium → cosmetic)
       1.10.5 UAT Sign-Off from Payroll Administrator and Manager Representatives
   1.11 Closure
       1.11.1 Production Deployment (plugin uploaded, activated, configured on live WordPress site)
       1.11.2 First Payroll Run Verification (manual cross-check of plugin output vs. existing method)
       1.11.3 User Documentation (Administrator setup guide, Manager guide, Worker one-page quick reference)
       1.11.4 Lessons Learned Workshop (entire project team)
       1.11.5 Final Project Report & Budget Reconciliation
       1.11.6 Transition to Maintenance (ongoing support plan, update testing cadence, bug report channel)
```

### 2.2 Scope Change Control Process

1. Change Request submitted via the Change Request Form (see Appendix)
2. PM logs request and assesses impact: schedule, cost, quality, risk
3. If schedule impact > 1 week or cost impact > $1,000, PM escalates for decision
4. Decision: Approved, Rejected, or Deferred to a future phase
5. If approved: plan baselines updated, development team briefed, work begins
6. If rejected: logged with rationale, requester notified

---

## 3. SCHEDULE MANAGEMENT

### 3.1 Schedule Baseline

| Phase | Duration | Start | Finish | Key Deliverable |
|-------|----------|-------|--------|-----------------|
| Initiation | 2 weeks | 01/07/26 | 14/07/26 | Signed charter, requirements, dev environment ready |
| Planning | 1 week | 15/07/26 | 21/07/26 | Wireframes, architecture doc, test plan baselined |
| Core Foundation | 3 weeks | 22/07/26 | 11/08/26 | Clock-in/out with camera + GPS working on staging |
| Distance Calculation | 2 weeks | 12/08/26 | 25/08/26 | Haversine distance + geofence flag functional |
| Working Hours Engine | 2 weeks | 26/08/26 | 08/09/26 | Clock pairing, hours calculation, break deduction |
| Notifications | 2 weeks | 09/09/26 | 22/09/26 | Emails sent to Member, Manager, Admin on clock-out |
| Reporting & Export | 2 weeks | 23/09/26 | 06/10/26 | CSV export, filterable admin log, Member + Manager dashboards |
| Role & Access Control | 1 week | 07/10/26 | 13/10/26 | Three roles functional, capability checks in place |
| Quality Hardening | 2 weeks | 07/10/26 | 20/10/26 | PHPCS clean, security review, i18n complete *(parallel with Reporting)* |
| Alpha Testing | 1 week | 21/10/26 | 27/10/26 | Internal bugs found and logged |
| UAT | 2 weeks | 28/10/26 | 10/11/26 | End-user testing, feedback collected |
| Bug Fixes | 1 week | 11/11/26 | 17/11/26 | Critical + high bugs resolved |
| Production Deploy | 1 day | 18/11/26 | 18/11/26 | Plugin live on production WordPress site |
| Payroll Verification | 1 week | 19/11/26 | 25/11/26 | Cross-check with existing payroll method |
| Closure | 2 days | 26/11/26 | 27/11/26 | Documentation, lessons learned, team release |

**Total Duration: 18 weeks (01 July – 27 November 2026)**

*(Detailed Gantt chart with task-level dependencies maintained in project management tool. Critical path: Core → Distance → Hours → Notifications → UAT. A delay in Core Foundation pushes the entire schedule.)*

### 3.2 Schedule Control
- Developer stand-up: Monday/Wednesday/Friday, 15 minutes, async (Slack)
- PM reviews progress against baseline every Friday
- Variance greater than 3 working days on the critical path triggers formal review
- UAT dates are firm — end-user representative availability is booked in advance

---

## 4. COST MANAGEMENT

### 4.1 Cost Baseline

| WBS Item | Budget | Actual (to date) | Variance |
|----------|--------|-------------------|----------|
| 1.1 Initiation | $2,600 | — | — |
| 1.2 Planning | $3,250 | — | — |
| 1.3 Core Foundation | $5,850 | — | — |
| 1.4 Distance Calculation | $3,900 | — | — |
| 1.5 Working Hours Engine | $3,900 | — | — |
| 1.6 Notifications | $3,900 | — | — |
| 1.7 Reporting & Export | $3,900 | — | — |
| 1.8 Role & Access Control | $1,950 | — | — |
| 1.9 Quality Hardening | $3,900 | — | — |
| 1.10 Alpha + UAT + Bug Fixes | $7,800 | — | — |
| 1.11 Closure | $1,300 | — | — |
| Infrastructure (Staging Server + SMTP) | $1,000 | — | — |
| Contingency Reserve | $4,950 | — | — |
| **TOTAL** | **$54,450** | **—** | **—** |

### 4.2 Cost Control
- Developer invoices submitted fortnightly, cross-checked against completed WBS items
- Any single expense exceeding $500 requires PM approval
- Contingency reserve drawn only with documented justification
- Budget status reviewed at each monthly milestone checkpoint

---

## 5. QUALITY MANAGEMENT

### 5.1 Quality Standards

| Deliverable | Quality Standard | Verification Method |
|-------------|------------------|---------------------|
| Camera capture | Image captured and uploaded in under 3 seconds on 4G mobile connection; minimum resolution 640×480 pixels | Manual test on 3 target devices (entry-level Android, mid-range Android, iPhone SE) |
| GPS accuracy | Lat/lng recorded to 6 decimal places; device-reported accuracy value stored alongside coordinates | Verified against known survey coordinates at 3 test locations |
| Distance calculation | Haversine result matches established measurement (Google Maps "measure distance") to within 2% | Automated unit test with 10 known coordinate pairs |
| Working hours | Clock-in to clock-out duration correct to within 1 minute across 10 test scenarios (including break deduction, overnight) | PHPUnit test suite; UAT cross-check with manual timesheets |
| Email delivery | 100% of test emails received by all 3 recipient types within 60 seconds of clock-out | UAT: email verification by Member, Manager, and Admin reps × 5 clock-out events |
| CSV export | Columns match standard payroll import format; data rows exactly match on-screen dashboard records | Payroll administrator rep verifies 10 exported rows against dashboard display |
| Security | Zero vulnerabilities from OWASP Top 10 applicable to WordPress plugins | Manual code review: input validation, output escaping, nonce verification, capability checks confirmed |
| Code quality | PHPCS WordPress-Extra standard: 0 errors, fewer than 5 warnings | CI pipeline (GitHub Actions or similar) check on every commit |
| Browser compatibility | Fully functional on Chrome Android 120+, Safari iOS 17+, Chrome Desktop 120+, Firefox Desktop 120+ | Cross-browser manual test suite before Alpha release |
| Internationalisation | All user-facing strings wrapped in translation functions; .pot file generated and valid | WP-CLI `i18n make-pot` check; spot-check 20 strings |

### 5.2 Quality Control Activities

| Activity | Frequency | Owner |
|----------|-----------|-------|
| PHPCS lint check | Every commit (automated CI) | Lead Developer |
| Unit tests executed | Every commit | Lead Developer |
| Peer code review | Before merge to main branch | PM or second developer |
| Manual security review | End of Quality Hardening phase (week 14) | PM + external reviewer (if available) |
| Cross-browser/device testing | End of each execution phase | QA |
| UAT script walkthrough | UAT phase (week 16) | PM + end-user representatives |
| First payroll run cross-check | Week 17 (post go-live) | Payroll administrator rep |

---

## 6. RISK MANAGEMENT

### 6.1 Risk Register

| ID | Risk Description | Prob | Impact | Strategy | Mitigation / Contingency | Owner |
|----|------------------|------|--------|----------|--------------------------|-------|
| R1 | Browser denies camera/geolocation permission (user declines prompt, or unsupported browser) | High | High | Mitigate | In-app guidance with screenshots explaining how to enable permissions; detect permission state and show contextual help; fallback: admin can manually record a punch event with photo uploaded from camera roll and coordinates entered manually | Lead Dev |
| R2 | GPS accuracy poor (urban canyons, inside large buildings, remote sites with weak satellite signal) | Med | High | Mitigate | Store device-reported accuracy value with each record; if accuracy > 50 metres, warn worker and flag record for manager review; set generous default geofence radius (100m) that admin can adjust per site; manager has authority to override out-of-range flags | Lead Dev |
| R3 | Worker forgets to clock out (phone battery dies, distraction, end-of-shift rush) | High | Med | Mitigate | Admin dashboard prominently highlights all "open" clock-ins older than configurable threshold; auto-email reminder to worker at configurable interval (e.g., 10 hours after clock-in); admin can manually close an open shift with a note explaining the correction | PM / Lead Dev |
| R4 | Server disk space exhausted by accumulated image uploads | Med | High | Mitigate | Compress images client-side before upload (max width 800px, JPEG quality 70%); configurable auto-delete policy for images older than X days; admin dashboard shows current storage usage; server-level disk space monitoring alert at 80% capacity | IT / DevOps rep |
| R5 | Email delivery fails (hosting provider limits, spam classification, missing SPF/DKIM records) | Med | High | Mitigate | Recommend and document SMTP relay setup (Mailgun, SendGrid, or similar — free tiers sufficient for typical use); log all outbound email attempts in plugin; display email status in admin dashboard; all records always viewable in dashboard regardless of email delivery status | Lead Dev / IT |
| R6 | WordPress core update introduces a breaking change that disables plugin functionality | Low | High | Accept (with contingency) | Plugin uses only standard WordPress APIs and hooks (no deprecated functions); test against WordPress beta releases when available; maintain documented rollback procedure; identify backup developer for emergency fixes | Lead Dev |
| R7 | Sole developer unavailability (sickness, departure, key person dependency) | Low | Critical | Mitigate | Code committed to shared repository daily; comprehensive inline code documentation maintained; architecture document kept current; identified backup developer with access to repo and briefed on codebase | PM |

### 6.2 Risk Review
- Risk register reviewed at weekly PM-Developer check-in (Fridays)
- New risks added within 24 hours of identification
- Probability and impact re-assessed at each monthly milestone checkpoint
- Top 3 risks actively discussed at each review

---

## 7. COMMUNICATION MANAGEMENT

| Audience | Information | Frequency | Method | Owner |
|----------|-------------|-----------|--------|-------|
| Lead Developer | Task priorities, blockers, technical decisions, upcoming milestones | Mon/Wed/Fri | Slack async stand-up + weekly 30-minute video call | PM |
| Payroll Administrator Rep | CSV format requirements, email content, UAT schedule | As needed + weekly during UAT | Email + UAT walkthrough session | PM |
| Site/Team Manager Reps | Geofence logic, dashboard design, UAT schedule and instructions | Fortnightly during development; daily during UAT | Email update + UAT briefing session | PM |
| Field Worker Reps (UAT) | Simple testing instructions, what to do, who to contact with issues | Day before UAT begins | Printed 1-page guide + 10-minute in-person/video briefing | PM |
| IT/DevOps Rep | Server requirements, SSL, SMTP setup, disk space monitoring | Initiation phase + pre-deployment | Email + setup checklist | PM |
| All Stakeholders | Major milestone achievements, upcoming activities, any schedule/cost variances | Monthly | 1-page status update email | PM |

---

## 8. PHASE GATES

| Gate | When | Criteria to Pass | Decision Maker |
|------|------|------------------|----------------|
| Gate 1: Initiation → Planning | 14/07/26 | Charter signed, requirements documented, technical feasibility confirmed, budget approved, development environment ready | PM |
| Gate 2: Planning → Execution | 21/07/26 | Wireframes approved, architecture document reviewed, test plan agreed, risk register populated, PM plan baselined | PM + Lead Developer |
| Gate 3: Core → Full Build | 25/08/26 | Clock-in/out working end-to-end (camera → GPS → save → display); distance calculation verified against test coordinates | PM + Lead Developer |
| Gate 4: Full Build → Alpha | 20/10/26 | All planned features functional on staging environment; PHPCS clean; manual security review completed; internationalisation complete | PM |
| Gate 5: Alpha → UAT | 27/10/26 | Zero critical bugs remaining; all 8 core test cases pass in internal testing | PM |
| Gate 6: UAT → Go-Live | 17/11/26 | UAT sign-off received from Payroll Administrator rep and Manager rep; all critical and high-priority bugs resolved | PM |
| Gate 7: Go-Live → Closure | 27/11/26 | First full payroll run verified against existing method; user documentation delivered; lessons learned workshop completed | PM |

---

## 9. CLOSURE CHECKLIST

| Item | Status | Notes |
|------|--------|-------|
| Plugin deployed to production WordPress site | ☐ | Activated, configured, shortcode page published and tested live |
| First payroll run cross-checked against existing method | ☐ | Payroll administrator rep confirms data accuracy within acceptable tolerance |
| User documentation delivered | ☐ | Administrator setup guide, Manager guide, Worker one-page quick reference |
| Source code in shared repository with documentation | ☐ | Access granted to IT/DevOps; README includes setup and maintenance instructions |
| Lessons learned workshop held | ☐ | PM facilitates; notes captured and distributed to all project participants |
| Final budget reconciliation complete | ☐ | Actuals compared to baseline; variances documented and explained |
| Contingency reserve remaining accounted for | ☐ | Returned or allocated with documented decision |
| Maintenance plan documented and agreed | ☐ | Bug report channel, WordPress update testing cadence, responsible person(s) identified |
| Project team formally released | ☐ | Acknowledgement and thanks communicated |
| Final project report submitted | ☐ | Includes: objectives achieved, final budget, lessons learned, recommendations for future phases |

---

## APPENDIX A: CHANGE REQUEST FORM

| Field | Details |
|-------|---------|
| **Change Request ID** | CR-[###] |
| **Date Requested** | [DD/MM/YYYY] |
| **Requested By** | [Name and Role] |
| **Description of Change** | [Clear description of what is being requested] |
| **Reason for Change** | [Why is this needed? What problem does it solve?] |
| **Impact Assessment** | |
| — Schedule Impact | [+/- days or weeks] |
| — Cost Impact | [+/- $ amount] |
| — Quality Impact | [Does this improve, reduce, or not affect quality?] |
| — Risk Impact | [Does this introduce new risks or reduce existing ones?] |
| **Recommendation** | [PM's recommendation with rationale] |
| **Decision** | ☐ Approved ☐ Rejected ☐ Deferred to Phase 2 |
| **Approved By** | [Name] |
| **Date of Decision** | [DD/MM/YYYY] |

---

## APPENDIX B: CORE TEST CASES SUMMARY

| TC# | Scenario | Expected Result |
|-----|----------|-----------------|
| TC1 | Worker clocks IN: camera captures photo, GPS returns valid coordinates, distance is within configured geofence radius | Punch record saved with IN status, image attached, all metadata stored; success message displayed to worker |
| TC2 | Worker clocks OUT after TC1: camera captures photo, GPS valid, within geofence | Punch record saved with OUT status; hours calculated and displayed; email sent to Member, assigned Manager, and Admin within 60 seconds |
| TC3 | Worker clocks IN but GPS coordinates are outside configured geofence radius | Record saved but flagged "OUT OF RANGE"; prominent warning displayed to worker; admin notification generated |
| TC4 | Worker attempts to clock IN while an open clock-in (no matching OUT) already exists for the same Member-ID today | Second IN blocked; message displayed: "You already have an open shift. Please clock out before starting a new shift." |
| TC5 | Worker clocks OUT on a shift that spans midnight (overnight shift) | OUT correctly paired with the previous calendar day's IN record; total hours calculated correctly across the date boundary |
| TC6 | Admin exports CSV for a specific Member-ID and date range | Downloaded CSV contains correct rows matching on-screen dashboard data; columns match standard payroll import format; total hours match |
| TC7 | Manager views their team dashboard | Dashboard displays only Members assigned to that Manager; open shifts visually highlighted; Manager cannot access plugin settings or other Managers' teams |
| TC8 | Email received by all three intended recipients after clock-out | Member receives shift summary with hours and photo; Manager receives team member's shift summary; Admin receives copy; all delivered within 60 seconds |

