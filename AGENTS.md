# AGENTS.md — AvailHive

## Project

AvailHive is a multi-provider booking and availability management SaaS for service businesses. It helps businesses turn provider availability into confirmed appointments by supporting calendar sync, booking pages, appointment management, reminders, payments, client intake, and waitlist automation.

The product must support solo providers and multi-provider businesses from the beginning.

Examples of providers:

- Staff members
- Consultants
- Clinicians
- Stylists
- Coaches
- Technicians
- Rooms
- Equipment
- Other bookable resources

## Core product principle

Do not assume a business has only one provider.

All booking, availability, appointment, waitlist, calendar-sync, notification, reporting, and permission logic must support multiple providers under one workspace.

## Standing agent rules

### 1. Multi-provider by default

When creating or modifying scheduling logic:

- Always include provider context.
- A workspace may have one provider or many providers.
- Services may be assigned to one provider, multiple providers, or all providers.
- Availability must be computed per provider.
- Appointments must reference a provider.
- Waitlist entries must support both provider-specific and provider-flexible preferences.
- Calendar sync must be provider-specific.
- Dashboard metrics must support business-wide and provider-level views.

### 2. Workspace-first architecture

All user-facing business data must belong to a workspace.

Use this hierarchy:

```text
Workspace
  Providers
  Services
  Booking Pages
  Clients
  Appointments
  Waitlist Entries
  Calendar Connections
  Locations
  Team Members
  Settings
```

Never store business data without a workspace owner or workspace association.

### 3. Provider and user are not always the same thing

A provider may be a user account, but does not have to be.

Examples:

- A solo consultant is both user and provider.
- A clinic admin manages providers who may or may not log in.
- A room or equipment item can be a provider/resource but not a human user.

Do not hard-code provider behavior as authenticated user behavior.

### 4. Booking must respect service eligibility

A client can only book a provider if:

- The provider is active.
- The provider is assigned to the selected service.
- The provider is available during the selected slot.
- The provider has no calendar conflicts.
- The location or meeting method is valid for that provider and service.
- Buffers, minimum notice, maximum future booking window, holidays, and business rules are satisfied.

### 5. Support specific-provider and any-provider flows

Client booking pages must support both:

- Choose a specific provider.
- Choose any available provider.

For any-provider booking, the system should assign a provider using a defined assignment strategy such as:

- First available
- Round robin
- Least booked
- Highest priority
- Manual selection by admin

The selected strategy must be explicit and testable.

### 6. Waitlist support

Waitlist logic must support:

- Waiting for a specific provider.
- Waiting for any eligible provider.
- Waiting for a specific service.
- Preferred dates and times.
- Preferred location or meeting method.
- Priority ranking.
- Manual and automated slot offers.
- Expiring slot offers.

Do not build waitlist logic as a single global queue unless explicitly scoped that way.

### 7. Calendar sync rules

Each provider may connect one or more calendars.

Calendar sync must:

- Read busy blocks from connected calendars.
- Prevent double-booking.
- Create or update appointment events when required.
- Respect provider-level calendar settings.
- Handle disconnected calendars gracefully.
- Avoid exposing private calendar details to clients.

Only availability should be exposed publicly, not calendar event titles or personal details.

### 8. Permissions and roles

Support role-based access control.

Suggested roles:

- Owner: full access to workspace billing, settings, team, data, and bookings.
- Admin: manages scheduling, providers, services, clients, and appointments.
- Provider: manages own availability, appointments, profile, and calendar connections.
- Staff: limited operational access assigned by workspace settings.

Providers should not automatically see all business data unless granted permission.

### 9. Data privacy and security

Agents must avoid exposing private data between providers, clients, or workspaces.

Requirements:

- Enforce workspace scoping on every query.
- Enforce role permissions in API handlers and server actions.
- Never trust client-provided workspaceId, providerId, or userId without authorization checks.
- Store secrets and calendar tokens securely.
- Avoid logging sensitive client intake answers, calendar tokens, payment details, or health-related data.

### 10. UI consistency

All UI should follow the AvailHive design language:

- Clean SaaS interface
- Soft rounded cards
- Clear hierarchy
- Calm neutral palette
- Strong primary calls to action
- Fast booking flow
- Mobile-friendly client booking page
- Accessibility-conscious contrast, labels, focus states, and keyboard navigation

### 11. Code quality expectations

Every implementation should be:

- Type-safe
- Modular
- Testable
- Readable
- Server-authorized
- Accessible
- Mobile-responsive where relevant

Prefer clear domain names over clever abstractions.

### 12. Do not create single-provider shortcuts

Avoid naming and logic like:

```text
userAvailability
myCalendarOnly
businessOwnerAsProvider
appointment.userId as provider
singleProviderBookingPage
```

Prefer:

```text
providerAvailability
providerCalendarConnections
workspaceProviders
appointment.providerId
bookingPage.providerSelectionMode
```

## Recommended domain vocabulary

Use these terms consistently:

- Workspace: business account or organization.
- Provider: bookable person or resource.
- Service: bookable offering.
- Appointment: confirmed booking.
- Booking Page: public page where clients schedule appointments.
- Availability Rule: recurring or date-specific availability.
- Calendar Connection: external calendar integration for a provider.
- Waitlist Entry: client request for future/open slot.
- Slot Offer: offered appointment slot sent to a waitlisted client.
- Client: person booking the appointment.
- Team Member: authenticated user in a workspace.

## Implementation checklist for agents

Before completing scheduling-related work, verify:

- Does the code support multiple providers?
- Is providerId required where needed?
- Is workspace scoping enforced?
- Are service-provider assignments respected?
- Are provider calendar conflicts checked?
- Are buffers and booking rules applied?
- Does the UI support provider filtering or provider selection where appropriate?
- Are permissions enforced server-side?
- Are empty states included?
- Are tests included or updated?

