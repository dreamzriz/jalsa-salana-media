# Jamaat Project - Jalsa Salana UK 2026 WiFi Infrastructure

## Project Overview

This repository contains comprehensive deployment plans for a **temporary guest WiFi network** for the **Jalsa Salana UK 2026** event—a large marquee gathering (1200 x 600 feet) expected to host 400–500 concurrent users.

## What We're Building

A reliable, scalable WiFi infrastructure that serves temporary event attendees with:
- **Fast onboarding** via captive portal (QR codes)
- **Fair bandwidth allocation** (per-user throttling to prevent hogging)
- **24-hour session tokens** (simple login, no daily re-auth)
- **Redundant internet connectivity** (dual-WAN failover)
- **Real-time monitoring** (24/7 health dashboard + alerts)

## Project Goals

1. **Pilot Phase (Week of Event):**
   - Deploy minimal setup (3 APs, single 5G WAN) for proof-of-concept
   - Cost budget: **~£4,000**
   - Expected capacity: 150–200 concurrent active users
   - Collect real-world usage metrics for Phase 2 planning

2. **Production Phase (Future Events):**
   - Scale to full marquee coverage (16 APs + 2 spares)
   - Dual-WAN (Starlink primary + 5G backup) for reliability
   - Enterprise-grade gateway with VLAN/QoS/captive portal
   - Repeatable infrastructure for annual events
   - Cost budget: **~£20,000–£24,000** (breaks even at 2+ events/year)

## Scope & Expectations

**In Scope:**
- WiFi coverage for marquee interior
- Guest user onboarding and session management
- Bandwidth management and traffic shaping
- WAN failover redundancy
- Event-day monitoring and support
- Post-event analytics and logging

**Out of Scope:**
- Coverage outside marquee footprint
- Cellular/3G backup (5G/Starlink only)
- Video conferencing optimization
- Guest VPN or advanced security features

## Deployment Plans

### 1. Temporary Pilot Plan (£4k, Rent)

**Best for:** Proof-of-concept, first-time event, minimal capex

- [**Jalsa Salana UK 2026 - Temporary Pilot Plan (£1.2k Budget)**](docs/jalsa-wifi-temporary-pilot-1.2k.md)
  - **3 APs** (rental, priority zones only)
  - **Single 5G WAN** (no Starlink yet)
  - **Open-source gateway** (Linux + OpenNDS)
  - **Setup:** 2–3 hours
  - **Users:** 150–200 concurrent (acceptable experience)
  - **Perfect for:** Learning what works and what doesn't

### 2. Permanent Production Plan (£20k–£24k, Purchase)

**Best for:** Repeatable annual events, professional SLA, full coverage

- [**Jalsa Salana UK 2026 - Permanent Production Deployment**](docs/jalsa-wifi-permanent-production.md)
  - **16 APs + 2 spare** (full marquee coverage)
  - **Dual-WAN** (Starlink primary + 5G backup)
  - **Enterprise gateway** (Fortinet/Cisco/Ubiquiti)
  - **Setup:** 4–5 hours + 8–12 week procurement
  - **Users:** 400–500 concurrent (excellent experience)
  - **Perfect for:** Mission-critical reliability, reusable infrastructure

---

## How to Use This Repository

1. **Start with the Pilot Plan** if you're running the event for the first time or want to minimize budget risk
2. **Choose Production Plan** if you're running 2+ events per year and need enterprise reliability
3. **Each plan includes:**
   - Complete Bill of Materials (BOM) with vendors and budget
   - Network architecture and VLAN design
   - Event-day setup checklist
   - Monitoring and troubleshooting guide
   - Success metrics and post-event analysis plan

---

## Event Attendee Experience (Expected)

**Guest Journey:**
1. Join SSID → Captive portal opens automatically
2. Accept terms → One-page signup (email optional)
3. 24-hour access granted → Enjoy event
4. After 24 hours → Simple re-login if needed (no password required)

**Network Policy:**
- Per-user download limit: 2–3 Mbps (prevents streaming dominance)
- Per-user upload limit: 1–1.5 Mbps (prevents backup/sync)
- Max 1–2 devices per account (prevents device hoarding)
- Session timeout: 24 hours (re-login required after)

**Expected Quality:**
- Good experience (minimal buffering): 150–200 users on Pilot; 400–500 on Production
- Acceptable/basic (occasional lag): Up to 220 on Pilot; up to 500 on Production
- Congestion: Beyond these thresholds, expect slowdowns

---

## Key Decisions Made

| Decision | Pilot | Production |
|---|---|---|
| **Infrastructure cost** | Rent (temporary) | Buy (permanent) |
| **WAN strategy** | Single 5G | Dual-WAN (Starlink + 5G) |
| **Coverage** | 3 APs (priority zones) | 16 APs (full marquee) |
| **Gateway** | Open-source Linux | Enterprise appliance |
| **Capacity** | 150–200 concurrent | 400–500 concurrent |
| **Monitoring** | Manual checks | 24/7 automated alerts |
| **Break-even** | Single event | 2+ events/year |

---

## Quick Reference: Budget & Timeline

| Phase | Budget | Setup Time | Procurement | Users |
|---|---:|---|---|---:|
| **Pilot** | ~£1,220 | 2–3 hours | 1 week | 150–200 |
| **Production** | ~£20,000–£24,000 | 4–5 hours | 8–12 weeks | 400–500 |

---

## Next Steps

1. **Review the Pilot Plan** to understand requirements
2. **Confirm event date and expected attendance**
3. **Identify a tech lead** (Linux skills optional but helpful)
4. **Decide: Pilot first or go straight to Production?**
5. **Order hardware** (8–12 week lead time for bulk orders)
6. **Schedule setup day** (coordinate with venue, power, internet provider)

---

## Support & Questions

For detailed technical specifications, network architecture diagrams, event-day checklists, and troubleshooting guides, see the individual deployment plans linked above.

- **Pilot Plan:** Best for beginners; simple, low-risk, educational
- **Production Plan:** Best for reliability; enterprise features, full coverage, long-term investment
