# Smart Campus Room Availability Checker

Client-side React/TypeScript demo for filtering fictional campus rooms, checking availability, and confirming session-only bookings. The demo clock is fixed at 09:00; refresh resets changes. No backend or credentials are needed.

- UI and state: `src/App.tsx` and `src/components/`; entry point: `src/main.tsx`.
- Booking rules and time helpers: `src/lib/booking.ts`; room checks and immutable updates: `src/lib/rooms.ts`; filters: `src/lib/filtering.ts`.
- Fictional catalog and demo clock: `src/data/rooms.ts`; types: `src/types/room.ts`.

## Setup and local commands

Use Node.js 24+ and npm from this repository root. For an authorized initial setup, run `npm ci` using the existing lockfile. Respect requests not to install anything.

- `npm run dev`: start the local Vite app.
- `npm test`: run Vitest once.
- `npm run lint`: check ESLint rules.
- `npm run typecheck`: check TypeScript without emitting files.
- `npm run build`: build locally into `dist/`; this does not deploy.
- `npm run preview`: preview the local build.

## Change discipline

Keep booking and filtering rules in pure `src/lib/` functions, separate from React rendering and event coordination. Reuse the same availability rules for checking and confirmation. Never mutate existing room objects, booking objects, or booking arrays; copy before updating or sorting.

Test changed behavior in `src/lib/*.test.ts` and UI interactions in `src/App.test.tsx`, including boundaries, immutability, and room isolation. For code changes, run tests, lint, typecheck, and build unless the task restricts execution; report anything not run. jsdom tests do not verify real-browser layout or native controls.

## Acceptance criteria

1. Reject invalid intervals and bookings that start at or before the demo clock. Require whole-minute values with `0 <= start < end <= 1440`.
2. Allow 120 minutes if the other rules pass. Reject longer bookings.
3. Reject blocked rooms and overlapping bookings. Allow bookings that touch at an endpoint.
4. Checking availability does not change the bookings. Confirming a booking updates only the selected room without mutating the original data.
5. Room filters require enough capacity and all selected equipment. A matching blocked room stays visible, but users cannot book it.

## Safety and review

Do not access or add secrets or real personal data. Use fictional fixtures only. Do not run destructive commands, apply automatic audit fixes, change dependencies or lockfile resolutions without approval, or deploy to production. Require human review before merging or deploying. Keep changes within this repository; leave the preserved sibling `bolt-campus` untouched.
