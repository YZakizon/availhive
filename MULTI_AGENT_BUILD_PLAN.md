# AvailHive Multi-Agent Build Plan

## Suggested filename

Use this file name:

```bash
MULTI_AGENT_BUILD_PLAN.md
```

This file explains how to build AvailHive with a multi-agent workflow, how to divide responsibilities, how to sequence the work, and how to avoid agents overwriting each other or duplicating logic.

---

# 1. Project goal

AvailHive is a mobile-first, multi-provider booking and availability management SaaS.

Core product promise:

> Turn availability into bookings.

The system must support:

- Multi-provider scheduling
- Mobile-first client booking flows
- Workspace/multi-tenant architecture
- Provider-specific and any-provider booking
- Calendar integrations
- Appointment management
- Waitlist automation
- Billing, Stripe, and pricing plans
- Appointment payments
- Blog/CMS
- Analytics and reporting
- Platform owner admin
- Security, audit logs, testing, and deployment

---

# 2. Multi-agent strategy

Use specialized agents with clear ownership.

Do not let every agent edit every part of the app. Each agent should own one domain, follow shared contracts, and request architecture changes through the Lead Architect Agent.

The most important rule:

```text
One agent owns one domain. Shared contracts require Lead Architect review.
```

---

# 3. Recommended agents

## 3.1 Lead Architect Agent

Owns overall architecture, sequencing, code review, and shared contracts.

Responsibilities:

- Read `AGENTS.md`, `CODEX_PLAN.md`, `CODEX_RULES.md`, and `UI_DESIGN.md` first.
- Maintain the canonical project architecture.
- Define shared database entities.
- Define route structure.
- Define API contracts.
- Prevent duplicate implementations.
- Review PRs from all other agents.
- Enforce workspace isolation.
- Enforce multi-provider support.
- Enforce provider context in booking logic.
- Enforce mobile-first UI rules.
- Enforce separation of SaaS billing and client appointment payments.
- Resolve conflicts between agents.

This agent should be first and last in every major phase.

---

## 3.2 Database / Backend Foundation Agent

Owns schema, migrations, seed data, multi-tenancy, and core backend models.

Responsibilities:

- Create database schema.
- Add core entities:
  - Workspace
  - User
  - Membership
  - Role
  - Permission
  - Provider
  - Service
  - AvailabilityRule
  - Appointment
  - Client
  - WaitlistEntry
  - Plan
  - Subscription
  - Payment
  - AuditLog
- Enforce `workspaceId` on all workspace-owned entities.
- Add indexes for scheduling queries.
- Add seed data for demo workspace.
- Create repository/service layer patterns.
- Add migration scripts.

Do not build UI.

---

## 3.3 Auth and Permissions Agent

Owns authentication, workspace membership, roles, access control, and platform admin permissions.

Responsibilities:

- Implement signup.
- Implement login/logout.
- Implement password reset.
- Implement email verification if needed.
- Implement workspace creation during onboarding.
- Implement workspace invitations.
- Implement role-based access control.
- Implement provider/staff/admin/billing admin permissions.
- Implement platform owner admin permissions separately from workspace permissions.
- Add authorization middleware.
- Add permission tests.

Critical rule:

```text
Platform admin is not the same as workspace admin.
Workspace admins can manage only their workspace.
Platform admins access platform routes only.
```

---

## 3.4 Scheduling Engine Agent

Owns availability calculation, provider selection, booking rules, time zones, and double-booking prevention.

Responsibilities:

- Compute availability per provider.
- Support specific-provider booking.
- Support any-available-provider booking.
- Support service-provider eligibility.
- Respect working hours.
- Respect breaks.
- Respect holidays.
- Respect buffers.
- Respect minimum notice.
- Respect maximum booking window.
- Prevent double-booking.
- Handle client/provider/business time zones.
- Support manual blocks and date overrides.
- Add unit tests for availability logic.

This is one of the most important agents. It should not also build UI.

---

## 3.5 Booking Flow Agent

Owns the public client booking experience.

Responsibilities:

- Build mobile-first public booking page.
- Build service selection.
- Build provider selection.
- Build date/time slot picker.
- Build client details form.
- Build intake questions.
- Build confirmation page.
- Build reschedule flow.
- Build cancel flow.
- Add “join waitlist” when no slots are available.
- Use scheduling engine APIs only.
- Do not duplicate availability logic in the UI.

