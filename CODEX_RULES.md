# CODEX_RULES.md — AvailHive Engineering Rules

## Purpose

This file defines engineering rules for building AvailHive. These rules are intended for coding agents, contributors, and reviewers.

AvailHive is a multi-provider booking SaaS. The system must never be implemented as a single-provider-only product.

## Rule 1 — Multi-provider is mandatory

Every scheduling feature must support multiple providers.

Required behavior:

- Appointments must belong to a provider.
- Services must be assignable to multiple providers.
- Availability must be computed per provider.
- Booking pages must support provider selection rules.
- Waitlist entries must support provider preference.
- Calendar connections must belong to providers.
- Dashboard and reports must support provider filters.

Do not create MVP shortcuts that assume one provider per workspace.

## Rule 2 — Workspace scoping is mandatory

Every business record must be scoped to a workspace.

All database queries must enforce workspace ownership or membership.

Applies to:

- Providers
- Services
- Clients
- Appointments
- Waitlist entries
- Booking pages
- Locations
- Calendar connections
- Notifications
- Payments
- Settings

Never trust a workspaceId from the client without verifying the authenticated user has access to that workspace.

## Rule 3 — Provider is a domain entity, not just a user

Providers can be:

- Authenticated users
- Invited team members
- Staff profiles managed by an admin
- Rooms
- Equipment
- Other bookable resources

Therefore:

- Do not use userId as a replacement for providerId.
- Do not assume every provider can log in.
- Do not assume every provider has an email.
- Do not assume every user is a provider.

## Rule 4 — Appointments require provider, service, client, and workspace

A valid appointment must include:

- workspaceId
- providerId
- serviceId
- clientId
- startAt
- endAt
- status

Appointments without providerId are invalid unless they are temporary drafts created before final assignment.

## Rule 5 — Service-provider eligibility must be enforced

Before booking, verify:

- Provider is active.
- Service is active.
- Provider is assigned to the service.
- Booking page exposes the service.
- Booking page allows the selected provider flow.
- Provider location/method is compatible with service.

Do not rely only on front-end filtering.

## Rule 6 — Availability must be server-authoritative

The server must recompute and validate availability at booking time.

Never trust a slot selected on the client without rechecking:

- Provider availability rules
- Provider overrides
- Existing appointments
- External calendar busy blocks
- Buffers
- Service duration
- Minimum notice
- Maximum booking window
- Location compatibility
- Provider assignment

## Rule 7 — External calendar details are private

Calendar sync can use busy blocks to determine availability, but public users must not see private event details.

Public booking pages may show:

- Available dates
- Available times
- Provider name/profile if enabled

Public booking pages must not show:

- External calendar event titles
- External calendar event attendees
- External calendar locations
- Private provider notes
- Calendar tokens

## Rule 8 — Permissions must be enforced server-side

Client-side hiding is not security.

Every server action, API route, loader, and mutation must verify authorization.

Suggested permission model:

- Owner: full access.
- Admin: operational access across workspace.
- Provider: access to own profile, availability, appointments, and calendar connections.
- Staff: limited access based on workspace settings.

Providers should not access another provider’s schedule unless granted permission.

## Rule 9 — Booking pages must support three provider modes

Booking pages should support:

1. Any provider only
2. Specific provider only
3. Client chooses provider

For any-provider bookings, the assignment strategy must be explicit:

- First available
- Round robin
- Least booked
- Manual

Do not silently assign providers with hidden or inconsistent logic.

## Rule 10 — Waitlist must support provider preference

Waitlist entries must support:

- Specific provider
- Any eligible provider

Waitlist matching must respect:

- Service
- Provider eligibility
- Preferred provider
- Preferred date range
- Preferred time windows
- Location/method
- Priority
- Offer expiration

## Rule 11 — Timezones must be explicit

Store timestamps in UTC where appropriate.

Display times in the relevant timezone:

- Workspace timezone for business dashboard defaults.
- Provider timezone for provider-specific scheduling.
- Client-local timezone on public booking pages when appropriate.

Availability rules should store local recurring times with timezone context.

Always test daylight saving time boundaries.

## Rule 12 — Prevent double booking

Before creating or rescheduling an appointment, verify there is no conflict with:

- Existing appointments for the provider
- Provider buffer windows
- External calendar busy blocks
- Availability overrides
- Service/location constraints

Use database transactions or locking where needed to avoid race conditions.

## Rule 13 — Keep public booking fast

The public booking flow should be optimized for speed.

Requirements:

- Minimal required fields.
- Clear service selection.
- Clear provider selection.
- Fast date/time slot loading.
- Mobile-first layout.
- Helpful empty states.
- Waitlist option when no slots are available.

## Rule 14 — Design for non-technical users

AvailHive is for business owners and staff, not only technical users.

Prefer plain language:

- “Provider” or “Team member” instead of “resource entity.”
- “Available times” instead of “slot matrix.”
- “Booking page” instead of “public scheduling endpoint.”
- “Waitlist” instead of “deferred fulfillment queue.”

## Rule 15 — Use domain-driven names

Good names:

```text
workspaceId
providerId
serviceId
clientId
bookingPageId
availabilityRule
calendarConnection
waitlistEntry
slotOffer
assignmentStrategy
```

Avoid ambiguous names:

```text
ownerCalendar
mySchedule
selectedUser
singleProvider
bookingUser
mainProvider
```

## Rule 16 — Add empty, loading, and error states

Every major screen must include:

- Loading state
- Empty state
- Error state
- Permission-denied state where relevant

Important empty states:

- No providers
- No services
- No appointments
- No availability
- No calendar connected
- No waitlist entries

## Rule 17 — Accessibility is required

UI must include:

- Semantic HTML
- Keyboard navigation
- Visible focus states
- Labels for inputs
- ARIA only where useful
- Sufficient color contrast
- Screen-reader-friendly status messages

Calendar and time-slot selection must be usable without a mouse.

## Rule 18 — Testing requirements

Every scheduling change should include tests for:

- Single-provider workspace
- Multi-provider workspace
- Specific-provider booking
- Any-provider booking
- Service assigned to multiple providers
- Service not assigned to selected provider
- Calendar conflict
- Existing appointment conflict
- Buffers
- Waitlist match
- Permission boundary

## Rule 19 — No sensitive data leakage

Do not log:

- Calendar tokens
- Payment data
- Client intake sensitive answers
- Private provider notes
- Personal calendar event details

Use redaction for structured logs.

## Rule 20 — MVP quality bar

An MVP feature is not complete unless:

- It works for a workspace with at least two providers.
- It respects workspace permissions.
- It handles empty states.
- It handles common validation errors.
- It is mobile-friendly where client-facing.
- It has tests for core logic.

