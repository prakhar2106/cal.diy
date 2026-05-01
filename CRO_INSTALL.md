# Install cro

This PR wires PostHog autocapture into `prakhar2106/cal.diy` so cro can ingest pageviews, clicks, and route changes.

**App ID:** `01d60639152027efb39d025a37f635e7`
**Framework detected:** `next-mixed`

## What this PR does

- Adds `cro.config.json` (App ID + your PostHog host/key + capture options).
- Adds `app/cro-tracker.tsx` — a tiny client component that initializes PostHog on mount.
- Adds `posthog-js` to your `package.json` (if it wasn't already there).
- cro could not safely patch your root layout — the file shape was unfamiliar. Add this one line yourself:

```tsx
import { CroTracker } from './cro-tracker';
// ... and inside <body> or your root provider tree:
<CroTracker />
```

## After merge

1. `pnpm install` (or your package manager equivalent) to pick up `posthog-js`.
2. Deploy. cro detects events on the next cron tick — usually within 10 minutes.

## How to add typed events (optional)

```ts
// lib/cro.ts
import posthog from 'posthog-js';

type EventMap = {
  signup_form_submitted: { plan: 'free' | 'pro' };
  booking_created: { service_id: string; revenue_cents: number };
};

export function capture<E extends keyof EventMap>(name: E, props: EventMap[E]): void {
  posthog.capture(name, props);
}
```

cro detects typed events and proposes funnels against them automatically.