Critical rule:

```text
The booking page must support both “choose provider” and “any available provider.”
```

---

## 3.6 Provider and Services Agent

Owns provider management, service management, and assignment logic UI.

Responsibilities:

- Build provider list.
- Build provider profile.
- Add provider invite/edit/deactivate.
- Add provider availability settings.
- Add provider calendar connection status.
- Build service list.
- Build service editor.
- Assign services to providers.
- Configure duration.
- Configure price.
- Configure buffers.
- Configure location type.
- Configure intake questions.
- Configure payment rules.

---

## 3.7 Admin Dashboard Agent

Owns the workspace admin app shell and dashboard pages.

Responsibilities:

- Build responsive admin layout.
- Build sidebar navigation.
- Build dashboard summary cards.
- Build upcoming appointments section.
- Build provider workload section.
- Build availability overview.
- Build recent activity feed.
- Build empty states.
- Ensure admin views are responsive on mobile, tablet, and desktop.

This agent should consume backend data but not create business logic directly.

---

## 3.8 Appointments and Clients Agent

Owns appointment management and client CRM.

Responsibilities:

- Build appointments list/table.
- Add filters by provider, service, date, status, and payment status.
- Build appointment detail drawer.
- Add actions:
  - Reschedule
  - Cancel
  - Reassign provider
  - Mark completed
  - Mark no-show
  - Send reminder
- Build client list.
- Build client profile.
- Show appointment history.
- Show waitlist history.
- Show notes and tags.
- Add CSV export if in scope.

---

## 3.9 Calendar Integration Agent

Owns Google/Outlook calendar sync.

Responsibilities:

- Implement calendar OAuth.
- Connect provider-specific calendars.
- Let provider choose calendars to check for conflicts.
- Let provider choose calendar to write appointments to.
- Create external calendar events after booking.
- Update events after reschedule.
- Cancel/remove events after cancellation.
- Handle reconnect flow.
- Log sync failures.
- Add tests around calendar event creation/update.

This agent depends on the scheduling engine and appointment model.

---

## 3.10 Notification Agent

Owns emails, SMS, reminders, and templates.

Responsibilities:

- Booking confirmation emails.
- Provider notification emails.
- Reschedule emails.
- Cancellation emails.
- Reminder jobs.
- Waitlist offer emails/SMS.
- Payment receipt messages.
- Payment failure messages.
- Provider invitation emails.
- Notification templates.
- Template variables.
- Test-send flow.
- Background job scheduling.

Keep notifications separate from appointment business logic.

Appointment logic should emit events. The notification system should handle delivery.

---

## 3.11 Waitlist Automation Agent

Owns waitlist matching and cancellation fill logic.

Responsibilities:

- Build waitlist data model if not already created.
- Build waitlist list and detail UI.
- Support provider-specific waitlists.
- Support any-provider waitlists.
- Match waitlist entries to open slots.
- Suggest waitlist candidates after cancellation.
- Offer slot manually.
- Auto-offer slot if enabled.
- Add offer expiration.
- Convert accepted offer into appointment.
- Respect service, provider, location, time preferences, and plan limits.

Important dependency:

```text
This agent must use the scheduling engine. It must not create its own availability calculation.
```

---

## 3.12 Billing and Stripe Agent

Owns SaaS subscriptions, plan changes, feature gates, and Stripe webhooks.

Responsibilities:

- Create Plan model.
- Create Subscription model.
- Create FeatureEntitlement/UsageLimit logic.
- Implement Stripe Checkout.
- Implement Stripe Billing.
- Implement Stripe Customer Portal.
- Upgrade/downgrade/cancel/reactivate subscription.
- Handle monthly/yearly plans.
- Handle trialing, active, past_due, cancelled states.
- Implement Stripe webhook handlers.
- Make webhooks idempotent.
- Verify webhook signatures.
- Build billing settings UI.
- Build pricing page data integration.

Critical rule:

```text
Do not mix SaaS subscription billing with client appointment payments.
```

---

## 3.13 Appointment Payments Agent

Owns businesses collecting money from clients during booking.

Responsibilities:

- Add service-level pricing.
- Add deposit/full payment/pay-later rules.
- Implement Stripe PaymentIntent or Checkout for appointment payments.
- Mark appointment payment status.
- Prevent confirmed paid bookings before required payment succeeds.
- Handle failed payment.
- Handle refunds.
- Show payment status in appointment detail.
- Add payment settings screen.

