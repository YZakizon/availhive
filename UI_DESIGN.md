# UI_DESIGN.md — AvailHive Product and Wireframe Guide

## Product identity

AvailHive is a clean, modern SaaS platform for multi-provider booking and availability management.

Positioning:

> Turn availability into bookings.

AvailHive helps service businesses manage provider availability, booking pages, appointment scheduling, reminders, and waitlist automation from one workspace.

## Target users

AvailHive is designed for:

- Clinics
- Salons
- Consultants
- Coaches
- Agencies
- Repair businesses
- Wellness providers
- Education/tutoring businesses
- Teams that manage appointments across multiple staff members or resources

## Design principles

### 1. Availability-first

The interface should make availability feel central. Users should always understand:

- Who is available
- What service can be booked
- When slots are open
- Which provider is assigned
- Where the appointment happens
- Whether waitlist opportunities exist

### 2. Multi-provider from the start

The product must visually support businesses with one provider or many providers.

Provider-aware UI should appear in:

- Dashboard
- Calendar
- Booking page builder
- Public booking page
- Appointments
- Waitlist
- Reports
- Settings

### 3. Simple for non-technical users

Use clear language and progressive disclosure.

Avoid overwhelming users with scheduling complexity. Show advanced rules only when needed.

### 4. Fast client booking

The public booking experience should be mobile-first, fast, and low-friction.

Clients should be able to book in a few steps:

1. Choose service.
2. Choose provider or any available provider.
3. Choose date and time.
4. Enter details.
5. Confirm.

If no slot is available, joining the waitlist should feel like the natural next step.

## Visual style

### Overall look

- Modern SaaS
- Clean dashboard layout
- Light neutral background
- White cards
- Soft borders
- Rounded corners
- Calm professional palette
- Subtle shadows
- Clear CTAs
- Simple icons

### Suggested tone

- Helpful
- Efficient
- Calm
- Trustworthy
- Polished

### Layout language

- Left sidebar for authenticated app navigation
- Top bar with workspace switcher, search, notifications, and user menu
- Card-based dashboard sections
- Tables for operational data
- Drawers for appointment details
- Modals for quick actions
- Step-based flows for onboarding and booking page creation

## Main navigation

Sidebar items:

- Dashboard
- Calendar
- Booking Pages
- Appointments
- Availability
- Waitlist
- Clients
- Providers
- Services
- Settings

Settings sub-navigation:

- Workspace
- Team
- Providers
- Calendar Integrations
- Notifications
- Payments
- Branding
- Booking Rules

## Screen 1 — Marketing homepage

### Purpose

Explain AvailHive and convert visitors into signup or demo requests.

### Sections

#### Hero

Headline:

> Turn availability into bookings.

Subheadline:

> AvailHive helps teams manage provider availability, booking pages, calendar sync, reminders, and waitlist automation in one simple scheduling platform.

Primary CTA:

- Start free

Secondary CTA:

- View demo

Hero visual:

- Dashboard preview showing providers, available slots, and upcoming appointments.

#### Feature cards

- Multi-provider scheduling
- Smart availability
- Booking pages
- Calendar sync
- Waitlist fill
- Automated reminders
- Team permissions
- Client intake

#### Multi-provider section

Show a simple visual of three providers with different availability and services.

Copy:

> Manage every provider’s schedule without double-booking.

#### Waitlist automation section

Show cancellation opening and automatic slot offer.

Copy:

> Fill cancellations faster with provider-aware waitlists.

#### Social proof

Use placeholder testimonials from businesses:

- Clinic owner
- Salon manager
- Consultant team lead

#### Pricing preview

Cards:

- Solo
- Team
- Growth

#### Final CTA

Headline:

> Ready to make every open slot easier to book?

CTA:

- Start free

## Screen 2 — Signup and onboarding

### Purpose

Help a new business create a working booking page quickly.

### Steps

1. Create account
2. Create workspace
3. Choose business type
4. Add first service
5. Add provider or invite team
6. Set provider availability
7. Connect calendar
8. Create booking page
9. Publish booking link

### Multi-provider details

On the provider step, offer two choices:

- “I’m the only provider”
- “I have multiple providers or resources”

If multiple providers:

- Add provider names
- Assign services
- Set default availability
- Invite providers by email later

### Empty state

