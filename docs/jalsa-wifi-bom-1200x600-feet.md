# Jalsa Salana UK 2026 - Wi-Fi BOM (Single Marquee 1200 x 600 feet, Up to 500 Active Users)

## Scope

This BOM is sized for one marquee area only.

- Footprint: 1200 x 600 feet
- Area: 720,000 sq ft (approx 66,890 sq m)
- Peak active users: up to 500 concurrent users
- Access model: free guest Wi-Fi with QR captive portal and 24-hour re-login

## Design Assumptions

- User behavior is light to moderate (messaging, browsing, maps, social apps)
- Heavy streaming is shaped/limited during peak time
- AP mounting positions are available along tent frame/poles
- Cabling paths are available for most APs

## Recommended Capacity Targets

- Internet target: 400 Mbps to 1 Gbps usable aggregate
- Per-user cap: 2 to 4 Mbps down, 1 Mbps up
- Session timeout: 24 hours (re-auth required)

## Bill of Materials (Recommended Baseline)

| Item | Qty | Notes |
|---|---:|---|
| Dual-WAN gateway/firewall with captive portal | 1 | Must support rate limits, VLANs, user sessions |
| Managed PoE+ switches (24-port) | 2 | 48 PoE ports total for APs and growth |
| Wi-Fi 6 APs (deployed) | 16 | Primary design count for coverage + capacity |
| Wi-Fi 6 APs (hot spare) | 4 | Rapid swap during event |
| Starlink terminal (primary) | 1 | Priority/Business recommended |
| Backup internet link (5G business or 2nd Starlink) | 1 | Failover + load sharing |
| UPS (core network) | 2 | One for gateway/core, one for Starlink + distribution |
| Outdoor-rated Cat6A runs | 20 to 28 | Includes spares and alternate paths |
| Mounting kits/brackets | 20 | For AP and radio mounting |
| Weatherproof enclosures | 4 to 8 | For edge switch/power points if required |
| Labeling, patch leads, consumables kit | 1 lot | Event install essentials |

## AP Layout Guidance (Single Marquee)

- Use a grid/row layout, not edge-only placement
- Keep AP spacing generally in the 60 to 120 feet range, adjusted after survey
- Alternate channels and lower transmit power to reduce co-channel interference
- Bias coverage toward denser seating and circulation lanes

## VLAN and IP Plan (Example)

- VLAN 10: Guest Wi-Fi clients
- VLAN 20: Network management
- VLAN 30: Event operations/staff

DHCP sizing recommendation:

- Guest pool: at least 1500 IPs (for device churn and lease overlap)
- Lease time: 4 to 8 hours (operationally efficient for event turnover)

## Rough Budget (UK)

### Managed rental for 3-day event (recommended)

- Estimated range: GBP 14,000 to GBP 38,000
- Includes deployment, tuning, and event support variability

### Purchase model

- Estimated range: GBP 24,000 to GBP 70,000
- Excludes full staffing and temporary civil/power extras

## Optional Add-Ons

- Second gateway for HA
- Additional 4 to 8 APs for stronger edge coverage
- NOC monitoring screen + on-site network engineer
- Portable mast kits for cleaner RF geometry

## Acceptance Criteria

- Captive portal works with QR flow and 24-hour re-login
- Stable access at 500 concurrent users under test load
- No critical dead zones in primary seating/circulation area
- Failover to backup WAN completes without major user impact

## Pre-Event Test Checklist

1. Validate WAN failover and recovery
2. Run onboarding test across Android and iOS
3. Simulate at least 300 to 500 concurrent sessions
4. Verify rate limits and client isolation
5. Confirm AP spare replacement workflow