This should be separate from subscription billing.

---

## 3.14 Marketing Site and CMS Agent

Owns the public marketing website, blog, and content management.

Responsibilities:

- Homepage.
- Features page.
- Pricing page.
- Use-case pages.
- Blog index.
- Blog detail page.
- Help center.
- Case studies.
- CMS admin.
- Draft/published/scheduled states.
- SEO metadata.
- Slugs.
- Categories/tags.
- Author profiles.

Rule:

```text
CMS must stay separate from scheduling, appointment, provider, billing, and waitlist logic.
```

---

## 3.15 Analytics and Reporting Agent

Owns reporting dashboards, exports, and metrics.

Responsibilities:

- Booking volume.
- Provider utilization.
- Cancellation rate.
- No-show rate.
- Revenue reports.
- Waitlist conversion.
- Most-booked services.
- Source/referral tracking.
- CSV exports.
- Dashboard charts.

This agent should only read normalized business data. It should not mutate scheduling logic.

---

## 3.16 Platform Owner Admin Agent

Owns the SaaS operator admin area.

Responsibilities:

- Platform admin dashboard.
- Workspace search.
- Workspace detail views.
- Subscription overview.
- Usage overview.
- Integration health.
- Failed job views.
- Failed webhook views.
- Suspend/reactivate workspace.
- Feature flags.
- Platform audit logs.

Critical rule:

```text
Platform owner admin is separate from customer workspace admin.
```

---

## 3.17 QA / Testing Agent

Owns automated tests, E2E tests, and regression testing.

Responsibilities:

- Unit tests for scheduling engine.
- Permission tests.
- Workspace isolation tests.
- Booking flow E2E tests.
- Reschedule/cancel E2E tests.
- Waitlist offer E2E tests.
- Stripe webhook tests.
- Calendar sync tests.
- Mobile responsive tests.
- Downgrade/plan-limit tests.

This agent should run throughout the project, not only at the end.

---

## 3.18 DevOps / Deployment Agent

Owns environments, CI/CD, secrets, monitoring, and deployments.

Responsibilities:

- Local/staging/production environment setup.
- Environment variables.
- Database migrations.
- Seed data.
- CI pipeline.
- Test pipeline.
- Error monitoring.
- Logging.
- Backups.
- Health checks.
- Stripe test/live config.
- Calendar OAuth config.
- Email/SMS provider config.

---

# 4. Best build order

Do not start all agents at once. Use waves.

---

## Wave 0 — Planning and contracts

Agents involved:

- Lead Architect Agent
- Database/Foundation Agent
- Auth Agent
- Scheduling Engine Agent
- UI Design Agent if separate

Deliverables:

- Final route map
- Final database schema
- Entity relationships
- Permission matrix
- API contracts
- Event names
- Shared types
- Coding conventions

Nothing else should start until this is stable.

---

## Wave 1 — Foundation

Agents involved:

- Database/Foundation Agent
- Auth and Permissions Agent
- DevOps Agent
- QA Agent

Build:

- Project setup
- Database schema
- Migrations
- Authentication
- Workspace model
- Roles and permissions
- Basic app shell
- Test setup
- CI basics

Exit criteria:

- User can sign up.
- User can create workspace.
- Workspace isolation works.
- Roles exist.
- Tests run.

---

## Wave 2 — Core scheduling MVP

Agents involved:

- Scheduling Engine Agent
- Provider and Services Agent
- Booking Flow Agent
- Admin Dashboard Agent
- Appointments and Clients Agent
- QA Agent

Build:

- Providers
- Services
- Availability
- Booking page
- Appointment creation
- Admin dashboard
- Appointment list/detail
- Client records

Exit criteria:

- Client can book with a specific provider.
- Client can book with any available provider.
- Admin can view appointments.
- Provider availability is respected.
- Double booking is prevented.
- Mobile booking flow works.

---

## Wave 3 — Calendar sync and notifications

Agents involved:

- Calendar Integration Agent
- Notification Agent
- Scheduling Engine Agent
- QA Agent

Build:

- Google Calendar
- Outlook Calendar
- Calendar conflict detection
- Booking confirmation emails
- Reminders
- Cancellation/reschedule notifications

Exit criteria:

- Connected provider calendars block unavailable times.
- Bookings create calendar events.
- Reschedules update calendar events.
- Cancellations update/cancel calendar events.
- Notifications send correctly.

