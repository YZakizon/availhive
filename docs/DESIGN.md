# DESIGN.md

## Product

**ShowLockr** is a SaaS app for service businesses that want to secure appointments with deposits, policy acceptance, SMS/email reminders, and no-show protection.

Target customers include:

- Hair salons
- Nail salons
- Spas
- Barbers
- Tattoo studios
- Med spas
- Tutors
- Photographers
- Independent service providers

The product should feel modern, trustworthy, fast, and simple. It is not a generic calendar app. It is an appointment commitment platform.

Core promise:

> Secure every appointment before it reaches your calendar.

Primary workflow:

```text
Create booking hold
→ Send confirmation link
→ Client accepts policy
→ Client opts into SMS/email reminders
→ Client pays deposit
→ Appointment becomes confirmed
→ Reminders are sent
→ Appointment happens or is marked cancelled/no-show
```

---

## Tech Stack

Use this frontend stack:

```text
Next.js App Router
TypeScript
Tailwind CSS
shadcn/ui
Lucide React icons
React Hook Form
Zod
```

Do not use:

```text
Material UI
Bootstrap
Chakra UI
Ant Design
DaisyUI
```

Use shadcn/ui components where appropriate:

```text
Button
Card
Input
Label
Textarea
Select
Checkbox
Tabs
Badge
Table
Dialog
DropdownMenu
Sheet
Separator
Avatar
Calendar
Popover
Toast/Sonner
Tooltip
Progress
```

Use Lucide icons:

```text
Calendar
CreditCard
ShieldCheck
Bell
Mail
MessageSquare
Users
Scissors
Settings
LayoutDashboard
Receipt
Clock
CheckCircle2
AlertTriangle
XCircle
Link
Copy
Plus
Search
Filter
ChevronRight
MoreHorizontal
Lock
```

---

## Design Direction

### Visual style

The UI should look like a premium modern B2B SaaS dashboard.

Use:

```text
Clean white backgrounds
Soft gray panels
Rounded cards
Subtle shadows
Blue/indigo accent colors
Calm green success states
Warm amber pending states
Soft red danger states
Readable typography
Generous spacing
Mobile-first customer pages
Desktop-first admin dashboard
```

Avoid:

```text
Overly playful colors
Dark cyberpunk style
Heavy gradients everywhere
Tiny unreadable text
Crowded dashboards with no hierarchy
Generic template look
```

### Brand personality

ShowLockr should feel:

```text
Trustworthy
Secure
Operationally useful
Clear
Calm
Professional
Fast
```

Not:

```text
Cute
Luxury-only
Enterprise-heavy
Crypto/fintech-like
Medical-only
Tattoo-only
```

---

## Design Tokens

Use Tailwind variables and shadcn theme tokens.

Recommended color direction:

```text
Primary: blue/indigo
Background: white / slate-50
Foreground: slate-950
Muted: slate-100 / slate-500
Border: slate-200
Success: emerald
Warning: amber
Danger: rose/red
Info: sky/blue
```

Example Tailwind usage:

```tsx
className="bg-background text-foreground"
className="border border-border bg-card shadow-sm rounded-xl"
className="text-muted-foreground"
className="bg-primary text-primary-foreground"
className="bg-emerald-50 text-emerald-700 border-emerald-200"
className="bg-amber-50 text-amber-700 border-amber-200"
className="bg-rose-50 text-rose-700 border-rose-200"
```

### Border radius

Use consistent rounded UI:

```text
Cards: rounded-xl or rounded-2xl
Buttons: rounded-md or rounded-lg
Badges: rounded-full
Inputs: shadcn default
```

### Shadows

Use subtle shadows only:

```text
shadow-sm
shadow-md for elevated panels only
No heavy drop shadows
```

### Typography

Use the default Next.js font or Inter if configured.

Text hierarchy:

```text
Page title: text-2xl or text-3xl font-semibold tracking-tight
Section title: text-lg font-semibold
Card title: text-sm font-medium or text-base font-semibold
Muted helper: text-sm text-muted-foreground
Table text: text-sm
Badge text: text-xs font-medium
```

---

## App Information Architecture

### Public routes

```text
/                       Marketing landing page
/pricing                Optional pricing page
/login                  Sign in
/signup                 Sign up
/confirm/[token]        Public customer confirmation/deposit page
/confirm/[token]/success Confirmation success page
/confirm/[token]/expired Expired hold page
```

