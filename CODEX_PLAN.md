# CODEX_PLAN.md — AvailHive Implementation Plan

## Product summary

AvailHive is a multi-provider booking and availability management SaaS that helps businesses turn availability into bookings.

Core capabilities:

- Multi-provider scheduling
- Public booking pages
- Calendar sync
- Service and provider management
- Appointment management
- Availability rules
- Waitlist and cancellation filling
- Client intake forms
- Reminders and notifications
- Team roles and permissions
- Dashboard and reporting

The product must support both solo users and businesses with many providers.

## North-star user flow

1. Business owner creates a workspace.
2. Owner adds services.
3. Owner adds or invites providers.
4. Providers set working hours and connect calendars.
5. Owner creates a booking page.
6. Client selects a service.
7. Client chooses a specific provider or any available provider.
8. Client selects an available slot.
9. Client submits intake details.
10. Appointment is confirmed.
11. Notifications and calendar events are created.
12. If no slots are available, client can join the waitlist.
13. Cancellations can trigger waitlist slot offers.

## Core entities

### Workspace

Represents the business account.

Suggested fields:

- id
- name
- slug
- timezone
- defaultCurrency
- ownerUserId
- plan
- createdAt
- updatedAt

### User

Represents an authenticated account.

Suggested fields:

- id
- name
- email
- avatarUrl
- createdAt
- updatedAt

### TeamMember

Connects a user to a workspace and role.

Suggested fields:

- id
- workspaceId
- userId
- role
- status
- invitedEmail
- invitedAt
- joinedAt
- createdAt
- updatedAt

Roles:

- owner
- admin
- provider
- staff

### Provider

A bookable person or resource.

Suggested fields:

- id
- workspaceId
- userId nullable
- name
- displayName
- roleTitle
- bio
- avatarUrl
- email
- phone
- type: person, room, equipment, other
- status: active, inactive, archived
- timezone
- defaultLocationId nullable
- bookingPriority
- createdAt
- updatedAt

### Service

A bookable offering.

Suggested fields:

- id
- workspaceId
- name
- description
- durationMinutes
- priceAmount nullable
- currency nullable
- locationMode: online, phone, in_person, flexible
- bufferBeforeMinutes
- bufferAfterMinutes
- minNoticeMinutes
- maxBookingDaysAhead
- status
- createdAt
- updatedAt

### ProviderService

Many-to-many mapping between providers and services.

Suggested fields:

- id
- workspaceId
- providerId
- serviceId
- isActive
- customDurationMinutes nullable
- customPriceAmount nullable
- createdAt
- updatedAt

### Location

Physical or virtual service location.

Suggested fields:

- id
- workspaceId
- name
- type: physical, online, phone
- address nullable
- meetingUrl nullable
- phoneNumber nullable
- instructions nullable
- createdAt
- updatedAt

### AvailabilityRule

Recurring provider availability.

Suggested fields:

- id
- workspaceId
- providerId
- dayOfWeek
- startTime
- endTime
- timezone
- locationId nullable
- effectiveFrom nullable
- effectiveUntil nullable
- isActive
- createdAt
- updatedAt

### AvailabilityOverride

Date-specific availability exception.

Suggested fields:

- id
- workspaceId
- providerId
- date
- startTime nullable
- endTime nullable
- type: available, unavailable
- reason nullable
- createdAt
- updatedAt

### CalendarConnection

External calendar connection for a provider.

Suggested fields:

- id
- workspaceId
- providerId
- providerName: google, outlook, apple, other
- externalAccountEmail
- accessTokenEncrypted
- refreshTokenEncrypted
- syncStatus
- lastSyncedAt
- createdAt
- updatedAt

### BookingPage

Public scheduling page.

Suggested fields:

- id
- workspaceId
- slug
- title
- description
- status
- providerSelectionMode: any_provider, specific_provider, client_choice
- assignmentStrategy: first_available, round_robin, least_booked, manual
- brandColor
- logoUrl
- createdAt
- updatedAt

### BookingPageService

Services exposed on a booking page.

Suggested fields:

- id
- workspaceId
- bookingPageId
- serviceId
- sortOrder
- createdAt
- updatedAt

### Client

Person who books an appointment.

Suggested fields:

- id
- workspaceId
- name
- email
- phone
- notes
- createdAt
- updatedAt

### Appointment

Confirmed booking.

Suggested fields:

- id
- workspaceId
- providerId
- serviceId
- clientId
- bookingPageId nullable
- locationId nullable
- startAt
- endAt
- timezone
- status: confirmed, cancelled, completed, no_show, rescheduled
- source: public_booking, admin_created, waitlist, import
- intakeAnswersJson
- cancellationReason nullable
- externalCalendarEventId nullable
- createdAt
- updatedAt