---

## Wave 4 — Waitlist automation

Agents involved:

- Waitlist Automation Agent
- Booking Flow Agent
- Notification Agent
- Scheduling Engine Agent
- QA Agent

Build:

- Waitlist join flow
- Waitlist dashboard
- Provider-specific waitlists
- Any-provider waitlists
- Slot matching
- Offer slot
- Auto-fill cancellations

Exit criteria:

- Client can join waitlist.
- Admin can offer a slot.
- Client can accept slot.
- Accepted waitlist offer creates appointment.
- Automation respects provider, service, location, and availability rules.

---

## Wave 5 — Billing and plans

Agents involved:

- Billing and Stripe Agent
- Appointment Payments Agent
- UI Agent
- QA Agent

Build:

- Pricing plans
- Feature gates
- Stripe subscriptions
- Plan changes
- Billing portal
- Appointment payments
- Deposits
- Refund tracking

Exit criteria:

- Workspace can subscribe.
- Workspace can upgrade/downgrade/cancel.
- Plan limits apply.
- Stripe webhooks update local state.
- Client can pay for appointment if required.

---

## Wave 6 — Marketing, CMS, analytics

Agents involved:

- Marketing Site and CMS Agent
- Analytics Agent
- UI Agent
- QA Agent

Build:

- Homepage
- Pricing page
- Blog
- CMS admin
- Help center
- Reports
- CSV exports

Exit criteria:

- Marketing site is live.
- Blog posts can be created and published.
- Reports show useful booking/provider/waitlist/payment data.

---

## Wave 7 — Platform owner admin and launch readiness

Agents involved:

- Platform Owner Admin Agent
- Security Agent if separate
- DevOps Agent
- QA Agent
- Lead Architect Agent

Build:

- Platform admin dashboard
- Workspace search
- Billing overview
- Usage overview
- Integration health
- Failed jobs
- Feature flags
- Workspace suspension/reactivation
- Audit logs
- Monitoring
- Production deployment

Exit criteria:

- Platform owner can manage customers.
- Support can inspect operational issues.
- Platform actions are audit logged.
- Production environment is stable.

---

# 5. Suggested folder ownership

Example structure:

```text
/apps/web
  /app
    /(marketing)        -> Marketing/CMS Agent
    /(auth)             -> Auth Agent
    /(booking)          -> Booking Flow Agent
    /(dashboard)        -> Admin Dashboard Agent
    /(platform-admin)   -> Platform Owner Admin Agent

/packages
  /db                   -> Database/Foundation Agent
  /auth                 -> Auth Agent
  /permissions          -> Auth Agent
  /scheduling           -> Scheduling Engine Agent
  /billing              -> Billing Agent
  /payments             -> Appointment Payments Agent
  /calendar             -> Calendar Integration Agent
  /notifications        -> Notification Agent
  /analytics            -> Analytics Agent
  /ui                   -> UI Agent
  /config               -> DevOps/Lead Architect Agent

/tests
  /unit                 -> QA Agent
  /integration          -> QA Agent
  /e2e                  -> QA Agent
```

Ownership map:

```text
Lead Architect:
- /docs
- shared architecture decisions
- cross-package reviews

Database Agent:
- /packages/db
- migrations
- schema
- seed data

Scheduling Agent:
- /packages/scheduling
- availability tests

Auth Agent:
- /packages/auth
- /packages/permissions
- auth routes

Booking Agent:
- /apps/web/app/(booking)
- booking-specific components

Admin Agent:
- /apps/web/app/(dashboard)
- dashboard components

Billing Agent:
- /packages/billing
- /apps/web/app/(dashboard)/billing

Platform Admin Agent:
- /apps/web/app/(platform-admin)
- /packages/platform-admin

QA Agent:
- /tests
- E2E specs
- test fixtures

DevOps Agent:
- CI/CD
- deployment config
- environment templates
```

---

# 6. Shared contracts every agent must follow

## 6.1 Entity ownership contract

Workspace-owned entities must include `workspaceId`:

- Provider
- Service
- BookingPage
- Appointment
- Client
- WaitlistEntry
- CalendarConnection
- NotificationTemplate
- Payment
- Subscription
- AuditLog

---

## 6.2 Provider context contract

All appointment and availability logic must include provider context.

A booking may be:

- `specificProvider`
- `anyAvailableProvider`

