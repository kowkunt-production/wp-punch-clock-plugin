Here's a complete filled-in example of the **WordPress Plugin Development** template applied to your specific plugin project. I've kept it practical and focused on what a developer or PM would actually need.

---

# PROJECT MANAGEMENT PLAN
## *WP Punch Clock Plugin*

| Document Control | |
|-------------------|---|
| **Project Manager** | Alex Rivera |
| **Sponsor** | Greenway Construction Ltd (James Okonkwo, Ops Director) |
| **Date** | 15/06/2026 |
| **Version** | 1.0 |
| **Status** | Draft for Approval |

---

## 1. PROJECT CHARTER

### 1.1 Project Purpose & Business Case

> Greenway Construction has 120 field workers across 14 active sites. Currently, attendance is tracked on paper timesheets — photographed manually by site supervisors, transcribed into spreadsheets, and manually entered into payroll. This takes 3–4 working days per fortnightly pay cycle with a 12% error rate (overpayments + underpayments + corrections).
>
> **WP Punch Clock** replaces this entirely: a worker visits the company's WordPress portal on their phone, the plugin accesses the device camera via the browser, captures a geo-tagged photo at clock-in and clock-out, calculates the straight-line distance between the worker's GPS coordinates and a pre-configured site destination point (to verify the worker is actually on site), logs the working hours per Member-ID, and emails the verified shift record to the worker, their assigned site manager, and the payroll admin.
>
> Expected savings: **22 admin hours per week**, payroll error rate reduced to **under 2%**, and a **fully auditable attendance trail**.

### 1.2 Project Objectives (SMART)

| # | Objective | Success Measure |
|---|-----------|-----------------|
| 1 | Deliver a functional WordPress plugin that captures a camera image, GPS coordinates, timestamp, and Member-ID at clock-in and clock-out via a shortcode on a WordPress page | Plugin installed on staging, all 8 core test cases pass |
| 2 | Calculate straight-line (Haversine) distance between the worker's GPS fix and the pre-configured site destination coordinates, flagging any clock event where distance exceeds the configurable threshold radius | Distance logged correctly to 2 decimal places for 10 test coordinate pairs; out-of-range events trigger a visible warning and admin notification |
| 3 | Pair clock-in and clock-out records by Member-ID and date to calculate total hours worked, with configurable auto-deduction for unpaid breaks | Payroll export matches 10 manually verified timesheets to within 1 minute |
| 4 | On clock-out, automatically email a shift summary to the Member (worker), the Member's assigned Owner/Manager, and the site Admin | 100% email delivery in UAT; correct recipients; email content matches record |
| 5 | Provide CSV export of payroll-ready data filterable by Member-ID, date range, and site | 10-row CSV matches on-screen records; format accepted by payroll processor |

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
| Passes WordPress Plugin Review Guidelines (if submitted to .org repo) | |

### 1.4 Key Stakeholders

| Name/Role | Interest | Influence |
|-----------|----------|------------|
| Sponsor: James Okonkwo, Ops Director | Budget authority, field adoption mandate | High |
| PM: Alex Rivera | On-time, on-budget delivery | High |
| Lead Developer: TBC (contract) | Code architecture, plugin quality | High |
| Payroll Admin: Grace Liu | Export format, accuracy, email receipt | High |
| Site Manager (rep): Mike Turner | Geofence logic, team oversight dashboard | Med |
| Field Worker (reps): 2 crew | Mobile UX, camera flow, speed | Med (High for UAT sign-off) |
| IT/DevOps: Sam Patel | WordPress environment, HTTPS, email deliverability, SSL certificate | Med |

### 1.5 Assumptions & Constraints

| Assumptions | Constraints |
|-------------|-------------|
| Company WordPress site runs on HTTPS (mandatory for camera/geolocation API) | Browser-only: no native app. UX limited to what a mobile browser can do |
| Target devices are modern smartphones (iOS 15+/Android 12+ Chrome/Safari) | Plugin must work on a standard WordPress installation (no custom server config) |
| Workers have unique Member-IDs already assigned (from existing HR system) | Images stored in WP Media Library (server disk space must be monitored) |
| A single WordPress user account per Member-ID exists (or can be created) | Email delivery depends on the host's wp_mail() or SMTP plugin being functional |
| Site destination coordinates are known and can be entered by admin (lat/lng) | Haversine distance is straight-line — does not account for terrain, buildings, or GPS drift |
| Workers will use their own phones (BYOD) | No guaranteed GPS accuracy (typically 5–20m with good signal) |
| Plugin will not be distributed via WP.org initially (internal use first) | |

