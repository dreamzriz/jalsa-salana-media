# Jalsa Salana UK 2026 - Guest Wi-Fi Plan (Up to 500 Active Users)

## Objective

Provide stable free guest Wi-Fi for up to 500 concurrent users in a remote event area using Starlink-based internet and managed outdoor Wi-Fi.

## Design Inputs

- Event duration: 3 days
- Remote site conditions
- Target concurrent users: up to 500
- Coverage area: single marquee/tent only
- Approximate footprint: 1200 x 600 feet
- Guest re-authentication interval: every 24 hours
- Onboarding: QR code with Facebook page visit prompt in captive portal journey

## Recommended Solution

### 1) Internet / Backhaul

- Primary: 1 Starlink Business/Priority line
- Secondary backup: 1 additional Starlink or 5G business router/data plan
- Use a dual-WAN gateway for failover and load sharing

### 2) Core Network

- 1 gateway/firewall with captive portal support
- 1 managed PoE switch (24 or 48 ports, based on final AP count)
- Optional small aggregation switch for stage/operations side if needed
- UPS for network core and Starlink equipment

### 3) Wi-Fi Access Layer

- For one marquee with up to 500 active users, start estimate: 10 to 18 Wi-Fi 6 APs
- Inside-tent cell design (not one long-range AP), with APs distributed in rows for even client load
- Use directional placement toward seating/dense user blocks where possible
- Prefer wired/fiber uplinks to APs; use wireless bridges only if cabling is not practical

## VLAN and Linux DHCP Guidance

VLANs + Linux DHCP are useful and recommended for:

- Address allocation at scale
- Guest/admin segmentation
- Cleaner policy control

But they do not replace the need for enough AP density and channel planning.

## Captive Portal and Access Policy

- QR scan opens captive portal
- Page includes:
  - Terms of use
  - Privacy notice
  - Optional Facebook visit/follow step
- Grant internet access after acceptance
- Session timeout set to 24 hours, requiring re-login daily

## Suggested Network Policies

- Per-user speed cap: 2 to 4 Mbps down, 1 Mbps up
- Device/session limits per user as required
- Client isolation for guest network
- Optional content and streaming controls during peak hours

## Hardware Estimate (Typical)

- 1 x dual-WAN gateway/firewall
- 1 to 2 x managed PoE switches
- 10 to 18 x Wi-Fi 6 APs (indoor or outdoor-rated based on tent environment)
- 1 x Starlink kit (primary)
- 1 x backup internet path (Starlink or 5G)
- 1 to 2 x UPS units
- Outdoor cabling, mounts, enclosures, labeling

## Rough Cost Estimate (UK)

### Managed rental model (3-day event)

- Approx GBP 10,000 to GBP 32,000
- Includes variable setup, tuning, and support staffing

### Buy model

- Approx GBP 16,000 to GBP 65,000
- Plus deployment labor, temporary infrastructure, and operations support

## Deployment Steps

1. Site survey and single-marquee user-density mapping
2. AP placement and channel plan
3. Captive portal configuration and 24-hour session policy
4. Pre-event load test
5. Live monitoring during event
6. Post-event performance report

## Success Criteria

- Stable onboarding with QR + captive portal
- Acceptable browsing/messaging performance at peak
- Daily re-auth functioning every 24 hours
- Low complaint rate and minimal dead zones

## Notes

- For strong reliability, do not undersize AP count.
- Plan spare APs, spare PoE ports, and spare cables for rapid fault replacement.
