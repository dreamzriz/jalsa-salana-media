# Jalsa Salana UK 2026 - Guest Wi-Fi Solution Plan

## Objective

Provide free guest Wi-Fi for a large 3-day event in a remote area using Starlink and event-grade outdoor Wi-Fi.

## Confirmed Design Inputs

- Event duration: 3 days
- Estimated visitors overall: ~40,000
- Target active users: 2,000 to 3,000 (concurrent peak scenario)
- Captive portal policy: guest re-login every 24 hours
- Onboarding requirement: QR code flow that includes Facebook page visit prompt

## Key Technical Reality

2 to 5 routers/APs are not enough for 2,000 to 3,000 concurrent users.

Reason:

- VLANs and Linux DHCP/IP pools solve addressing and segmentation only.
- They do not solve radio airtime contention, RF interference, and client density limits.

## Recommended Architecture

### 1) Internet / Backhaul

- Preferred: temporary fiber or microwave as primary backhaul.
- Starlink: use as secondary or part of a multi-WAN pool.
- If Starlink-heavy deployment is required, plan multiple terminals and load balancing.

### 2) Network Core

- Multi-WAN gateway/firewall
- 10G-capable core/distribution switching
- UPS + generator-backed power
- Monitoring and logging during all event hours

### 3) Wi-Fi Access Layer

- Outdoor Wi-Fi 6/6E APs (omni + sector mix)
- Capacity-cell design across zones (entrance, marquee, food areas, walkways)
- Typical event-scale range for this requirement: 40 to 100 APs (final count depends on site survey)

### 4) Guest Access and Captive Portal

- QR codes placed in key locations
- Captive portal with:
  - Terms and acceptable use policy
  - Privacy notice (GDPR-aligned)
  - Optional Facebook visit/follow prompt
- Session expiration after 24 hours; re-auth required daily

## Capacity and Performance Controls

- Per-device rate limiting (example: 1 to 3 Mbps down, 0.5 to 1 Mbps up)
- Client isolation for guest security
- Traffic shaping and optional blocking of heavy streaming during peak periods

## Cost Guidance (UK, Rough)

### Managed rental model (3-day event, recommended)

- Approximate range: GBP 60,000 to GBP 220,000
- Varies by AP count, staffing, backhaul type, and power/cabling complexity

### Buy model

- Approximate range: GBP 120,000 to GBP 450,000+ (hardware/integration)
- Plus operational staffing and temporary infrastructure costs

## Why 2 to 5 APs Will Fail at This Scale

- Insufficient airtime for concurrent clients
- Higher retransmissions from interference and weak edge clients
- Poor user experience in dense crowd conditions
- Throughput collapse during peak load despite valid IP assignment

## Minimum Viable Strategy if Budget Is Limited

- Do not attempt full-site blanket Wi-Fi
- Provide prioritized zone-based coverage only
- Set strict concurrency and per-user speed controls
- Expand in phases after test/pilot validation

## Implementation Steps

1. Conduct RF and site survey
2. Define zone-level concurrency targets
3. Produce final AP and channel plan
4. Deploy and test captive portal + 24-hour session policy
5. Run event with on-site NOC monitoring
6. Capture post-event performance metrics for next-year optimization

## Compliance and Policy Notes

- Avoid forcing Like/Share as a hard access gate.
- Prefer a compliant "Visit/Follow" prompt before/after successful authentication.
- Ensure privacy, consent, and logging policies are documented and visible.