### 1.6 High-Level Risks

| Risk | Potential Impact |
|------|------------------|
| Browser denies camera/geolocation permission (user error or old browser) | Worker can't clock in/out; fallback manual entry needed |
| GPS accuracy poor in remote construction sites or inside structures | Distance check produces false out-of-range flags; worker disputes |
| Email delivery fails (host limits, spam filtering) | Shift records not received; payroll delayed; trust eroded |
| Image uploads consume server disk space rapidly (120 workers × 2 photos × 20 days = 4,800 images/month) | Server runs out of space; plugin fails; site goes down |
| Clock-in without matching clock-out (worker forgets, phone dies) | Open-ended shift; payroll calculation impossible; manual admin intervention needed |
| WordPress or PHP update breaks plugin functionality | Plugin stops working; all attendance tracking halts |

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
| Internal alpha testing (team) | 29/09/2026 |
| UAT with 2 site managers + 5 field workers | 13/10/2026 |
| Bug fixes + UAT sign-off | 27/10/2026 |
| Production deployment | 03/11/2026 |
| First live payroll run using plugin data | 15/11/2026 |
| Project closure | 22/11/2026 |

### 1.8 Budget Summary

| Category | Amount |
|----------|--------|
| Lead Developer (contract, 16 weeks × 30 hrs/week @ $65/hr) | $31,200 |
| UX/UI Designer (wireframes + mobile-first UI, 2 weeks) | $4,800 |
| Project Manager (part-time, 4 months) | $8,000 |
| QA Testing (internal + UAT coordination, 3 weeks) | $4,500 |
| Staging server setup + SSL (4 months) | $400 |
| Email delivery service (SMTP relay, 4 months test + 12 months live) | $600 |
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
       1.1.3 Detailed Requirements Document (this doc)
       1.1.4 Technical Feasibility Confirmed (browser API tests on target devices)
       1.1.5 Development Environment (local + WP staging site, HTTPS, SMTP)
   1.2 Planning
       1.2.1 UX Wireframes (Mobile-First)
       1.2.2 Technical Architecture Document (plugin structure, DB schema, hooks)
       1.2.3 Risk Register Populated
       1.2.4 Test Plan (8 core test cases, UAT script)
       1.2.5 PM Plan Baselined
   1.3 Execution – Core Plugin Foundation
       1.3.1 Plugin bootstrap (main file, activation/deactivation hooks)
       1.3.2 Custom Post Type: 'punch_record' with meta fields registered
       1.3.3 Admin Settings Page (Site Destinations CRUD, radius, email templates, break setting)
       1.3.4 Shortcode: [punch_clock] renders clock-in/out button + camera viewfinder
       1.3.5 Camera Capture (MediaDevices.getUserMedia, canvas snapshot, Blob upload)
       1.3.6 Geolocation Capture (Geolocation API, lat/lng/accuracy, error handling)
       1.3.7 Punch Record Save (image → Media Library, metadata → post_meta, AJAX)
   1.4 Execution – Distance Calculation
       1.4.1 Site Destination selector (dropdown or auto-detect nearest site)
       1.4.2 Haversine formula function (PHP, unit-testable, returns metres)
       1.4.3 Distance threshold check (compare calculated distance to site radius)
       1.4.4 Out-of-range warning (frontend notification + flag on record)
       1.4.5 Admin notification for out-of-range clock events
   1.5 Execution – Working Hours Engine
       1.5.1 Clock-in/out pairing logic (find open IN record for Member-ID on same day)
       1.5.2 Hours calculation (OUT timestamp – IN timestamp, break deduction)
       1.5.3 Edge cases: multiple INs without OUT, overnight shifts, missed clock
       1.5.4 Daily/weekly totals per Member-ID
   1.6 Execution – Notifications
       1.6.1 Email template system (merge tags: {member_name}, {hours}, {distance}, etc.)
       1.6.2 Member email trigger (on clock-out, shift summary)
       1.6.3 Owner/Manager email trigger (assigned via Member-ID user meta)
       1.6.4 Admin email trigger (CC all clock-out summaries)
       1.6.5 Email logging (for debugging, stored with record)
   1.7 Execution – Reporting & Export
       1.7.1 Admin punch log table (filterable by Member-ID, date, site, IN/OUT status)
       1.7.2 CSV export (admin-only, respects current filters, payroll-ready columns)
       1.7.3 Member dashboard: own history, total hours this period
       1.7.4 Owner/Manager dashboard: team view, pending (open) clock-ins
   1.8 Execution – Role & Access Control
       1.8.1 Role: Administrator (full access, all settings, all records)
       1.8.2 Role: Manager/Owner (view assigned Members only, no settings)
       1.8.3 Role: Member (view own records only, clock in/out)
       1.8.4 Capability mapping + menu visibility per role
   1.9 Execution – Quality & Hardening
       1.9.1 WordPress Coding Standards (PHPCS pass, 0 errors)
       1.9.2 Input validation & sanitization (all $_POST, $_GET, user input)
       1.9.3 Output escaping (all frontend renders)
       1.9.4 Nonce verification (all forms, AJAX)
       1.9.5 Capability checks (all sensitive operations)
       1.9.6 Image handling security (file type validation, EXIF stripping)
       1.9.7 Internationalisation (all strings in __(), _e(), etc., .pot file generated)
       1.9.8 Cross-browser testing (Chrome Android, Safari iOS, Chrome Desktop)
   1.10 Execution – Testing & UAT
       1.10.1 Unit tests (PHPUnit: Haversine, hours calculation, pairing logic)
       1.10.2 Internal alpha testing (whole team, find bugs)
       1.10.3 UAT with 2 site managers + 5 field workers (real construction site, 3 days)
       1.10.4 Bug fixes from UAT (prioritised: critical → high → medium)
       1.10.5 UAT sign-off from Sponsor and Payroll Admin
   1.11 Closure
       1.11.1 Production deployment (plugin uploaded, activated, configured)
       1.11.2 First payroll run verification (manual cross-check)
       1.11.3 User documentation (admin guide, worker one-pager)
       1.11.4 Lessons learned workshop
       1.11.5 Project report + final budget reconciliation
       1.11.6 Transition to maintenance (ongoing support plan)