If no providers exist:

> Add your first provider to start accepting bookings.

CTA:

- Add provider

## Screen 3 — Main dashboard

### Purpose

Give admins a quick operational overview.

### Layout

Top summary cards:

- Bookings today
- Available slots
- Waitlist requests
- Provider utilization
- No-show rate

Main content:

- Upcoming appointments
- Availability by provider
- Waitlist opportunities
- Recent activity
- Booking page performance

### Provider-aware elements

Include filters:

- Provider
- Service
- Location
- Date range

Availability card:

- Provider avatar/name
- Next available slot
- Today’s bookings
- Utilization percentage

### Empty states

No appointments:

> No bookings yet. Publish your booking page to start filling your calendar.

No provider availability:

> Add provider availability so clients can book open times.

## Screen 4 — Provider management

### Purpose

Let admins manage bookable people and resources.

### Provider list

Columns/cards:

- Name
- Role/type
- Assigned services
- Connected calendar status
- Today’s bookings
- Next available slot
- Status

Actions:

- Add provider
- Invite provider
- Edit
- Deactivate

### Provider profile

Sections:

- Profile details
- Services offered
- Working hours
- Calendar connection
- Locations
- Booking rules
- Permissions

### Provider profile fields

- Name
- Display name
- Role/title
- Photo
- Bio
- Email
- Phone
- Provider type
- Timezone
- Default location
- Booking priority

### Permission options

- Can manage own availability
- Can manage own appointments
- Can see client contact info
- Can view all provider schedules
- Can edit services

## Screen 5 — Services screen

### Purpose

Configure what clients can book.

### Service list

Columns/cards:

- Service name
- Duration
- Price
- Assigned providers
- Location/method
- Status

### Service editor

Fields:

- Name
- Description
- Duration
- Price
- Location/method
- Buffer before
- Buffer after
- Minimum notice
- Maximum booking window
- Assigned providers
- Intake questions

### Multi-provider behavior

Service assignment options:

- All providers
- Selected providers
- No providers yet

Warning state:

> This service has no assigned providers, so clients cannot book it yet.

## Screen 6 — Availability screen

### Purpose

Manage recurring and one-off availability.

### Views

- Provider schedule view
- Weekly availability editor
- Date-specific overrides
- Holiday/unavailable days
- Location-specific availability

### Provider selector

At the top:

- All providers
- Specific provider dropdown

### Weekly schedule

For each provider:

- Monday through Sunday
- Available time ranges
- Breaks
- Location/method

### Override examples

- Mark unavailable for vacation
- Add special Saturday hours
- Change location for a specific day

### Empty state

> No availability has been set for this provider.

CTA:

- Add working hours

## Screen 7 — Booking page builder

### Purpose

Let admins create public booking pages.

### Sections

- Page title
- Page slug
- Description
- Services included
- Provider selection mode
- Assignment strategy
- Branding
- Intake questions
- Reminder settings
- Confirmation settings
- Preview panel

### Provider selection modes

1. Any available provider
2. Client chooses provider
3. Specific provider only

### Assignment strategy for any-provider mode

Options:

- First available
- Round robin
- Least booked
- Manual approval

### Preview panel

Preview should show:

- Service selection
- Provider selection if enabled
- Date picker
- Time slots
- Client details form
- Waitlist option

## Screen 8 — Client-facing booking page

### Purpose

Let clients book appointments quickly.

### Mobile-first flow

Step 1: Choose service

- Service name
- Duration
- Price if enabled
- Description

Step 2: Choose provider

Options depend on booking page settings:

- Any available provider
- Specific provider cards

Provider card should show:

- Name
- Role
- Photo
- Next available slot
- Services eligible

Step 3: Choose date and time

- Date picker
- Available time slots
- Timezone display
- Provider name if specific provider is selected

Step 4: Enter details

- Name
- Email
- Phone
- Intake questions
- Notes

Step 5: Confirm

- Service
- Provider
- Date/time
- Location/method
- Price/payment if enabled

### No availability state

Message:

> No matching times are available right now.

Actions:

- Try another provider
- Try another date
- Join waitlist

### Waitlist entry

Fields:

- Preferred provider or any provider
- Preferred dates
- Preferred times
- Contact details
- Notes

## Screen 9 — Team schedule / calendar

### Purpose