### Authenticated app routes

```text
/app/dashboard
/app/booking-holds
/app/booking-holds/new
/app/booking-holds/[id]
/app/appointments
/app/calendar
/app/services
/app/policies
/app/providers
/app/customers
/app/customers/[id]
/app/payments
/app/waitlist
/app/analytics
/app/settings
/app/settings/business
/app/settings/notifications
/app/settings/integrations
/app/settings/billing
```

### Navigation labels

Main sidebar:

```text
Dashboard
Booking Holds
Appointments
Calendar
Services & Policies
Providers / Staff
Customers
Payments
Waitlist & Refill
Analytics
Settings
```

---

## Core Layouts

## 1. Marketing Landing Page

Route:

```text
/
```

Goal:

Convert service businesses to start using ShowLockr.

Hero copy:

```text
Headline:
Secure every appointment before it hits your calendar.

Subheadline:
Collect deposits, capture policy acceptance, and send automated reminders so clients show up — or release the slot in time.

Primary CTA:
Start free

Secondary CTA:
View demo
```

Hero feature pills:

```text
Deposit links
Policy acceptance
SMS & email reminders
No-show protection
```

Sections:

```text
Hero
Problem section
How it works
Feature cards
Use cases by industry
Pricing preview
FAQ
Footer
```

Use case cards:

```text
Hair salons
Nail studios
Spas
Barbers
Tattoo artists
Med spas
Tutors
Photographers
```

How it works:

```text
1. Create a booking hold
2. Send the secure confirmation link
3. Client accepts your policy
4. Client pays the deposit
5. Appointment is confirmed
6. Reminders go out automatically
```

---

## 2. Auth Pages

Routes:

```text
/login
/signup
```

Layout:

Centered card with brand on the left/top.

Fields:

```text
Email
Password
Business name for signup
```

Actions:

```text
Continue with email
Continue with Google placeholder
Forgot password
Create account / Sign in link
```

Tone:

Simple, minimal, no clutter.

---

## 3. App Shell

Authenticated pages use a shared shell.

Desktop layout:

```text
Left sidebar: 260px
Top header: page title, search/quick actions, account menu
Main content: responsive grid/cards
```

Mobile layout:

```text
Top bar with logo and menu button
Sidebar becomes Sheet drawer
Primary actions remain visible
Tables become stacked cards when needed
```

Sidebar footer:

```text
Business avatar/logo
Business name
Owner/admin role
Settings dropdown
```

---

## 4. Dashboard

Route:

```text
/app/dashboard
```

Purpose:

Show business owner what needs attention today.

Primary content:

```text
KPI cards
Recent activity
Upcoming appointments
Awaiting deposit holds
Quick actions
```

KPI cards:

```text
Upcoming appointments
Awaiting deposits
Confirmed appointments
No-show rate
Deposit revenue
Cancellation risk
```

Quick actions:

```text
New booking hold
Create service
Add provider
View payments
```

Recent activity examples:

```text
Deposit paid by Emily Davis
Booking hold created for Michael Brown
Policy accepted by Jessica Martinez
SMS reminder sent to Olivia Wilson
Booking hold expired for Sarah Johnson
```

Upcoming appointments table columns:

```text
Time
Client
Service
Provider
Status
Deposit
Actions
```

---

## 5. Booking Holds List

Route:

```text
/app/booking-holds
```

Purpose:

Manage appointments that are not fully confirmed yet or were recently confirmed.

Top actions:

```text
New Booking Hold
Search
Filters
Export optional
```

Filters:

```text
Status
Provider
Service
Date range
Deposit status
```

Table columns:

```text
Client
Service
Provider
Date & time
Deposit
Status
Created
Actions
```

Statuses:

```text
Hold Created
Awaiting Confirmation
Policy Accepted
Deposit Paid
Confirmed
Expired
Cancelled
```

Status badge colors:

```text
Hold Created: slate
Awaiting Confirmation: amber
Policy Accepted: blue
Deposit Paid: emerald
Confirmed: green
Expired: rose
Cancelled: red/slate
```

Row actions:

```text
View
Copy confirmation link
Resend email
Cancel hold
```

---

## 6. Create Booking Hold

Route:

```text
/app/booking-holds/new
```

Purpose:

Allow a business to create a deposit-secured appointment hold manually.

This is the MVP's most important admin form.

Form sections:

### Client Information

