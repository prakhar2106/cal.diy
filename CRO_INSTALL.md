# Install cro

This PR wires the cro web SDK into your app for analytics tracking.

**App ID:** `01d60639152027efb39d025a37f635e7`

## What this PR adds

- `cro.config.json` — your App ID + capture options.
- `CRO_INSTALL.md` — these install instructions.

## What you do next

1. `pnpm add @cro/web-sdk` (or npm/yarn equivalent).
2. Wrap your app:

```tsx
// pages/_app.tsx
import { CroProvider } from '@cro/web-sdk/react';
import config from '../cro.config.json';

export default function App({ Component, pageProps }) {
  return (
    <CroProvider appId={config.app_id} options={config.capture}>
      <Component {...pageProps} />
    </CroProvider>
  );
}
```

3. Merge this PR. cro starts ingesting events on the next deploy.

## Capture wrapper (optional)

For typed events, drop in a small wrapper:

```ts
// lib/cro.ts
import { capture as croCapture } from '@cro/web-sdk';

type EventMap = {
  signup_form_submitted: { plan: 'free' | 'pro' };
  booking_created: { service_id: string; revenue_cents: number };
};

export function capture<E extends keyof EventMap>(name: E, props: EventMap[E]): void {
  croCapture(name, props);
}
```

cro detects the typed events and proposes funnels against them automatically.
