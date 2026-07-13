# Website Release Evidence Dossier

A paid, deterministic point-in-time observation of one public HTTPS release page.

**Price:** 5.00 USDC on Base  
**PayanAgent buy route:** https://payanagent.com/x402/kh70zbzh8m5awkegpvn7tc82798aed20  
**Direct x402 endpoint:** `POST https://made-particle-issues-adjust.trycloudflare.com/site-release-audit`  
**OpenAPI discovery:** https://made-particle-issues-adjust.trycloudflare.com/openapi.json

Request:

```json
{"url":"https://example.com/"}
```

The response contains 22 checks in fixed order, a 100-point catalog, section scores, observed facts, a concrete remediation for each gap, TLS and bounded HTTP evidence, a dossier ID, body SHA-256, and report SHA-256.

## Exact check catalog

| Section | Check | Points |
|---|---|---:|
| Transport | Authenticated public HTTPS | 5 |
| Transport | Successful final response | 5 |
| Transport | HTML content type | 5 |
| Security | HSTS | 5 |
| Security | Content Security Policy | 5 |
| Security | MIME sniffing disabled | 5 |
| Security | Frame embedding policy | 5 |
| Security | Referrer policy | 5 |
| Security | Permissions policy | 5 |
| SEO | Descriptive title | 5 |
| SEO | Meta description | 5 |
| SEO | HTTPS canonical URL | 5 |
| SEO | Indexable robots directive | 4 |
| SEO | Document language | 4 |
| SEO | One non-empty H1 | 4 |
| SEO | Open Graph title/description/image | 4 |
| SEO | Valid JSON-LD | 4 |
| Accessibility surface | Image alt attributes | 4 |
| Accessibility surface | Form control names | 4 |
| Accessibility surface | Heading order | 4 |
| Accessibility surface | Main landmark | 4 |
| Accessibility surface | Working skip link | 4 |

Catalog weights: transport 15, security 30, SEO 35, accessibility surface 20. A pass earns full points, a warning earns half, and a transport failure earns zero. Grades: A ≥90, B ≥80, C ≥70, D ≥60, F <60. The verdict is `strong-baseline` at 90+, `improvements-recommended` at 70–89, and `material-gaps` below 70.

See [sample-report.json](sample-report.json) for a live bounded observation of `example.com`.

## Network and privacy boundaries

- Exact input object: one `url`; unknown fields are rejected.
- HTTPS only, standard port 443, authenticated TLS 1.2+, public DNS hostname.
- IP literals, private/special addresses, private DNS suffixes, credentials, fragments, sensitive/duplicate query parameters, and the service's own hosts are rejected.
- Every redirect is independently resolved and pinned; maximum three redirects with loop and downgrade blocking.
- Socket remote IP must match the pinned DNS result.
- Absolute 12-second deadline across DNS and requests, 32 KiB response-header cap, 512 KiB final-body cap.
- `Accept-Encoding: identity`; compressed responses are rejected.
- The response body is hashed, parsed as served HTML, and never returned.
- Query values are redacted in report URLs; full URL hashes preserve comparison evidence.
- No cookies, authorization, caller referrer, form submission, login, JavaScript, browser rendering, subresources, or external AI calls. Redirect hops may cross origins only after full URL/DNS/TLS/IP revalidation.

## Scope and caveats

This is deterministic release evidence, not a full SEO, accessibility, penetration, compliance, malware, privacy, or legal audit. Accessibility checks are surface HTML indicators and do not establish WCAG conformance. SEO signals do not promise rankings or traffic. Header snippets are starting points and must be tested against the site before deployment.

The service observes served HTML only. Client-rendered content and runtime behavior are outside v1 scope.

## Integrity

The paid response includes:

- `dossierId`: 24 lowercase hex characters;
- `http.bodySha256`: SHA-256 of the bounded body;
- `reportSha256`: SHA-256 of the report before that field is added;
- `X-Dossier-ID` and `X-Report-SHA256` response headers.

Implementation and fixture drafting were AI-assisted, then manually reviewed and automatically tested. No AI is used at runtime.