Show bookings and availability across providers.

### Views

- Day
- Week
- Month
- Provider columns
- Service filters

### Provider-column day view

Columns:

- Provider A
- Provider B
- Provider C

Each column shows:

- Appointments
- Available gaps
- Blocked times
- Waitlist opportunities

### Filters

- Provider
- Service
- Location
- Status
- Date range

### Actions

- Create appointment
- Block time
- Edit availability
- Offer waitlist slot
- Reschedule appointment

## Screen 10 — Appointments screen

### Purpose

Manage confirmed, cancelled, completed, and missed appointments.

### List/table columns

- Date/time
- Client
- Provider
- Service
- Location/method
- Status
- Payment status
- Source

### Filters

- Provider
- Service
- Location
- Status
- Date
- Booking source

### Appointment detail drawer

Sections:

- Appointment summary
- Client details
- Provider details
- Service details
- Intake answers
- Notification history
- Payment status
- Internal notes

Actions:

- Reschedule
- Cancel
- Mark completed
- Mark no-show
- Send reminder
- Offer slot to waitlist if cancelled

## Screen 11 — Waitlist screen

### Purpose

Help businesses fill cancellations and open slots.

### Summary cards

- Waiting clients
- Open slot matches
- Offers pending
- Filled from waitlist

### Waitlist table columns

- Client
- Service
- Provider preference
- Preferred dates/times
- Priority
- Status
- Created date

### Provider preference display

Examples:

- “Dr. Chen only”
- “Any eligible provider”
- “Any provider at Downtown location”

### Match panel

When an opening exists, show:

- Open slot
- Provider
- Service
- Eligible waitlist entries
- Confidence/match reason

Actions:

- Offer slot
- Skip
- Mark contacted
- Book manually

### Automation toggle

Label:

- Auto-fill cancellations

Helper text:

> Automatically offer open slots to eligible waitlisted clients based on provider, service, preferred times, and priority.

## Screen 12 — Clients screen

### Purpose

Manage client records and booking history.

### Client list columns

- Name
- Email
- Phone
- Last appointment
- Upcoming appointment
- Preferred provider
- Total bookings
- Status

### Client profile

Sections:

- Contact details
- Upcoming appointments
- Past appointments
- Waitlist entries
- Notes
- Intake history

## Screen 13 — Settings

### Purpose

Configure workspace-wide behavior.

### Sections

#### Workspace settings

- Business name
- Slug
- Timezone
- Currency
- Logo
- Brand color

#### Team

- Invite users
- Assign roles
- Remove users

#### Providers

- Provider defaults
- Provider permissions
- Booking priority

#### Calendar integrations

- Provider calendar status
- Connect calendar
- Disconnect calendar
- Sync settings

#### Notifications

- Client confirmation
- Provider notification
- Reminder timing
- Cancellation message
- Waitlist offer message

#### Payments

- Stripe connection
- Payment requirements
- Deposit settings
- Cancellation policy

#### Booking rules

- Minimum notice
- Maximum future booking window
- Cancellation window
- Time slot increments
- Assignment strategy defaults

## Component guidelines

### Provider selector

Use on:

- Dashboard
- Calendar
- Appointments
- Availability
- Waitlist
- Reports

States:

- All providers
- Specific provider
- Provider group if later supported

### Provider card

Show:

- Avatar
- Name
- Role
- Status
- Next available time
- Services count
- Calendar connection status

### Time slot button

States:

- Available
- Selected
- Unavailable
- Loading
- Held temporarily

### Waitlist badge

Statuses:

- Waiting
- Offered
- Booked
- Expired
- Cancelled

### Appointment status badge

Statuses:

- Confirmed
- Cancelled
- Completed
- No-show
- Rescheduled

## Empty-state copy

### No providers

> Add your first provider to start accepting bookings.

CTA:

- Add provider

### No services

> Create a service so clients know what they can book.

CTA:

- Create service

### No availability

> Set working hours so AvailHive can find bookable times.

CTA:

- Add availability

### No booking pages

> Create a booking page to share your availability with clients.

CTA:

- Create booking page

### No waitlist entries

> When clients can’t find a matching time, they can join your waitlist here.

CTA:

- Configure waitlist

## Wireframe prompt

Use this prompt to generate wireframes for AvailHive:

```text
Create a clean SaaS wireframe for a booking and availability management platform called AvailHive.

Product concept:
AvailHive helps businesses turn availability into bookings. Users can connect calendars, create booking pages, manage appointment slots, collect client details, send reminders, support multiple providers, and fill cancellations from a provider-aware waitlist.

Design style:
Modern, minimal, professional SaaS UI. Use a clean dashboard layout, lots of white space, soft rounded cards, simple icons, neutral colors, and clear CTAs. Prioritize usability over decoration.

Target users:
Small businesses, consultants, clinics, salons, coaches, service providers, and teams that need appointment scheduling across multiple providers.

Core requirement:
The product must support multi-provider scheduling from the beginning. A provider can be a staff member, consultant, clinician, stylist, technician, room, equipment, or other bookable resource. Do not design a single-provider-only product.

Create wireframes for these screens:

1. Marketing homepage
- Hero section with headline: “Turn availability into bookings”
- Subheadline explaining multi-provider scheduling, calendar sync, booking pages, reminders, and waitlist automation
- Primary CTA: “Start free”
- Secondary CTA: “View demo”
- Feature cards: Multi-provider scheduling, Smart availability, Booking pages, Calendar sync, Waitlist fill, Automated reminders, Team permissions, Client intake
- Social proof/testimonial section
- Pricing preview
- Final CTA

2. Signup / onboarding flow
- Create account
- Create workspace
- Choose business type
- Add first service
- Add first provider or invite multiple providers
- Set provider working hours
- Connect provider calendar
- Create first booking page
- Publish booking page

3. Main dashboard
- Sidebar navigation: Dashboard, Calendar, Booking Pages, Appointments, Availability, Waitlist, Clients, Providers, Services, Settings
- Top summary cards: Bookings today, Available slots, Waitlist requests, Provider utilization, No-show rate
- Upcoming appointments list
- Availability by provider
- Recent activity feed
- Waitlist opportunities
- CTA to create a new booking page

4. Provider management screen
- Provider list
- Invite provider button
- Provider profile
- Assigned services
- Connected calendar status
- Working hours
- Location
- Permissions

5. Services screen
- Service list
- Service editor
- Duration
- Price/payment optional
- Location: online, phone, in-person
- Buffer times
- Assigned providers
- Intake questions

6. Availability screen
- Provider selector
- Weekly working hours
- Date-specific overrides
- Holidays/unavailable days
- Location-specific availability
- Calendar conflict status

7. Booking page builder
- Booking type name
- Services included
- Provider selection mode: any provider, client chooses provider, specific provider
- Assignment strategy: first available, round robin, least booked, manual
- Duration
- Price/payment optional
- Location/method
- Availability rules
- Intake questions
- Reminder settings
- Preview panel showing client-facing booking page

8. Client-facing booking page
- Business name and service
- Provider selection step with “Any available provider” and specific provider cards
- Calendar/date picker
- Available time slots
- Client details form
- Confirmation screen
- Option to join waitlist if no slot is available

9. Team schedule view
- Calendar grouped by provider
- Filters for provider, service, location, and status
- Availability gaps
- Booked appointments
- Waitlist opportunities

10. Waitlist screen
- List of clients waiting for openings
- Preferred dates/times
- Service requested
- Provider preference: specific provider or any eligible provider
- Priority/status
- Suggested open slots
- Button: “Offer slot”
- Automation toggle: “Auto-fill cancellations”

11. Appointments screen
- Table/list of booked appointments
- Filters by status, date, service, provider, and location
- Appointment details drawer
- Actions: reschedule, cancel, mark no-show, send reminder

12. Settings screen
- Calendar integrations by provider
- Team members
- Provider permissions
- Branding
- Notifications
- Payments
- Booking rules

Important UX requirements:
- Make the booking flow feel fast and simple.
- Make availability the central concept.
- Multi-provider scheduling must be visible throughout the product.
- Highlight waitlist automation as a premium feature, not the entire product.
- Use clear empty states for new users.
- Include mobile-responsive versions for the client booking page and dashboard.
- Use realistic placeholder data.
- Keep the interface simple enough for non-technical business owners.

Output:
Provide low-fidelity wireframes with labeled sections, layout hierarchy, and brief notes explaining each screen’s purpose and main user actions.
```