```

### 2.2 Scope Change Control Process

1. Change Request submitted via the Change Request Form (see Appendix)
2. PM logs request and assesses impact: schedule, cost, quality, risk
3. If schedule impact > 1 week or cost impact > $1,000, PM escalates to Sponsor
4. Sponsor decision: Approved, Rejected, or Deferred to Phase 2
5. If approved: plan baselines updated, team briefed, work begins
6. If rejected: logged with rationale, requester notified

**Example during this project:** Payroll Admin Grace requested a "manual punch entry" form for admins to fix missed clock-outs. Assessed at +2 weeks development. Sponsor deferred to Phase 2; temporary workaround: admin can add a punch_record directly via WordPress admin until then.

---

## 3. SCHEDULE MANAGEMENT

### 3.1 Schedule Baseline

| Phase | Duration | Start | Finish | Key Deliverable |
|-------|----------|-------|--------|-----------------|
| Initiation | 2 weeks | 01/07/26 | 14/07/26 | Signed charter, requirements, dev env ready |
| Planning | 1 week | 15/07/26 | 21/07/26 | Wireframes, architecture doc, test plan |
| Core Foundation | 3 weeks | 22/07/26 | 11/08/26 | Clock-in/out with camera + GPS working on staging |
| Distance Calculation | 2 weeks | 12/08/26 | 25/08/26 | Haversine working, geofence flag functional |
| Working Hours Engine | 2 weeks | 26/08/26 | 08/09/26 | Clock pairing, hours calc, break deduction |
| Notifications | 2 weeks | 09/09/26 | 22/09/26 | Emails sent to Member, Owner, Admin on clock-out |
| Reporting & Export | 2 weeks | 23/09/26 | 06/10/26 | CSV export, filterable admin log, member dashboard |
| Role & Access Control | 1 week | 07/10/26 | 13/10/26 | Three roles functional, capabilities tested |
| Quality Hardening | 2 weeks | 07/10/26 | 20/10/26 | PHPCS clean, security review, i18n done *(parallel with Reporting)* |
| Alpha Testing | 1 week | 21/10/26 | 27/10/26 | Internal bugs found and logged |
| UAT | 2 weeks | 28/10/26 | 10/11/26 | Field worker testing, sign-off |
| Bug Fixes | 1 week | 11/11/26 | 17/11/26 | Critical + high bugs fixed |
| Production Deploy | 1 day | 18/11/26 | 18/11/26 | Live site activated |
| Payroll Verification | 1 week | 19/11/26 | 25/11/26 | Cross-check with manual records |
| Closure | 2 days | 26/11/26 | 27/11/26 | Documentation, lessons learned, team release |

**Total Duration: 18 weeks (01 July – 27 November 2026)**

*(Detailed Gantt chart with dependencies maintained in project management tool. Critical path: Core → Distance → Hours → Notifications → UAT. A 2-week delay in Core Foundation pushes go-live by 2 weeks.)*

### 3.2 Schedule Control
- Developer stand-up: Monday/Wednesday/Friday, 15 min, async (Slack)
- PM reviews progress against baseline every Friday
- Variance > 3 working days triggers escalation to Sponsor
- UAT dates are **firm** — site manager and worker availability booked in advance

---

## 4. COST MANAGEMENT

### 4.1 Cost Baseline

| WBS Item | Budget | Actual (to 15/07/26) | Variance |
|----------|--------|----------------------|----------|
| 1.1 Initiation | $2,600 | $2,450 | -$150 |
| 1.2 Planning | $3,250 | $3,250 | $0 |
| 1.3 Core Foundation | $5,850 | — | — |
| 1.4 Distance Calculation | $3,900 | — | — |
| 1.5 Working Hours Engine | $3,900 | — | — |
| 1.6 Notifications | $3,900 | — | — |
| 1.7 Reporting & Export | $3,900 | — | — |
| 1.8 Role & Access Control | $1,950 | — | — |
| 1.9 Quality Hardening | $3,900 | — | — |
| 1.10 Alpha + UAT + Fixes | $7,800 | — | — |
| 1.11 Closure | $1,300 | — | — |
| Staging Server + SMTP | $1,000 | $400 | -$600 |
| Contingency Reserve | $4,950 | $0 | — |
| **TOTAL** | **$54,450** | **$6,100** | **-$750** |

### 4.2 Cost Control
- Developer invoices submitted fortnightly, cross-checked against completed WBS items
- Any single expense > $500 requires PM approval
- Contingency drawdown reported to Sponsor before use
- Budget status included in monthly Sponsor update

---

## 5. QUALITY MANAGEMENT

### 5.1 Quality Standards

| Deliverable | Quality Standard | Verification Method |
|-------------|------------------|---------------------|
| Camera capture | Image captured and uploaded in < 3 seconds on 4G connection; minimum 640×480 resolution | Manual test on 3 target devices (iPhone SE, Samsung A-series, Pixel) |
| GPS accuracy | Lat/lng recorded to 6 decimal places; accuracy value from device stored alongside | Verified against known coordinates at 3 test locations |
| Distance calculation | Haversine result matches Google Maps "measure distance" to within 2% | Automated unit test with 10 known coordinate pairs |
| Working hours | Clock-in/out duration correct to 1 minute for 10 test scenarios | Unit test suite; UAT cross-check with manual timesheet |
| Email delivery | 100% of test emails received within 60 seconds | UAT email check by all 3 recipient types × 5 clock-out events |
| CSV export | Columns match payroll spec; data matches on-screen record exactly | Payroll Admin Grace verifies 10 rows against dashboard |
| Security | Zero OWASP Top 10 vulnerabilities relevant to WP plugins | Manual code review; all inputs validated, outputs escaped, nonces verified |
| Code quality | PHPCS WordPress-Extra: 0 errors, < 5 warnings | CI pipeline check before every commit |
| Browser compatibility | Functional on Chrome Android 120+, Safari iOS 17+, Chrome Desktop 120+ | Cross-browser test suite (BrowserStack or manual) |

### 5.2 Quality Control Activities

| Activity | Frequency | Owner |
|----------|-----------|-------|
| PHPCS lint check | Every commit (CI pipeline) | Lead Developer |
| Unit tests run | Every commit | Lead Developer |
| Peer code review | Before merge to main branch | PM or second developer |
| Manual security review | At end of Quality Hardening phase | PM + external reviewer |
| Device/browser testing | End of each execution phase | QA |
| UAT script execution | UAT phase | PM + 5 field workers + 2 managers |
| Payroll cross-check | First live payroll run | Grace Liu (Payroll Admin) |

---

## 6. RISK MANAGEMENT

### 6.1 Risk Register

| ID | Risk Description | Prob | Impact | Strategy | Mitigation / Contingency | Owner |
|----|------------------|------|--------|----------|--------------------------|-------|
| R1 | Browser denies camera/geolocation permission | High | High | Mitigate | Clear in-app instructions with screenshots; detect permission state and show guidance; fallback: manual photo upload + manual lat/lng entry in admin panel | Lead Dev |
| R2 | GPS accuracy poor (urban canyon, indoors, remote sites) | Med | High | Mitigate | Use device-reported accuracy value; if > 50m, warn worker and flag record for manager review; set generous default radius (100m) that admin can tighten per site | Lead Dev |
| R3 | Worker forgets to clock out (open-ended shift) | High | Med | Mitigate | Admin dashboard shows all open clock-ins; auto-email reminder at configurable time (e.g. 10 hours after clock-in); admin can manually close with a note | PM / Lead Dev |
| R4 | Server disk space exhausted by image uploads | Med | High | Mitigate | Compress images to max 800px wide on upload; set auto-delete policy for images older than 90 days (configurable); disk space monitoring alert at 80% | IT / Sam Patel |
| R5 | Email delivery fails (spam, host limits, misconfiguration) | Med | High | Mitigate | Use dedicated SMTP relay service (Mailgun/SendGrid free tier sufficient); log all email attempts; admin notification if email queue backs up; records always viewable in dashboard regardless of email | Lead Dev / Sam Patel |
| R6 | WordPress core update breaks plugin | Low | High | Accept (with contingency) | Plugin uses standard WP APIs and hooks only; test against WP beta releases; maintainer on standby for emergency fix; have rollback plan | Lead Dev |
| R7 | Developer unavailability (sole developer risk) | Low | Critical | Mitigate | Code committed daily; comprehensive inline documentation; architecture document kept updated; identified backup developer (contact details on file) | PM |

### 6.2 Risk Review
- Risk register reviewed at weekly PM/Dev check-in (Fridays)
- New risks added within 24 hours of identification
- Sponsor briefed on Top 3 risks at monthly update

---

## 7. COMMUNICATION MANAGEMENT

| Audience | Information | Frequency | Method | Owner |
|----------|-------------|-----------|--------|-------|
| Sponsor (James) | Budget, schedule, key risks, decisions needed | Monthly (1st Thurs) | 30-min video call + 1-page status slide | PM |
| Lead Developer | Task priorities, blockers, technical decisions | Mon/Wed/Fri | Slack async stand-up + weekly 30-min call | PM |
| Payroll Admin (Grace) | CSV format, email content, testing schedule | As needed + UAT phase | Email + UAT walkthrough | PM |
| Site Managers (UAT) | UAT schedule, what to test, feedback form | 2 weeks before UAT, then daily during | Email + on-site visit (Day 1) | PM |
| Field Workers (UAT) | Simple instructions, what to do, who to call | Day before UAT | Printed 1-page guide + on-site 10-min briefing | PM + Site Manager |
| IT (Sam) | Server requirements, SSL, SMTP, disk space | Initiation + pre-deployment | Email + setup checklist | PM |

---

## 8. PHASE GATES

| Gate | When | Criteria to Pass | Decision Maker |
|------|------|------------------|----------------|
| Gate 1: Initiation → Planning | 14/07/26 | Charter signed, technical feasibility confirmed, budget approved, dev environment ready | James (Sponsor) |
| Gate 2: Planning → Execution | 21/07/26 | Wireframes approved, architecture doc reviewed, test plan agreed, risk register populated | PM + Lead Dev |
| Gate 3: Core → Full Build | 25/08/26 | Clock-in/out working end-to-end (camera → GPS → save → display); distance calc verified | PM + Lead Dev |
| Gate 4: Full Build → Alpha | 20/10/26 | All features functional on staging; PHPCS clean; security review done; i18n complete | PM |
| Gate 5: Alpha → UAT | 27/10/26 | Zero critical bugs; all 8 core test cases pass internally | PM |
| Gate 6: UAT → Go-Live | 17/11/26 | UAT sign-off from Sponsor + Payroll Admin; all critical & high bugs fixed | James (Sponsor) |
| Gate 7: Go-Live → Closure | 27/11/26 | First payroll run verified; documentation delivered; lessons learned complete | James (Sponsor) |

---

## 9. CLOSURE CHECKLIST

| Item | Status | Notes |
|------|--------|-------|
| Plugin deployed to production | ☐ | Activated, configured, shortcode page published |
| First payroll run cross-checked | ☐ | Grace Liu confirms data matches within tolerance |
| User documentation delivered | ☐ | Admin guide (PDF) + Worker one-pager (laminated for site shed) |
| Source code in company repository | ☐ | GitHub private repo, admin access for Sam/IT |
| Lessons learned workshop held | ☐ | PM facilitates; notes distributed to all stakeholders |
| Final budget reconciliation | ☐ | Actuals vs. baseline; variance explained |
| Contingency remaining | ☐ | Returned to budget or allocated to Phase 2 features |
| Maintenance plan agreed | ☐ | Who handles bugs? Emergency contact? WP update testing cadence? |
| Team formally released | ☐ | Thank-you from Sponsor |
| Project report submitted | ☐ | Includes: objectives met, final budget, lessons learned, Phase 2 recommendations |

---

## APPENDIX: CHANGE REQUEST FORM

| Field | Details |
|-------|---------|
| **Change Request ID** | CR-[###] |
| **Date Requested** | |
| **Requested By** | |
| **Description of Change** | |
| **Reason** | |
| **Impact Assessment** | |
| — Schedule | |
| — Cost | |
| — Quality | |
| — Risk | |
| **Recommendation** | |
| **Decision** | ☐ Approved ☐ Rejected ☐ Deferred |
| **Approved By** | |
| **Date** | |

---

## APPENDIX: TEST CASE SUMMARY (8 Core Test Cases)

| TC# | Scenario | Expected Result |
|-----|----------|-----------------|
| TC1 | Worker clocks IN: camera captures, GPS valid, within geofence | Record saved with IN status; success message displayed |
| TC2 | Worker clocks OUT after TC1: camera captures, GPS valid | Record saved with OUT status; hours calculated; email sent to 3 recipients |
| TC3 | Worker clocks IN but GPS is outside geofence radius | Record saved but flagged "OUT OF RANGE"; warning shown; admin notified |
| TC4 | Worker tries to clock IN twice without clocking OUT | Second IN blocked; message: "You already have an open shift. Clock out first." |
| TC5 | Worker clocks OUT at midnight (overnight shift edge case) | OUT correctly paired with previous day's IN if within configurable shift window |
| TC6 | Admin exports CSV for Member-ID "GREEN-042", date range 01/11–14/11 | CSV contains correct rows, columns match payroll spec, total hours match dashboard |
| TC7 | Manager views team dashboard | Sees only own team members; open shifts highlighted; cannot access settings |
| TC8 | Email received by all 3 recipients on clock-out | Member gets shift summary; Manager gets summary; Admin gets summary; all within 60s |

---

This gives you a complete, ready-to-customise plan for the WP Punch Clock Plugin. Would you like me to expand any section further — such as the technical architecture document, detailed wireframe descriptions, or the UAT script?