```text
Client name required
Phone optional until customer confirms
Email optional until customer confirms
Instagram / handle optional
```

### Appointment

```text
Service required
Provider required if multi-provider business
Date required
Start time required
Duration auto-filled from service but editable
Location optional
```

### Payment

```text
Deposit amount required
Deposit type: fixed or percent
Estimated service total optional
```

### Policy

```text
Policy template required
Cancellation window display
No-show rule display
```

### Notes

```text
Internal notes optional
Visible-to-client notes optional
```

Primary action:

```text
Create Confirmation Link
```

After successful creation:

Show a success card with:

```text
Confirmation link
Copy button
Share by email button if email exists
Open hold detail button
```

Important rule:

Do not imply SMS is sent automatically unless consent already exists.

Helper text:

```text
Send this link to the client through your existing conversation. The client will confirm their details, accept your policy, opt into reminders, and pay the deposit.
```

---

## 7. Booking Hold Detail

Route:

```text
/app/booking-holds/[id]
```

Purpose:

Track a single hold from creation to confirmation.

Main layout:

```text
Header with client name, status badge, actions menu
Left: appointment summary
Middle: status timeline
Right: payment/policy/contact cards
Bottom: activity log
```

Timeline steps:

```text
1. Hold created
2. Confirmation link sent/copied
3. Client opened link
4. Policy accepted
5. Deposit paid
6. Appointment confirmed
```

Actions:

```text
Copy confirmation link
Resend email
Edit hold
Cancel hold
Mark expired
```

Show warnings:

```text
SMS consent not collected yet
Deposit unpaid
Hold expires soon
Policy not accepted
```

---

## 8. Public Confirmation Page

Route:

```text
/confirm/[token]
```

Purpose:

Client confirms the appointment, accepts policy, opts into reminders, and pays deposit.

This page must be mobile-first and very clean.

No dashboard/sidebar.

Page structure:

```text
Business logo/name
Secure booking indicator
Appointment summary card
Client details form
Policy acceptance section
Reminder consent section
Deposit payment section
CTA button
Footer terms/privacy
```

Appointment summary:

```text
Service
Provider
Date
Time
Duration
Location
Deposit amount
Estimated total if available
```

Policy section:

```text
Policy title
Policy summary
View full policy button/dialog
Required checkbox: I have read and agree to the cancellation and no-show policy.
```

Reminder consent:

```text
Optional checkbox: Send me appointment text reminders.
Optional checkbox: Send me appointment email reminders.
```

SMS consent text:

```text
By checking this box, you agree to receive appointment-related text messages from this business through ShowLockr, including confirmations, reminders, and cancellation updates. Reply STOP to opt out. Message and data rates may apply.
```

Payment section:

```text
Deposit amount
Stripe payment element placeholder or Checkout redirect button
```

CTA:

```text
Pay Deposit & Confirm
```

Loading states:

```text
Checking hold...
Creating secure checkout...
Redirecting to payment...
```

Error states:

```text
This booking hold has expired.
This booking hold was already confirmed.
This booking hold was cancelled.
Unable to create payment session.
```

---

## 9. Confirmation Success Page

Route:

```text
/confirm/[token]/success
```

Show:

```text
Success icon
You're confirmed
Appointment details
Deposit receipt summary
Add to calendar placeholder
Cancellation/reschedule link placeholder
Contact business
```

Copy:

```text
Your appointment is confirmed. Your deposit has been received and applied according to the business policy.
```

---

## 10. Services & Policies

Routes:

```text
/app/services
/app/policies
```

Can be one combined page with tabs:

```text
Services
Policy Templates
```

Service fields:

```text
Name
Category
Duration
Price
Deposit amount
Deposit type
Default provider optional
Active/inactive
```

Policy template fields:

```text
Name
Cancellation window hours
Late cancellation rule
No-show rule
Refund rule
Full policy text
Default yes/no
```

Service list columns:

```text
Service
Duration
Price
Deposit
Policy
Status
Actions
```

Policy cards:

```text
Standard Policy
Strict 24-hour Policy
No Refund Policy
Custom Policy
```

---

## 11. Providers / Staff

Route:

```text
/app/providers
```

Purpose:

Support multi-provider businesses.

Provider card fields:

```text
Avatar
Name
Role/specialty
Services performed
Availability summary
Status
```

Actions:

```text
Add provider
Edit provider
Manage availability
Assign services
```