### WaitlistEntry

Client request for an opening.

Suggested fields:

- id
- workspaceId
- clientId
- serviceId
- preferredProviderId nullable
- providerPreferenceMode: specific_provider, any_eligible_provider
- preferredDateStart nullable
- preferredDateEnd nullable
- preferredTimeWindowsJson
- locationId nullable
- priority
- status: waiting, offered, booked, expired, cancelled
- notes
- createdAt
- updatedAt

### SlotOffer

A slot offered to a waitlisted client.

Suggested fields:

- id
- workspaceId
- waitlistEntryId
- providerId
- serviceId
- appointmentStartAt
- appointmentEndAt
- expiresAt
- status: pending, accepted, declined, expired, cancelled
- token
- createdAt
- updatedAt

### Notification

Message or reminder event.

Suggested fields:

- id
- workspaceId
- appointmentId nullable
- waitlistEntryId nullable
- recipientType: client, provider, admin
- channel: email, sms, push
- status
- scheduledFor
- sentAt nullable
- templateKey
- createdAt
- updatedAt

## Implementation phases

## Phase 1 — Foundation

### Goals

- Set up project architecture.
- Create authentication and workspace model.
- Establish role-based permissions.
- Create reusable UI shell.

### Tasks

- Create app routes and layout.
- Add authentication.
- Add workspace creation flow.
- Add team member model.
- Add provider model.
- Add role-based authorization helpers.
- Add database schema and migrations.
- Add seed data for a multi-provider business.

### Acceptance criteria

- A user can create a workspace.
- A workspace can have multiple providers.
- A provider can optionally be linked to a user.
- Role checks are enforced server-side.
- Dashboard shell renders with empty states.

## Phase 2 — Services and provider management

### Goals

- Let businesses configure providers and services.
- Support service-provider assignments.

### Tasks

- Provider list screen.
- Provider profile/edit screen.
- Invite provider flow.
- Provider activation/deactivation.
- Service list screen.
- Service creation/edit screen.
- Assign services to providers.
- Add default buffers, booking windows, and service locations.

### Acceptance criteria

- Admin can create multiple providers.
- Admin can assign one service to many providers.
- Admin can assign one provider to many services.
- Inactive providers are hidden from public booking.
- Provider permissions are respected.

## Phase 3 — Availability engine

### Goals

- Compute available appointment slots per provider.
- Support specific-provider and any-provider availability.

### Tasks

- Availability rule CRUD.
- Availability override CRUD.
- Slot generation utility.
- Conflict detection against appointments.
- Conflict detection against connected calendar busy blocks.
- Buffer handling.
- Min notice and max booking window handling.
- Location compatibility handling.
- Provider eligibility filtering.

### Availability algorithm

For a selected service and date range:

1. Find active providers assigned to the service.
2. Apply provider selection filter if a specific provider was chosen.
3. Load provider availability rules.
4. Apply date-specific overrides.
5. Generate candidate slots based on service duration.
6. Apply buffers.
7. Remove slots outside booking window.
8. Remove slots conflicting with existing appointments.
9. Remove slots conflicting with external calendar busy blocks.
10. Remove slots incompatible with selected location or meeting method.
11. Return slots grouped by provider or merged for any-provider booking.

### Acceptance criteria

- Availability is calculated per provider.
- “Any available provider” returns valid combined availability.
- “Specific provider” only returns slots for that provider.
- Existing appointments block future bookings.
- Buffers are respected.
- Timezone behavior is tested.

## Phase 4 — Booking pages

### Goals

- Create public booking pages that support multi-provider scheduling.

### Tasks

- Booking page builder.
- Booking page service selection.
- Provider selection mode:
  - Any provider only
  - Specific provider only
  - Client chooses provider
- Public client booking flow.
- Client form and intake questions.
- Confirmation screen.
- Appointment creation.
- Calendar event creation placeholder.
- Email confirmation placeholder.

### Acceptance criteria

- Client can select a service.
- Client can select a provider or any provider based on page settings.
- Client sees only real available slots.
- Appointment is created with providerId, serviceId, clientId, and workspaceId.
- Client cannot book unavailable or conflicting slots.

## Phase 5 — Appointments dashboard

### Goals

- Let teams manage appointments.

### Tasks

- Appointment list.
- Filters by provider, service, location, status, and date.
- Appointment detail drawer.
- Reschedule action.
- Cancel action.
- Mark no-show action.
- Mark completed action.
- Manual appointment creation.
- Provider-specific views.

### Acceptance criteria

- Admin can view all appointments.
- Provider can view own appointments unless granted broader permission.
- Appointments can be filtered by provider.
- Rescheduling recomputes valid slots.
- Cancellations can trigger waitlist opportunities.

