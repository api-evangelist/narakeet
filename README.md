# Narakeet (narakeet)

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