Provider detail sections:

```text
Profile
Services
Availability
Upcoming appointments
Deposit/policy overrides optional
```

---

## 12. Customers

Routes:

```text
/app/customers
/app/customers/[id]
```

Customer list:

```text
Search
Filters
Customer cards/table
```

Columns:

```text
Name
Contact
Last visit
Total bookings
No-shows
Reliability
Actions
```

Customer detail:

```text
Contact info
Reliability score
Booking history
Payment/deposit history
Policy acceptance history
SMS/email consent state
Notes
```

Reliability states:

```text
Excellent
Good
Watch
Risky
Blocked
```

Important display rule:

Reliability should be based only on appointment behavior, not sensitive personal data.

---

## 13. Payments

Route:

```text
/app/payments
```

Purpose:

Show deposits, refunds, payouts, and integration state.

Summary cards:

```text
Total deposits
Refunds
Payouts
Net revenue
Failed payments
```

Tabs:

```text
Deposits
Refunds
Payouts
Failed
```

Ledger columns:

```text
Date
Type
Client
Service
Amount
Status
Method
Actions
```

Integration status cards:

```text
Stripe connected
Twilio connected
SendGrid connected
Calendar connected
```

Payment statuses:

```text
Pending
Succeeded
Failed
Refunded
Partially refunded
Forfeited
Paid out
```

---

## 14. Waitlist & Refill

Route:

```text
/app/waitlist
```

Purpose:

Later feature: refill cancelled appointments.

MVP can have UI shell with mock data.

Sections:

```text
Open slots
Waitlist queue
Refill activity
Settings
```

Open slots table:

```text
Date/time
Service
Provider
Reason opened
Potential matches
Action: Notify waitlist
```

Waitlist columns:

```text
Customer
Service
Preferred provider
Preferred time
Added
Status
```

Statuses:

```text
Active
Offered
Claimed
Expired
Removed
```

---

## 15. Analytics

Route:

```text
/app/analytics
```

Purpose:

Show whether ShowLockr is protecting revenue.

Metric cards:

```text
No-show rate
Deposit conversion rate
Late cancellations
Recovered revenue
Confirmed bookings
Expired holds
Reminder response rate
```

Charts:

```text
No-show rate over time
Deposit conversion over time
Cancellations by reason
Bookings by service
Revenue from deposits
```

Use simple placeholders if no chart library is installed.

Do not add a heavy chart library unless asked.

---

## 16. Settings

Route:

```text
/app/settings
```

Tabs/sidebar:

```text
Business Profile
Booking Page
Notifications
Branding
Integrations
Team & Permissions
Billing & Plan
```

Business profile fields:

```text
Business name
Email
Phone
Address
Website
Timezone
Logo
```

Notifications:

```text
Email reminder templates
SMS reminder templates
Reminder schedule
Cancellation messages
No-show messages
```

Integrations:

```text
Stripe
Twilio
SendGrid
Google Calendar
Outlook Calendar future
```

Billing:

```text
Current plan
Usage
SMS usage
Deposit volume
Upgrade button
```

---

## Component Guidelines

Create reusable components in:

```text
src/components/app/
src/components/dashboard/
src/components/booking-holds/
src/components/customers/
src/components/payments/
src/components/public-confirmation/
src/components/shared/
```

Suggested components:

```text
AppSidebar
AppTopbar
PageHeader
MetricCard
StatusBadge
EmptyState
DataTable
FormSection
CopyLinkButton
BookingHoldStatusTimeline
AppointmentSummaryCard
PolicyPreviewCard
ConsentCheckboxGroup
PaymentSummaryCard
ActivityFeed
IntegrationStatusCard
CustomerReliabilityBadge
```

---

## Status Badge System

Use a single `StatusBadge` component.

Booking hold statuses:

```text
HOLD_CREATED
AWAITING_CONFIRMATION
POLICY_ACCEPTED
AWAITING_DEPOSIT
DEPOSIT_PAID
CONFIRMED
EXPIRED
CANCELLED
```

Appointment statuses:

```text
CONFIRMED
COMPLETED
CANCELLED
LATE_CANCELLED
NO_SHOW
RESCHEDULED
```

Payment statuses:

```text
PENDING
SUCCEEDED
FAILED
REFUNDED
PARTIALLY_REFUNDED
FORFEITED
PAID_OUT
```

Message statuses:

```text
QUEUED
SENT
DELIVERED
FAILED
OPTED_OUT
```

Colors:

```text
Created / neutral: slate
Awaiting / pending: amber
Accepted / in-progress: blue
Paid / completed: emerald
Confirmed: green
Expired / danger: rose
Cancelled: red or muted slate
```

---

## Mock Data Requirements

Until backend is connected, use realistic mock data.

Example businesses:

```text
Beauty & Co. Studio
Northside Barber Lounge
Glow Nail Lab
Ink Room Studio
BrightPath Tutoring
Lens & Light Photography
```

Example providers:

```text
Sarah Lee
Nina Kim
Michael Brown
Alex Thompson
Olivia Wilson
Chris Taylor
```

Example services:

```text
Women's Haircut
Men's Haircut
Gel Manicure
Facial
Tattoo Session
Lash Extensions
Massage
Tutoring Session
Portrait Session
Consultation
```

Example customers:

```text
Jessica Martinez
Emily Davis
Michael Brown
Sarah Johnson
Alex Thompson
Olivia Wilson
Daniel Lee
Taylor Kim
Jordan Parker
Casey Morgan
```

Example policies:

```text
Standard 24-hour Cancellation Policy
Strict 24-hour Policy
48-hour Deposit Policy
No Refund Policy
Consultation Prepay Policy
```

---

## Responsive Behavior

### Desktop

Use full dashboard layout with sidebar and tables.

### Tablet

Sidebar can collapse. Use 2-column grids.

### Mobile

Customer confirmation page must be excellent on mobile.

Admin pages should be usable, but not overloaded:

```text
Tables become cards
Sidebar becomes drawer
Primary CTA stays visible
Forms are single-column
Timeline stacks vertically
```

Mobile priority actions:

```text
Create booking hold
Copy confirmation link
View awaiting deposits
Mark appointment completed/no-show
View payment status
```

---

## Accessibility Requirements

Implement:

```text
Semantic headings
Proper labels for form fields
Keyboard-accessible buttons and menus
Visible focus states
Accessible dialogs
Sufficient color contrast
ARIA labels for icon-only buttons
No information conveyed by color alone
```

Forms must show validation errors near fields.

Buttons should clearly show loading/disabled states.

---

## Empty States

Design useful empty states.

Examples:

### No booking holds

```text
No booking holds yet.
Create your first deposit-secured booking hold and send the confirmation link to a client.
[Create Booking Hold]
```

### No services

```text
Add your first service.
Services define duration, price, deposit amount, and default policy.
[Add Service]
```

### No customers

```text
Customers will appear after they confirm a booking or pay a deposit.
```

### No payments

```text
Deposit payments will appear here after clients confirm appointments.
```

---

## Loading States

Use skeletons for:

```text
Dashboard KPI cards
Tables
Customer detail
Booking hold detail
Payments ledger
```

Use button loading states for:

```text
Create confirmation link
Copy link
Create checkout session
Save settings
```

---

## Error States

Show clear errors.

Examples:

```text
Unable to create booking hold. Please check the form and try again.
This confirmation link has expired.
This appointment has already been confirmed.
Payment could not be started. Please try again.
SMS reminders cannot be enabled until the client opts in.
```

---

## Frontend Implementation Rules for Codex

When implementing UI:

```text
Read DESIGN.md, CODEX_PLAN.md, CODEX_RULES.md, and AGENTS.md first.
Use Next.js App Router.
Use TypeScript.
Use Tailwind and shadcn/ui.
Use server components by default.
Use client components only for forms, interactions, dialogs, tabs, and dynamic UI.
Use mock data until backend endpoints exist.
Keep components small and reusable.
Do not add backend business logic unless the task asks for it.
Do not add new dependencies unless necessary.
Do not create marketing SMS logic.
Do not send SMS from the frontend.
Run lint and typecheck before finishing.
```

---

# Master Codex Frontend UI Prompt

Use this prompt in Codex CLI when you want to generate the initial frontend UI.