## Phase 6 — Calendar sync

### Goals

- Connect external calendars per provider.
- Use external busy blocks in availability.

### Tasks

- Google Calendar integration.
- Outlook integration placeholder.
- Provider calendar connection screen.
- Busy block sync.
- Appointment event creation.
- Calendar disconnect handling.
- Token refresh handling.

### Acceptance criteria

- Each provider can connect their own calendar.
- Busy external events block booking availability.
- Appointment confirmation can create an external calendar event.
- Private calendar event details are not exposed publicly.

## Phase 7 — Waitlist automation

### Goals

- Let clients join a waitlist when no matching slot is available.
- Offer open slots to waitlisted clients.

### Tasks

- Waitlist entry creation from booking page.
- Waitlist management screen.
- Specific-provider waitlist entries.
- Any-provider waitlist entries.
- Cancellation-triggered slot matching.
- Manual slot offer flow.
- Auto-fill cancellation toggle.
- Slot offer expiration.
- Waitlist status tracking.

### Slot matching algorithm

When a slot opens:

1. Identify service and provider for the open slot.
2. Find waitlist entries for the same service.
3. Include entries for this specific provider.
4. Include entries that allow any eligible provider.
5. Filter by preferred date and time windows.
6. Filter by location or meeting method.
7. Sort by priority and created date.
8. Offer the slot manually or automatically based on settings.
9. Expire offer if not accepted in time.

### Acceptance criteria

- Waitlist entries can be provider-specific or provider-flexible.
- Admin can offer slots manually.
- Auto-fill can identify eligible waitlist entries.
- Accepted slot offers create appointments.
- Expired offers do not block the slot permanently.

## Phase 8 — Notifications and reminders

### Goals

- Send confirmations, reminders, cancellations, and waitlist offers.

### Tasks

- Notification templates.
- Email provider integration.
- SMS integration placeholder.
- Reminder scheduling.
- Provider notifications.
- Client notifications.
- Waitlist offer notifications.

### Acceptance criteria

- Client receives booking confirmation.
- Provider receives appointment notification.
- Reminders are scheduled relative to appointment time.
- Waitlist offers can be sent and expire.

## Phase 9 — Payments

### Goals

- Optional payment collection for bookable services.

### Tasks

- Stripe integration.
- Service price configuration.
- Payment required/optional toggle.
- Deposit support placeholder.
- Payment status on appointments.
- Refund/cancellation policy placeholder.

### Acceptance criteria

- Services can have prices.
- Booking flow can require payment before confirmation.
- Appointment stores payment status.

## Phase 10 — Reporting and polish

### Goals

- Help businesses understand booking performance.

### Tasks

- Dashboard metrics.
- Provider utilization.
- Booking conversion.
- Waitlist fill rate.
- No-show rate.
- Revenue summary if payments are enabled.
- Empty states.
- Loading states.
- Error states.
- Mobile optimization.
- Accessibility pass.

### Acceptance criteria

- Dashboard supports business-wide and provider-level views.
- Reports can be filtered by provider and service.
- Main flows work on mobile and desktop.

## Suggested route map

```text
/app
  /dashboard
  /calendar
  /appointments
  /booking-pages
  /booking-pages/[id]
  /providers
  /providers/[id]
  /services
  /availability
  /waitlist
  /clients
  /settings
  /settings/team
  /settings/calendars
  /settings/notifications
  /settings/payments

/book/[workspaceSlug]
/book/[workspaceSlug]/[bookingPageSlug]
/book/[workspaceSlug]/[bookingPageSlug]/confirm
/book/[workspaceSlug]/[bookingPageSlug]/waitlist
```

## Testing plan

### Unit tests

- Provider eligibility filtering.
- Slot generation.
- Buffer calculation.
- Timezone conversion.
- Appointment conflict detection.
- Calendar busy block conflict detection.
- Waitlist matching.
- Assignment strategies.

### Integration tests

- Create workspace with multiple providers.
- Assign services to providers.
- Book specific provider.
- Book any available provider.
- Provider calendar conflict blocks slot.
- Cancellation triggers waitlist match.
- Provider role cannot access another provider’s private data.

### End-to-end tests

- New business onboarding.
- Create provider, service, and booking page.
- Client books appointment.
- Client joins waitlist when no slots exist.
- Admin offers waitlist slot.
- Provider views own schedule.

## Non-negotiable requirements

- Multi-provider support is required in MVP.
- ProviderId must be present on appointments.
- Services must support many-to-many provider assignment.
- Availability must be computed per provider.
- Calendar connections must be provider-specific.
- Waitlist must support specific-provider and any-provider preferences.
- Authorization must be enforced server-side.
- Public booking flow must be mobile-friendly.

