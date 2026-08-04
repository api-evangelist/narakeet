# Narakeet (narakeet)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Narakeet turns text and Markdown scripts into realistic narrated audio and video using AI text-to-speech voices - 900 voices across 100 languages. Beyond its web app, Narakeet exposes a documented REST API (base `https://api.narakeet.com`) for building speech audio, building video from Markdown, listing voices, and checking account credits.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/narakeet/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/narakeet/refs/heads/main/apis.yml)

## Access Model

- **Base URL:** `https://api.narakeet.com`
- **Authentication:** an `x-api-key` header on every build request. The key is created in the account management interface.
- **Eligibility:** API access is only available on **top-up or metered commercial accounts**. Free and unmetered accounts cannot use the API.
- **Quota:** the default limit is **86,400 requests per day (1 per second)**; higher limits are available on request (contact@narakeet.com).
- **Async pattern:** longer audio builds and all video builds are **asynchronous**. The build call returns a JSON object with a `statusUrl`; the client **polls** that pre-signed URL (no API key required) until `finished` is true, then downloads from the `result` URL. This is HTTP polling, not a WebSocket or Server-Sent Events stream. Short audio builds instead return the audio bytes directly in a single synchronous response.

## Tags

- Text to Speech
- TTS
- Voice
- Audio
- Video
- AI
- Media Generation

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### Narakeet Text to Speech API

Convert text, SubRip, or WebVTT input into narrated audio in MP3, M4A, or WAV. Two modes share the same `POST /text-to-speech/{format}` endpoint, selected by the `Accept` header:

- **Short content (streaming):** `Accept: application/octet-stream`, input up to ~1 KB, must complete within ~30 seconds. Returns audio bytes directly; duration is in the `x-duration-seconds` response header. (WAV is not available in this mode.)
- **Long content (JSON polling):** no octet-stream Accept header, input up to ~1024 KB, may take up to ~45 minutes. Returns a JSON `statusUrl` to poll.

Voice, speed, and volume are set via query parameters (`voice`, `voice-speed`, `voice-volume`). `Accept: application/zip` produces batch audio from multi-scene input (scenes separated by `---`), and `Accept: application/json+vtt` also returns subtitle result URLs.

- **Human URL:** [https://www.narakeet.com/docs/automating/text-to-speech-api/](https://www.narakeet.com/docs/automating/text-to-speech-api/)
- **Base URL:** `https://api.narakeet.com`

#### Tags

- Text to Speech
- TTS
- Audio

#### Properties

- [Documentation](https://www.narakeet.com/docs/automating/text-to-speech-api/)
- [API Reference](https://www.narakeet.com/docs/automating/text-to-speech-api/)
- [OpenAPI](openapi/narakeet-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/narakeet.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/narakeet.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Narakeet Markdown to Video API

Build narrated video from a Markdown or text script plus its assets. Request a pre-signed upload token (`GET /video/upload-request/zip`), `PUT` a zip archive of the script and media to the returned URL, trigger a build (`POST /video/build`), then poll the returned `statusUrl` for the finished video and poster image.

- **Human URL:** [https://www.narakeet.com/docs/automating/video-api/](https://www.narakeet.com/docs/automating/video-api/)
- **Base URL:** `https://api.narakeet.com`

#### Tags

- Video
- Markdown
- Media Generation

#### Properties

- [Documentation](https://www.narakeet.com/docs/automating/video-api/)
- [API Reference](https://www.narakeet.com/docs/automating/video-api/)
- [OpenAPI](openapi/narakeet-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/narakeet.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/narakeet.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Narakeet Voice Listing API

Retrieve the JSON list of voices available to your account (`GET /voices`) - each with a name, language description, locale code, and supported narration styles - so integrations can keep an up-to-date voice picker. Does not consume credits but counts toward the daily request quota; caching (once per day) is recommended.

- **Human URL:** [https://www.narakeet.com/docs/automating/listing-voices-api/](https://www.narakeet.com/docs/automating/listing-voices-api/)
- **Base URL:** `https://api.narakeet.com`

#### Tags

- Voices
- Languages
- Catalog

#### Properties

- [Documentation](https://www.narakeet.com/docs/automating/listing-voices-api/)
- [API Reference](https://www.narakeet.com/docs/automating/listing-voices-api/)
- [OpenAPI](openapi/narakeet-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/narakeet.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/narakeet.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Narakeet Account Credits API

Check the credit seconds still available on your account (`GET /account/credits`), along with the billing plan and the identity of the API key. Useful for gating jobs and surfacing remaining balance before starting a long audio or video build.

- **Human URL:** [https://www.narakeet.com/docs/automating/account-credits-api/](https://www.narakeet.com/docs/automating/account-credits-api/)
- **Base URL:** `https://api.narakeet.com`

#### Tags

- Account
- Credits
- Billing

#### Properties

- [Documentation](https://www.narakeet.com/docs/automating/account-credits-api/)
- [API Reference](https://www.narakeet.com/docs/automating/account-credits-api/)
- [OpenAPI](openapi/narakeet-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/narakeet.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/narakeet.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/narakeet)
- [Website](https://www.narakeet.com)
- [Documentation](https://www.narakeet.com/docs/automating/)
- [Plans](plans/narakeet-plans-pricing.yml)
- [Rate Limits](rate-limits/narakeet-rate-limits.yml)
- [Fin Ops](finops/narakeet-finops.yml)
- [Blog](https://www.narakeet.com/news/)

## Pricing

Narakeet bills on a **top-up credit model** measured by the duration of audio/video produced; credits never expire. The confirmed entry pack is **6 USD for 30 minutes (~$0.20/minute)**, with larger packs (300, 1000, 2500, 10,000 minutes) offering progressively lower per-minute rates. A free-forever tier exists for evaluation but has **no API access and no commercial use**.

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