Services may be assigned to:

- One provider
- Many providers
- All providers

---

## 6.3 Permission contract

Every protected action must check:

- Authenticated user
- Workspace membership
- Role/permission
- Entity `workspaceId`
- Provider ownership if provider-scoped

---

## 6.4 Billing contract

Subscription billing and appointment payments are separate.

Subscription billing:

- Workspace pays AvailHive.

Appointment payments:

- Client pays business for appointment.

---

## 6.5 UI contract

All client-facing flows are mobile-first.

Admin views are responsive.

Tables must degrade to mobile cards or horizontal scroll.

Calendar views need list/agenda alternatives on mobile.

---

## 6.6 Event contract

Use domain events so agents do not tightly couple systems:

```text
appointment.created
appointment.rescheduled
appointment.cancelled
appointment.completed
appointment.no_show
waitlist.entry_created
waitlist.slot_offered
waitlist.offer_accepted
payment.succeeded
payment.failed
subscription.updated
calendar.sync_failed
provider.invited
```

Notification and analytics agents can listen to these events instead of embedding logic everywhere.

---

# 7. Example task distribution board

## Epic A — Foundation

- A1. Create database schema
- A2. Add workspace model
- A3. Add user model
- A4. Add role/permission model
- A5. Add provider model
- A6. Add service model
- A7. Add appointment model
- A8. Add waitlist model
- A9. Add subscription/payment models
- A10. Add audit log model

## Epic B — Auth and workspace

- B1. Signup
- B2. Login/logout
- B3. Workspace creation
- B4. Invite team member
- B5. Accept invite
- B6. Role management
- B7. Provider login permissions
- B8. Platform admin auth

## Epic C — Scheduling

- C1. Provider working hours
- C2. Service duration rules
- C3. Buffer rules
- C4. Time zone handling
- C5. Specific-provider availability
- C6. Any-provider availability
- C7. Double-booking prevention
- C8. Unit tests

## Epic D — Booking

- D1. Public booking page
- D2. Service selection
- D3. Provider selection
- D4. Date/time picker
- D5. Client details form
- D6. Intake questions
- D7. Confirmation page
- D8. Reschedule/cancel pages
- D9. Mobile QA

## Epic E — Admin

- E1. Dashboard shell
- E2. Provider management
- E3. Service management
- E4. Appointment list
- E5. Appointment details
- E6. Client list
- E7. Client profile
- E8. Settings

## Epic F — Calendar

- F1. Google OAuth
- F2. Outlook OAuth
- F3. Calendar conflict checking
- F4. Calendar event creation
- F5. Calendar event update
- F6. Calendar reconnect flow
- F7. Sync failure logging

## Epic G — Notifications

- G1. Email provider setup
- G2. Booking confirmation
- G3. Appointment reminders
- G4. Cancellation emails
- G5. Reschedule emails
- G6. Provider invites
- G7. Waitlist offer notifications
- G8. Template editor

## Epic H — Waitlist

- H1. Join waitlist form
- H2. Waitlist admin screen
- H3. Provider-specific waitlist
- H4. Any-provider waitlist
- H5. Slot matching
- H6. Manual offer
- H7. Auto-offer
- H8. Accept offer creates appointment

## Epic I — Billing

- I1. Plan model
- I2. Feature gates
- I3. Stripe Checkout
- I4. Stripe Customer Portal
- I5. Upgrade flow
- I6. Downgrade flow
- I7. Cancel/reactivate flow
- I8. Stripe webhooks
- I9. Invoice history

## Epic J — Appointment payments

- J1. Service price settings
- J2. Deposit settings
- J3. Payment step in booking
- J4. Payment status on appointment
- J5. Refund tracking
- J6. Failed payment handling

## Epic K — CMS and marketing

- K1. Homepage
- K2. Pricing page
- K3. Features page
- K4. Blog index
- K5. Blog post page
- K6. CMS admin
- K7. SEO metadata
- K8. Help center

## Epic L — Platform owner admin

- L1. Platform admin layout
- L2. Workspace search
- L3. Workspace detail page
- L4. Subscription overview
- L5. Usage overview
- L6. Integration health
- L7. Failed webhook/job views
- L8. Suspend/reactivate workspace
- L9. Feature flags
- L10. Platform audit logs

## Epic M — Testing and deployment

