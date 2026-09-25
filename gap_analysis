# Sancus — Three Rails Gap Analysis

**Question:** On each of the three rails, what does the agent leverage that already exists, and what must be built?

**Rails:** Gnani (voice) · Pine Labs (payments) · Delhivery (logistics)

Grounded only in public docs reviewed for this project. If a line does not apply: **None**. No invented endpoints.

**Sources:** [Gnani docs index](https://docs.gnani.ai/llms.txt), [Speech-to-text realtime](https://docs.gnani.ai/api/STT/stt-websocket), [Trigger call](https://docs.gnani.ai/Platform/Trigger_Call.md), [Advanced speech settings](https://docs.gnani.ai/B02_Advanced_ASR.md), [Custom actions](https://docs.gnani.ai/D05_Custom.md), [Dynamic variables](https://docs.gnani.ai/B04_Dynamic_Variables.md); [Pine Labs one-time mandate](https://www.pinelabs.com/docs/online-payments/one-time-mandate), [integration steps](https://www.pinelabs.com/docs/online-payments/one-time-mandate/integration-steps), [payouts](https://www.pinelabs.com/docs/online-payments/api/payouts/create-payout); [Delhivery One logistics portal](https://one.delhivery.com/developer-portal/documents), [Delhivery Maps OpenAPI](https://www.delhivery.com/maps/openapi.json).

**Sancus policy notes:** Passphrase-only speech (binary match). Location is read by the companion app via normal OS permission. Delhivery rail: None.

---



## Why “Sancus”

Sancus was the Roman god of trust, honesty, and oaths — specifically business contracts and binding agreements. Breaking the deal invited financial ruin. The agent applies that rule to scheduled commitments.

---



## Rail: Gnani (voice)

Capability 1  
Name: Outbound test call to a registered number  
Used at: Optional phone-channel audit when the deadline arrives  
Rail: Gnani  
What you send it: Agent identity, phone number, country code, display name, optional client reference  
What comes back: Acceptance that the call is being placed; later conversation logs or post-call hooks  
When it fails: Number not registered for outbound calls; missing environment; invalid phone; misconfigured pre-call or greeting hooks  
What it must never do: Treat a successful trigger as proof the user complied; assume self-serve production dialing for arbitrary users without Gnani enablement  
What exists today: `POST /v1/agents/{botId}/trigger_call` with whitelist rules; docs describe a test-oriented flow  
Build Status (Exists/Must Build): Exists

Capability 2  
Name: Dynamic greeting and pre-call variables  
Used at: Injecting the day’s passphrase and task context before the session speaks  
Rail: Gnani  
What you send it: Backend returns greeting text plus key–value context for the callee  
What comes back: Agent uses that injected context for the session  
When it fails: Hook times out (~10 seconds documented) or returns a bad status — call may not start  
What it must never do: Invent the passphrase without your source of truth  
What exists today: Dynamic Messages and pre-call variables in Agent Builder docs  
Build Status (Exists/Must Build): Exists

Capability 3  
Name: Advanced listening timeouts and noise filtering  
Used at: Silence detection, turn end, noisy rooms during passphrase capture  
Rail: Gnani  
What you send it: Configured limits (initial silence, end silence, max speech, background noise filtering)  
What comes back: Turn end / cancelled input behaviour according to those limits  
When it fails: Limits too short drop real answers; limits too long let idle users stall  
What it must never do: Claim bed-versus-gym or muffled-speech classification (not documented)  
What exists today: Advanced speech recognition settings in Agent Builder  
Build Status (Exists/Must Build): Exists

Capability 4  
Name: Custom on-call and post-call HTTP actions  
Used at: Notifying our orchestrator of pass, fail, silence, or disconnect so payments can run  
Rail: Gnani  
What you send it: Integration URL, method, headers, body fields, trigger timing  
What comes back: Your server’s response; action logs in the console  
When it fails: Timeout or HTTP error — financial step must not assume success  
What it must never do: Debit funds inside Gnani; Gnani only signals our backend  
What exists today: Custom integrations and actions documentation  
Build Status (Exists/Must Build): Exists

Capability 5  
Name: Realtime speech-to-text over a lasting socket  
Used at: In-app microphone path — stream audio and receive transcript segments for passphrase match  
Rail: Gnani  
What you send it: Raw mono pulse-code audio frames at a steady pace; headers for language, sample rate, optional voice-activity tuning  
What comes back: Connected, processing, and transcript events with text and timing  
When it fails: Bad audio format, rate limits, server errors; bursting frames harms activity detection  
What it must never do: Place a phone call by itself — this interface has no telephony  
What exists today: `wss://api.vachana.ai/stt/v3/stream` with documented voice-activity headers  
Build Status (Exists/Must Build): Exists

Capability 6  
Name: Conversation logs, stats, and post-call outcome read  
Used at: After a session — confirm disconnect, transcript, disposition for forfeiture vs pass  
Rail: Gnani  
What you send it: Conversation identity (from logs) or rely on post-call webhook payload  
What comes back: Outcome fields, transcript, audio where enabled  
When it fails: Missing conversation id; delayed webhook  
What it must never do: Skip idempotent handling of duplicate outcome events  
What exists today: Platform conversation logs / stats / audio endpoints documented in the docs index  
Build Status (Exists/Must Build): Exists

Capability 7  
Name: In-app live video session with speaking avatar and subtitles  
Used at: Primary L4 audit interface  
Rail: Gnani  
What you send it: None to Gnani for this surface — our client owns the session  
What comes back: None  
When it fails: Camera or network denied  
What it must never do: Be described as a documented Gnani Agent Builder product feature — public docs cover phone and browser voice tests, not a productized WebRTC avatar product  
What exists today: None in the reviewed Gnani Agent Builder / Speech docs for this exact surface  
Build Status (Exists/Must Build): Must Build

Capability 8  
Name: Passphrase-only matcher (any segment; no free chat)  
Used at: Every audit — binary match of the issued passphrase; ban open dialogue to block prompt injection  
Rail: Gnani  
What you send it: Transcript segments from Gnani speech products  
What comes back: None from Gnani beyond text — match logic is ours  
When it fails: Empty transcript; non-phrase speech → strike / fail per state machine  
What it must never do: Run an empathetic conversational agent over Gnani transcripts  
What exists today: Transcript text exists from Gnani; **matcher and no-chat policy are ours**  
Build Status (Exists/Must Build): Must Build

Capability 9  
Name: Vision plan — frame sampling, front/rear cameras, scene and before-after rules  
Used at: Presence scene assist; Transformation baseline and final delta; anti-cheat challenges  
Rail: Gnani  
What you send it: None to Gnani — frames go to our vision model  
What comes back: None from Gnani  
When it fails: Dark room, covered camera, refused pan, no face on front camera  
What it must never do: Be attributed to Gnani speech products  
What exists today: None in reviewed Gnani docs  
Build Status (Exists/Must Build): Must Build

Capability 10  
Name: Acoustic confidence / muffled-speech auto-escalation to typed passphrase  
Used at: Switching from spoken phrase to typed phrase when speech is unsafe  
Rail: Gnani  
What you send it: None documented as a published “below 70% confidence” field on Agent Builder turns  
What comes back: Transcripts and silence behaviour; not a documented per-utterance confidence score for this rule  
When it fails: None  
What it must never do: Claim Gnani returns a published gym-music or bed-speech confidence score for this policy  
What exists today: Silence timeouts and noise filtering exist; the specified confidence/muffle classifier does not  
Build Status (Exists/Must Build): Must Build

Capability 11  
Name: Production scheduled outbound at an exact clock time for arbitrary users  
Used at: Six o’clock audits at scale  
Rail: Gnani  
What you send it: Would need campaign or scheduler enablement beyond the public test trigger  
What comes back: None self-serve in public docs  
When it fails: Whitelist and test-only constraints  
What it must never do: Assume `trigger_call` alone is a production dialer  
What exists today: Test trigger plus note that campaign management is enabled by contacting Gnani  
Build Status (Exists/Must Build): Must Build

---



## Rail: Pine Labs (payments)

Capability 12  
Name: Access token for server calls  
Used at: Before every payments call  
Rail: Pine Labs  
What you send it: Client identity, secret, client-credentials grant  
What comes back: Bearer token  
When it fails: Bad credentials or expired token  
What it must never do: Ship secrets in the mobile app or browser  
What exists today: Authentication token endpoint in online payments docs  
Build Status (Exists/Must Build): Exists

Capability 13  
Name: Create customer profile  
Used at: First time a user binds payments  
Rail: Pine Labs  
What you send it: Merchant customer reference, name, mobile, email  
What comes back: Customer identity for later subscriptions  
When it fails: Validation errors  
What it must never do: Create uncontrolled duplicate references  
What exists today: Customer create API in one-time mandate integration steps  
Build Status (Exists/Must Build): Exists

Capability 14  
Name: One-time direct subscription and mandate registration (fund block)  
Used at: Commitment setup — after the safety filter passes — block the penalty amount in the user’s bank  
Rail: Pine Labs  
What you send it: One-time plan amount and validity; then payment with mandate create over UPI intent  
What comes back: Subscription and order identities; later active status after user authorizes in their payments app  
When it fails: User refuses pin; amount mismatch; presentation before active  
What it must never do: Treat “created” as funds already locked — wait for active; never lock funds for a goal rejected by the safety filter  
What exists today: One-time mandate product and integration steps (create subscription, create mandate, wait for active)  
Build Status (Exists/Must Build): Exists

Capability 15  
Name: Presentation debit against an active one-time mandate  
Used at: Unhappy path — take the blocked amount into merchant settlement  
Rail: Pine Labs  
What you send it: Active subscription identity and amount within the ceiling  
What comes back: Presentation identity and status; webhooks for charged or failed  
When it fails: Subscription not active; amount above ceiling; duplicate presentation reference  
What it must never do: Send money directly to the peer in this step — debit lands with the merchant  
What exists today: Presentation create and fetch in one-time mandate integration steps  
Build Status (Exists/Must Build): Exists

Capability 16  
Name: Payout to a beneficiary UPI address or bank account  
Used at: After a successful debit — pay the designated peer  
Rail: Pine Labs  
What you send it: Client reference, payee name, amount, mode (including UPI), remarks; VPA or account details per mode  
What comes back: Payment reference and status (may be scheduled)  
When it fails: Auth, validation, conflict, or processing failure  
What it must never do: Be described as an atomic “user bank to peer” escrow hop — funds leave the merchant payout balance  
What exists today: Create payout bank transfer API  
Build Status (Exists/Must Build): Exists

Capability 17  
Name: Programmatic release of an active one-time fund block without capture  
Used at: Happy path — void the block when the user passes  
Rail: Pine Labs  
What you send it: Unknown — no public release request schema in the one-time mandate integration steps reviewed  
What comes back: None documented for that exact operation  
When it fails: None  
What it must never do: Assume cancel-subscription equals mandate release without Pine Labs confirmation  
What exists today: Product text says merchants can release; public integration steps do not publish a release endpoint schema  
Build Status (Exists/Must Build): Must Build

Capability 18  
Name: Orchestration that chains debit then payout as one business forfeiture  
Used at: Every fail path  
Rail: Pine Labs  
What you send it: Ordered calls to presentation then payout, with idempotency keys  
What comes back: Two separate statuses to reconcile  
When it fails: Debit succeeds and payout fails — money sits with merchant until retry  
What it must never do: Hide the two-hop nature from operators or judges  
What exists today: Separate presentation and payout products; no single “forfeit to peer” API  
Build Status (Exists/Must Build): Must Build

Capability 19  
Name: Pass-path do-not-present hold until mandate expiry  
Used at: When release API is unavailable but audit passed  
Rail: Pine Labs  
What you send it: None to Pine Labs beyond not calling presentation  
What comes back: None  
When it fails: Operator error presents by mistake  
What it must never do: Capture after a recorded pass  
What exists today: Operational policy only — not a documented Pine Labs “do not present” API  
Build Status (Exists/Must Build): Must Build

---



## Rail: Delhivery (logistics)

No capabilities of Delhivery utilised. 

---



## Summary


| Rail      | Exists and callable                                                                                                  | Must build                                                                                                                                        |
| --------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Gnani     | Call trigger, dynamic variables, listen timeouts, custom actions, realtime speech socket, conversation outcome reads | In-app video avatar session, passphrase-only matcher policy, vision plans, confidence/muffle escalation as specified, production campaign dialing |
| Pine Labs | Token, customer, one-time fund block, presentation debit, payout to peer                                             | Publicly documented mandate release; two-hop forfeiture orchestrator; do-not-present pass hold                                                    |
| Delhivery | None                                                                                                                 | None                                                                                                                                              |


