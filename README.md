# agent-serve

Find where agents fail in your product, then fix the two failures that matter.

## Install

```bash
npx skills add katrinalaszlo/agent-serve
```

## What this is

A skill that audits your product the way an agent experiences it and tells you what to build at each point of friction.

Agent traffic is no longer hypothetical. Mintlify reported that agents went from 15% to 66% of its docs traffic in seven months (213M agent requests vs 105M human page loads, July 2026). Most of that traffic is reading, not buying, and that's the honest version of the story: agents research products at scale today, and when one tries to go further it usually can't. Signup wants a CAPTCHA it can't solve. Payment wants a browser checkout it can't drive. The agent doesn't complain to you about this. It reports back to its human, or moves on, and nothing shows up in your analytics either way.

Readiness scanners will score you on this. None of them repair it. This skill is the diagnostic half; the repair for the two hardest failures, signup and payment, is [tanso-oss](https://github.com/tansohq/tanso-oss), an open-source engine that adds machine-readable pricing, one-call agent signup, and saved-card payment with a 402 fallback on top of the Stripe stack you already run.

## Usage

```bash
/agent-serve https://example.com    # Audit a live product
/agent-serve                        # Audit from codebase
```

One command audits all six areas: onboarding, auth, purchasing, usage monitoring, self-management, and dev readiness.

## What you get

For each area, the skill tells you:
- **What exists today** — what the product already supports
- **What blocks agents** — the specific friction
- **What to build** — concrete fixes with effort estimates, referencing how companies like Stripe, Cloudflare, and Twilio solved each problem

### Example output

![Example agent-serve audit output](example-output.png)

## The patterns that matter

**Onboarding:** `POST /v1/accounts` returns account ID + API key in one call. No CAPTCHA, no email loop. Deploy-first-claim-later for dev tools.

**Auth:** OAuth Client Credentials for machine-to-machine. Scoped API keys with rotation endpoint. No magic links, no SMS OTP on programmatic paths.

**Purchasing:** Expose what Stripe already supports as API endpoints. Human saves a payment method once (Setup Intent), agent reuses it. `GET /plans` returns the catalog, `POST /subscriptions` creates one. No browser checkout required. Publish `pricing.json` for machine-readable pricing discovery.

**Usage:** Rate limit headers on every response. Dedicated usage endpoint with current-period data. Threshold webhooks so agents can self-throttle.

**Management:** Plan changes, cancellation, configuration — all via API. MCP server as the agent-facing interface for products that are ready.

**Dev ready:** Structured JSON errors with type, code, message, and failing parameter. Idempotency keys on mutating endpoints. Cursor-based pagination. API versioning with deprecation policy. OpenAPI spec published. Test mode with separate keys. llms.txt at domain root. Curated MCP server (10-15 tools, not full API dump). A2A Agent Card if exposing agent-to-agent capabilities.

**Starting from zero:** Pick one read endpoint and ship it. Then one write. Then usage visibility. Then programmatic signup. Four weeks from dashboard-only to agent-possible.

## What blocks agents

- CAPTCHA / reCAPTCHA
- Email verification loops
- SMS OTP
- Browser-only OAuth consent
- "Contact sales" gates
- PDF-only documentation
- Dashboard-only configuration

These fixes are measurable, not cosmetic. Adding an llms.txt alone cut one documentation site's agent 404s from 2.23 to 0.11 per retrieval. The signup and payment fixes are where [tanso-oss](https://github.com/tansohq/tanso-oss) picks up.

## What's next

**Part 2: Is Your Site Ready for AI?** — How agents find and evaluate your product, what they look for, and how to measure whether you're showing up.

**Part 3: Onboarding Agents** — The full agent-ready funnel: what onboarding, auth, purchasing, and account management actually require, where industry recommendations fall short, and what to build.

## Author

Kat Laszlo — [@katlaszlo](https://x.com/Katlaszlo)