- M1. Unit tests
- M2. Integration tests
- M3. E2E booking tests
- M4. E2E billing tests
- M5. Workspace isolation tests
- M6. Mobile responsive tests
- M7. CI/CD
- M8. Staging deployment
- M9. Production deployment

---

# 8. How to prevent multi-agent chaos

## Rule 1: One agent owns one domain

Bad:

```text
Booking agent modifies billing.
Billing agent modifies scheduling.
CMS agent modifies provider schema.
```

Good:

```text
Agent asks Lead Architect to change shared contracts.
The owning agent updates the shared package.
```

---

## Rule 2: Shared packages require Lead Architect review

Any change to these requires review:

- Database schema
- Scheduling engine
- Permissions
- Billing state
- Domain events
- Shared UI components
- API contracts

---

## Rule 3: Each agent works from a written task brief

Every task should include:

```text
Task:
Owner:
Files allowed:
Files not allowed:
Inputs:
Outputs:
Acceptance criteria:
Tests required:
Dependencies:
```

Example:

```text
Task: Build provider-specific availability calculation

Owner:
Scheduling Engine Agent

Files allowed:
- packages/scheduling/**
- packages/db/schema provider/availability references
- tests/scheduling/**

Files not allowed:
- apps/web/app/(booking)/**
- packages/billing/**
- packages/notifications/**

Inputs:
- Provider working hours
- Service duration
- Existing appointments
- Calendar conflicts
- Buffers
- Time zone

Outputs:
- getProviderAvailability()
- getAnyProviderAvailability()
- tests

Acceptance criteria:
- Prevents double booking
- Respects buffers
- Respects time zones
- Supports specific provider
- Supports any available provider
```

---

## Rule 4: Merge in vertical slices

A good vertical slice:

```text
Provider + Service + Availability + Booking Page + Appointment Creation
```

A bad slice:

```text
Build every dashboard page with fake data.
```

---

## Rule 5: Add tests before expanding features

Most dangerous areas:

- Availability engine
- Time zones
- Provider assignment
- Workspace isolation
- Stripe webhooks
- Calendar sync
- Waitlist matching
- Permissions

These need tests early.

---

# 9. Best MVP cut

For the first working version, build only this:

- Auth
- Workspace
- Roles
- Providers
- Services
- Provider availability
- Booking page
- Specific-provider booking
- Any-provider booking
- Appointment creation
- Basic admin dashboard
- Basic email confirmation
- Mobile-first booking flow

Do not start with:

- CMS
- Analytics
- AI
- Complex platform admin
- Advanced reporting
- Full appointment payments
- Advanced waitlist automation

Those are important, but not first.

The first end-to-end milestone should be:

```text
A business signs up, creates a workspace, adds a provider, creates a service, sets availability, publishes a booking page, and a client books an appointment from mobile.
```

---

# 10. Recommended agent prompts

## 10.1 Lead Architect Agent prompt

```text
You are the Lead Architect Agent for AvailHive, a mobile-first, multi-provider booking SaaS.

Read AGENTS.md, CODEX_PLAN.md, CODEX_RULES.md, UI_DESIGN.md, and MULTI_AGENT_BUILD_PLAN.md before making changes.

Your responsibilities:
- Maintain the canonical architecture.
- Enforce workspace isolation.
- Enforce multi-provider support.
- Enforce mobile-first UI rules.
- Enforce separation between SaaS billing and appointment payments.
- Review shared schema, permission, scheduling, billing, and API changes.
- Prevent agents from duplicating logic.
- Keep implementation aligned with the phase plan.

Do not implement isolated features unless they affect architecture or shared contracts.
```

## 10.2 Backend Foundation Agent prompt

```text
You are the Backend Foundation Agent for AvailHive.

Own:
- Database schema
- Migrations
- Seed data
- Core models
- Workspace isolation
- Shared repositories

Do not build UI.

Every workspace-owned model must include workspaceId.
Never create appointment, provider, service, waitlist, billing, or client data without workspace context.
Coordinate schema changes with the Lead Architect Agent.
```

## 10.3 Scheduling Engine Agent prompt

```text
You are the Scheduling Engine Agent for AvailHive.

Own:
- Availability calculation
- Provider eligibility
- Time zone handling
- Buffers
- Booking windows
- Double-booking prevention
- Specific-provider and any-provider scheduling

Do not build UI.
Do not implement billing.
Do not implement notifications.

All scheduling logic must support multiple providers.
Never assume one provider per workspace.
Add unit tests for every scheduling rule.
```

