# Jalsa Salana UK 2026 - Cost-Optimized Starter Plan (5 AP Now)

## Purpose

This is a temporary low-cost deployment for one marquee (1200 x 600 feet) using only 5 APs to start.

## Important Constraint

A 5-AP design is not suitable for full 500 concurrent-user coverage across the full marquee footprint.

Expected practical outcome with good tuning:

- Good experience: 120 to 180 active users
- Acceptable/basic experience: up to 220 active users
- Beyond this, expect congestion and slower speeds

## Scope for Phase 1 (Now)

- Cover only high-priority zones, not full marquee blanket coverage
- Prioritize:
  - Entrance/check-in area
  - Central seating block
  - Stage-adjacent circulation lane
  - Food/water queue area
  - Operations/help desk pocket

## Phase 1 Bill of Materials (5 AP)

| Item | Qty | Notes |
|---|---:|---|
| Gateway/firewall with captive portal and VLAN support | 1 | Dual-WAN preferred |
| Managed PoE+ switch (16 or 24 port) | 1 | Keep headroom for future APs |
| Wi-Fi 6 APs (deployed) | 5 | Same model for easy support |
| Wi-Fi 6 AP (spare) | 1 | Fast replacement during event |
| Starlink terminal (primary) | 1 | Priority/Business recommended |
| Backup WAN (5G business router) | 1 | Lower-cost failover option |
| UPS | 1 | Protects gateway, switch, WAN devices |
| Outdoor-rated Cat6A runs | 8 to 10 | Includes spare and reroute path |
| Mounts/brackets and consumables | 1 lot | Labeling and install kit |

## Network Policy for Cost Control

- Captive portal with QR onboarding
- Session timeout: 24 hours (re-login required)
- Per-user cap: 2 Mbps down, 1 Mbps up
- Limit max devices per user (recommended: 1 to 2)
- Apply streaming controls during peak windows

## VLAN and DHCP (Linux-Friendly)

- VLAN 10: Guest Wi-Fi
- VLAN 20: Management
- VLAN 30: Staff/Operations

DHCP recommendation:

- Guest pool: at least 600 IPs
- Lease time: 4 hours during event peak

## AP Placement Guidance (5 AP)

- Keep APs focused on user clusters, not perimeter corners
- Avoid max transmit power everywhere; tune for cleaner reuse
- Keep similar client counts per AP where possible
- Place one AP near each priority zone listed above

## Budget Estimate (UK, 3-Day Event)

### Managed rental

- Approx GBP 4,500 to GBP 11,000

### Purchase model

- Approx GBP 8,000 to GBP 22,000

## Risks You Accept with 5 AP

- Coverage gaps at marquee edges
- Throughput drop during peak congregation times
- Higher retry/interference risk if users cluster heavily
- Not suitable for guaranteed 500 concurrent users

## Upgrade Trigger Plan (Fast Scale-Up)

Add APs when any of these happen for more than 15 minutes:

- Average AP client count exceeds 40
- Onboarding success drops below 95%
- Median guest throughput drops below 1 Mbps
- User complaints spike in specific areas

Recommended expansion path:

- Step 1: add 3 APs (total 8)
- Step 2: add 4 APs (total 12)
- Step 3: move to full design (16 AP baseline)

## Event-Day Checklist

1. Test captive portal and QR journey on Android and iOS
2. Verify 24-hour re-login behavior
3. Confirm WAN failover to backup path
4. Check AP client load every 30 minutes
5. Keep one preconfigured spare AP ready