```text
Read AGENTS.md, CODEX_RULES.md, CODEX_PLAN.md, and DESIGN.md first.

You are the frontend UI agent for ShowLockr.

Build a polished MVP frontend using:
- Next.js App Router
- TypeScript
- Tailwind CSS
- shadcn/ui
- Lucide React icons

For this task, use mock data only. Do not connect to the database. Do not implement Stripe, Twilio, SendGrid, or real auth logic. Do not send SMS or email.

Implement these routes and screens:

Public:
- / landing page
- /login
- /signup
- /confirm/[token]
- /confirm/[token]/success
- /confirm/[token]/expired

Authenticated app shell:
- /app/dashboard
- /app/booking-holds
- /app/booking-holds/new
- /app/booking-holds/[id]
- /app/services
- /app/providers
- /app/customers
- /app/payments
- /app/waitlist
- /app/analytics
- /app/settings

Build reusable components:
- AppSidebar
- AppTopbar
- PageHeader
- MetricCard
- StatusBadge
- EmptyState
- DataTable or table components
- FormSection
- CopyLinkButton
- BookingHoldStatusTimeline
- AppointmentSummaryCard
- PolicyPreviewCard
- ConsentCheckboxGroup
- PaymentSummaryCard
- ActivityFeed
- IntegrationStatusCard
- CustomerReliabilityBadge

Design requirements:
- Modern premium SaaS dashboard
- White/light gray background
- Rounded cards
- Subtle shadows
- Blue/indigo primary color
- Clean typography
- Mobile-first public confirmation page
- Responsive authenticated dashboard
- Tables should become usable stacked layouts on mobile where practical

Important product rules:
- A booking hold is not confirmed until policy acceptance and deposit payment are complete.
- A phone number is not SMS consent.
- The UI must clearly separate client contact info from SMS reminder opt-in.
- Do not imply SMS reminders are enabled unless the client has opted in.
- The create booking hold flow should generate/copy a confirmation link that the business can send manually.

Use realistic mock data for salons, spas, nail studios, barbers, tattoo studios, med spas, tutors, and photographers.

After implementation, run:
- npm run lint
- npm run typecheck

Return a summary of changed files and any follow-up tasks.
```

---

# Smaller UI Task Prompts

## Prompt: Landing Page Only

```text
Read DESIGN.md.
Implement only the public landing page at / for ShowLockr using Next.js, TypeScript, Tailwind, shadcn/ui, and Lucide icons.
Use the hero, problem, how-it-works, features, use-cases, pricing preview, FAQ, and footer sections from DESIGN.md.
Do not implement backend logic.
Run lint and typecheck.
```

## Prompt: App Shell Only

```text
Read DESIGN.md.
Implement the authenticated app shell with sidebar, topbar, responsive mobile drawer, and placeholder pages for all /app routes.
Use shadcn/ui and Lucide icons.
Do not implement backend logic.
Run lint and typecheck.
```

## Prompt: Booking Hold UI Only

```text
Read DESIGN.md.
Implement the booking hold frontend screens:
- /app/booking-holds
- /app/booking-holds/new
- /app/booking-holds/[id]
Use mock data only.
Include status badges, timeline, copy confirmation link UI, and create hold form.
Do not implement Stripe or Twilio.
Run lint and typecheck.
```

## Prompt: Public Confirmation Page Only

```text
Read DESIGN.md.
Implement the mobile-first public confirmation page at /confirm/[token], plus success and expired states.
Use mock data only.
Include appointment summary, client details form, policy acceptance checkbox, SMS/email opt-in checkboxes, and deposit payment placeholder.
Do not implement real payment or messaging.
Make the consent language clear.
Run lint and typecheck.
```

## Prompt: Dashboard UI Only

```text
Read DESIGN.md.
Implement /app/dashboard with KPI cards, recent activity, upcoming appointments, awaiting deposits, and quick actions.
Use mock data only.
Use shadcn/ui Card, Badge, Table, Button, and Tabs where useful.
Run lint and typecheck.
```

## Prompt: Settings UI Only

```text
Read DESIGN.md.
Implement /app/settings with tabs/sidebar for business profile, booking page, notifications, integrations, team permissions, and billing.
Use mock data only.
Show Stripe, Twilio, SendGrid, and Google Calendar integration status cards.
Run lint and typecheck.
```

---

## Done Criteria for Frontend UI

The frontend UI task is complete when:

```text
All routes render without runtime errors.
Navigation works between pages.
Mock data appears realistic.
Public confirmation page works well on mobile.
Authenticated dashboard works well on desktop.
Primary forms have visible validation placeholders or UI states.
Status badges are consistent.
Empty/loading/error states exist for main screens or are stubbed clearly.
No backend secrets are used in frontend code.
npm run lint passes.
npm run typecheck passes.
```