## 10.4 Booking Flow Agent prompt

```text
You are the Booking Flow Agent for AvailHive.

Own:
- Mobile-first public booking pages
- Service selection
- Provider selection
- Date/time picker
- Client details
- Intake questions
- Confirmation
- Reschedule/cancel pages
- Waitlist entry point

Use scheduling APIs.
Do not duplicate availability logic in the frontend.
Support specific-provider and any-available-provider booking.
Client-facing flows must be excellent on mobile.
```

## 10.5 Billing Agent prompt

```text
You are the Billing and Stripe Agent for AvailHive.

Own:
- SaaS subscription plans
- Stripe Checkout
- Stripe Billing
- Stripe Customer Portal
- Plan upgrades/downgrades/cancellations
- Subscription webhooks
- Feature gates
- Invoice history

Do not implement client appointment payments unless explicitly assigned.
Do not store raw card data.
Verify Stripe webhook signatures.
Make webhooks idempotent.
Treat Stripe as source of truth for subscription state.
```

## 10.6 Platform Admin Agent prompt

```text
You are the Platform Owner Admin Agent for AvailHive.

Own:
- Platform admin dashboard
- Workspace search
- Workspace detail views
- Subscription overview
- Usage overview
- Integration health
- Failed job/webhook views
- Suspend/reactivate workspace
- Feature flags
- Platform audit logs

Platform admin is separate from workspace admin.
Do not expose platform routes to customer workspace admins.
Every platform admin action must be audit logged.
Minimize sensitive customer data shown in support views.
```

---

# 11. Command-line setup

Use these commands to add the planning files to a project repository.

## 11.1 Create a docs folder and copy planning files

```bash
mkdir -p docs
cp AGENTS.md docs/AGENTS.md
cp CODEX_PLAN.md docs/CODEX_PLAN.md
cp CODEX_RULES.md docs/CODEX_RULES.md
cp UI_DESIGN.md docs/UI_DESIGN.md
cp MULTI_AGENT_BUILD_PLAN.md docs/MULTI_AGENT_BUILD_PLAN.md
```

If the files are already in the repository root and you want to keep them there, use:

```bash
git add AGENTS.md CODEX_PLAN.md CODEX_RULES.md UI_DESIGN.md MULTI_AGENT_BUILD_PLAN.md
git commit -m "Add AvailHive product and multi-agent build plans"
```

If you moved them into `docs/`, use:

```bash
git add docs/AGENTS.md docs/CODEX_PLAN.md docs/CODEX_RULES.md docs/UI_DESIGN.md docs/MULTI_AGENT_BUILD_PLAN.md
git commit -m "Add AvailHive product and multi-agent build plans"
```

---

## 11.2 Create recommended monorepo folders

```bash
mkdir -p apps/web/app/{'(marketing)','(auth)','(booking)','(dashboard)','(platform-admin)'}
mkdir -p packages/{db,auth,permissions,scheduling,billing,payments,calendar,notifications,analytics,ui,config}
mkdir -p tests/{unit,integration,e2e}
mkdir -p docs/agents
```

If your shell has trouble with parentheses, use quoted paths:

```bash
mkdir -p "apps/web/app/(marketing)"
mkdir -p "apps/web/app/(auth)"
mkdir -p "apps/web/app/(booking)"
mkdir -p "apps/web/app/(dashboard)"
mkdir -p "apps/web/app/(platform-admin)"
mkdir -p packages/db packages/auth packages/permissions packages/scheduling packages/billing packages/payments packages/calendar packages/notifications packages/analytics packages/ui packages/config
mkdir -p tests/unit tests/integration tests/e2e docs/agents
```

---

## 11.3 Create agent brief files

```bash
cat > docs/agents/lead-architect.md <<'AGENT'
# Lead Architect Agent

You are the Lead Architect Agent for AvailHive, a mobile-first, multi-provider booking SaaS.

Read:
- docs/AGENTS.md
- docs/CODEX_PLAN.md
- docs/CODEX_RULES.md
- docs/UI_DESIGN.md
- docs/MULTI_AGENT_BUILD_PLAN.md

Responsibilities:
- Maintain canonical architecture.
- Enforce workspace isolation.
- Enforce multi-provider support.
- Enforce mobile-first UI rules.
- Enforce separation between SaaS billing and appointment payments.
- Review shared schema, permission, scheduling, billing, and API changes.
- Prevent duplicate logic.
AGENT

cat > docs/agents/backend-foundation.md <<'AGENT'
# Backend Foundation Agent

Own:
- Database schema
- Migrations
- Seed data
- Core models
- Workspace isolation
- Shared repositories

Rules:
- Do not build UI.
- Every workspace-owned model must include workspaceId.
- Coordinate schema changes with the Lead Architect Agent.
AGENT

cat > docs/agents/scheduling-engine.md <<'AGENT'
# Scheduling Engine Agent

Own:
- Availability calculation
- Provider eligibility
- Time zone handling
- Buffers
- Booking windows
- Double-booking prevention
- Specific-provider and any-provider scheduling

Rules:
- Do not build UI.
- Do not implement billing.
- Do not implement notifications.
- Never assume one provider per workspace.
- Add unit tests for every scheduling rule.
AGENT

cat > docs/agents/booking-flow.md <<'AGENT'
# Booking Flow Agent

Own:
- Mobile-first public booking pages
- Service selection
- Provider selection
- Date/time picker
- Client details
- Intake questions
- Confirmation
- Reschedule/cancel pages
- Waitlist entry point

Rules:
- Use scheduling APIs.
- Do not duplicate availability logic in the frontend.
- Support specific-provider and any-available-provider booking.
AGENT

cat > docs/agents/billing-stripe.md <<'AGENT'
# Billing and Stripe Agent

Own:
- SaaS subscription plans
- Stripe Checkout
- Stripe Billing
- Stripe Customer Portal
- Plan upgrades/downgrades/cancellations
- Subscription webhooks
- Feature gates
- Invoice history

Rules:
- Do not implement client appointment payments unless explicitly assigned.
- Do not store raw card data.
- Verify Stripe webhook signatures.
- Make webhooks idempotent.
- Treat Stripe as source of truth for subscription state.
AGENT

cat > docs/agents/platform-admin.md <<'AGENT'
# Platform Owner Admin Agent

Own:
- Platform admin dashboard
- Workspace search
- Workspace detail views
- Subscription overview
- Usage overview
- Integration health
- Failed job/webhook views
- Suspend/reactivate workspace
- Feature flags
- Platform audit logs

Rules:
- Platform admin is separate from workspace admin.
- Do not expose platform routes to customer workspace admins.
- Every platform admin action must be audit logged.
AGENT
```

Commit the agent briefs:

```bash
git add docs/agents
git commit -m "Add multi-agent role briefs"
```

---

## 11.4 Create branch naming convention

Use one branch per agent/domain:

```bash
git checkout -b agent/foundation-schema
git checkout -b agent/auth-permissions
git checkout -b agent/scheduling-engine
git checkout -b agent/booking-flow
git checkout -b agent/provider-services
git checkout -b agent/admin-dashboard
git checkout -b agent/calendar-sync
git checkout -b agent/notifications
git checkout -b agent/waitlist
git checkout -b agent/billing-stripe
git checkout -b agent/appointment-payments
git checkout -b agent/cms-marketing
git checkout -b agent/analytics
git checkout -b agent/platform-admin
git checkout -b agent/testing-devops
```

In practice, create only the branch you are working on at the moment.

---

## 11.5 Create task brief template

```bash
cat > docs/TASK_BRIEF_TEMPLATE.md <<'EOF_TASK'
# Task Brief

## Task

## Owner Agent

## Files allowed

## Files not allowed

## Inputs

## Outputs

## Acceptance criteria

## Tests required

## Dependencies

## Review required from

EOF_TASK

git add docs/TASK_BRIEF_TEMPLATE.md
git commit -m "Add task brief template"
```

---

# 12. Practical workflow

Use this process:

1. Lead Architect creates architecture and contracts.
2. Foundation/Auth agents build base system.
3. Scheduling agent builds tested availability engine.
4. Booking/Admin agents build UI against real APIs.
5. Calendar/Notification agents integrate external systems.
6. Waitlist agent builds on scheduling engine.
7. Billing agent adds subscriptions and gates.
8. Appointment payments agent adds client payments.
9. CMS/Marketing agent builds growth pages.
10. Platform Admin agent builds operator console.
11. QA/DevOps agents harden, test, deploy.

The most important sequencing rule:

```text
Do not build advanced features until the core booking flow works end-to-end.
```
