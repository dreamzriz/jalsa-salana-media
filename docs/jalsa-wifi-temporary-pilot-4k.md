# Jalsa Salana UK 2026 - Temporary Pilot Plan (£4k Budget)

## Purpose

Ultra-low-cost **temporary pilot** for one marquee area using 3 APs and single-WAN setup. This is NOT a permanent solution—designed for proof-of-concept and stress testing before full deployment.

## Key Philosophy

- **Rent, don't buy** (maximize capex savings)
- **Single WAN only** (Starlink optional/later addition)
- **Minimal APs** (3 focused zones, not full coverage)
- **Open-source software** (zero licensing cost)
- **Fast teardown** (event-only deployment)

## Scope: Priority Zones Only

Deploy WiFi for THREE highest-priority areas only:

1. **Check-in / Registration** (highest concentration)
2. **Main seating / stage area** (core event space)
3. **Food / water service queue** (secondary zone)

Accept NO coverage for:
- Marquee perimeter/edges
- Exterior circulation
- Storage/admin areas (use hardwired if needed)

## Phase 1 BOM (Temporary Pilot)

| Item | Qty | Notes | Budget |
|---|---:|---|---:|
| **Managed PoE+ switch (16 port, rental)** | 1 | Rental 3 days: ~£80–150 | £100 |
| **Wi-Fi 6 APs (rental, 3x)** | 3 | Rental 3 days: ~£60–100 each | £250 |
| **Wi-Fi 6 AP spare (rental)** | 1 | Quick swap during event | £80 |
| **Mini PC gateway (rental or borrow)** | 1 | Run Linux + captive portal | £50 |
| **5G router + business SIM** | 1 | Primary WAN only; SIM: ~£40/month for 1 month | £150 |
| **UPS (small, rental)** | 1 | Basic backup power | £40 |
| **Outdoor Cat6A patch + mounts** | 1 lot | ~50m Cat6A, clips, connectors | £120 |
| **Consumables / labels / misc** | 1 lot | Install tape, velcro, markers | £30 |
| **Setup + event-day engineer (1 person, 2 days)** | 1 | Voluntary or minimal fee | £200 |
| **Contingency (5%)** | — | Misc overages | £200 |
| **TOTAL PILOT BUDGET** | — | — | **£1,220** |

**Remaining budget headroom: ~£2,780** ← Reserve for:
- Extra 5G data overage (~£500)
- Emergency equipment replacement (AP failure, etc.)
- Extension of rental period if pilot extended
- Lessons-learned upgrades before full Phase 2

---

## Temporary Gateway Stack (Linux)

**Assumption:** A volunteer has basic Linux networking skills, OR you rent a managed gateway box for ~£80.

### Option A: DIY Linux (Cheapest)

**Hardware:**
- Refurbished mini PC (borrow or rent): ~£30–50 for event
- USB 2.5GbE NIC (if needed): already owned or ~£15 borrow

**Software (all free, open-source):**
```
OS: Ubuntu Server 24.04 LTS (live USB)
Firewall/NAT: nftables
DHCP/DNS: dnsmasq
Captive portal: OpenNDS
Traffic shaping: tc (CAKE queue discipline)
```

**Captive Portal Flow:**
1. User joins SSID → OpenNDS intercepts HTTP request
2. One-page signup (email optional, simple tick-box terms)
3. 24-hour session token issued
4. Re-login required after 24 hours
5. No streaming throttling (just per-user 2 Mbps cap via Linux tc)

### Option B: Managed Gateway Box (Simplest)

- Rent a commercial captive portal box (e.g., Ubiquiti Dream Machine or similar): ~£80 for 3 days
- Still manage WAN failover manually or via simple script
- Less volunteer labor needed

---

## WAN Configuration (Single Path)

### Primary: 5G Business Router

**Hardware:**
- Standard 5G router (Netgear/TP-Link business model): ~£80–120 (can borrow/rent)
- Business data SIM: ~£40 for 1-month contract (used only for event)
- Typical throughput: 100–300 Mbps down, 20–50 Mbps up (adequate for 100–150 concurrent users)

**Why single WAN for pilot?**
- Starlink requires terminal + monthly plan (~£100–150/month); too expensive for temp test
- 5G router is cheaper, instant setup, no hardware lock-in
- Failover (if 5G drops) = manual reset or best-effort restart

### Starlink Optional Add (If Budget Allows)

- If 5G proves unreliable during pilot: add Starlink terminal + 1-month service (~£150 total)
- Use as WAN2 in Phase 2 after pilot validates business case

---

## Network Design

### Single VLAN (Simplified)

For pilot, use just **one VLAN** to reduce complexity:
- VLAN 10: Guest (all users + gateway management traffic on same VLAN, firewall-separated)

DHCP: 300 IPs (smaller pool for 3 APs, lower contention)

### AP Placement (3 Only)

```
Check-in Area:    1 AP  (high density, short duration)
Main Seating:     1 AP  (largest user pool, central)
Food Queue:       1 AP  (secondary, transient)
Spare:            1 AP  (in equipment bag for quick swap)
```

Each AP target: ~50–80 concurrent users per AP = ~150–200 total active users during pilot.

---

## Traffic Policy (Very Simple)

- **Per-user limit:** 2 Mbps down, 1 Mbps up (via Linux `tc`)
- **Session timeout:** 24 hours (simplifies re-login, no daily cap)
- **Max devices per user:** 1–2 (strictly enforced in portal)
- **Blocked services:** None (keep it open for pilot; only throttle by user, not by service)
- **Peak-hour override:** None (accept degradation; data is valuable for Phase 2 planning)

---

## Event-Day Checklist (Pilot)

### Pre-Event (1 Week)

- [ ] Confirm 5G router SIM activation + test speed
- [ ] Borrow/rent mini PC, UPS, switch, APs; receive and inventory
- [ ] Create bootable Ubuntu USB with Linux gateway config (copy from repo or simple Ansible playbook)
- [ ] Test captive portal on laptop (offline simulation)
- [ ] Print QR codes for manual portal redirect; prepare simple A4 signage

### Setup Day (Morning of Event, 2–3 Hours)

- [ ] Power up UPS, mini PC, switch, 5G router in sequence
- [ ] Boot Linux gateway; verify nftables, dnsmasq, OpenNDS running
- [ ] Connect 5G router WAN to gateway WAN port; test internet connectivity
- [ ] Mount APs in three zones; connect to PoE switch via outdoor Cat6A
- [ ] Check AP hostnames, IP assignments from gateway DHCP log
- [ ] Test SSID broadcast and captive portal on phone (at least 2 devices)
- [ ] Verify per-user throttle rules active in `tc` qdisc
- [ ] Post QR code signage and welcome banner at check-in

### During Event

- [ ] Monitor gateway uptime + DHCP lease churn via `dnsmasq` logs
- [ ] Watch for AP LED status (green = healthy); restart if red/orange
- [ ] Check 5G router signal bars; reposition antenna if weak
- [ ] Note user-reported WiFi issues in log (time, location, device type)
- [ ] Reboot gateway + 5G router once per day if needed (early morning)

### Post-Event Teardown (Evening, 1–2 Hours)

- [ ] Shut down gateway cleanly (sync logs to USB)
- [ ] Disconnect all APs, switch, 5G router; wipe power
- [ ] Collect all hardware into rental boxes; photograph serial numbers for return
- [ ] Export DHCP logs, traffic counters for pilot analysis
- [ ] Deactivate 5G SIM (stop billing immediately)

---

## Success Metrics for Pilot

Track these during 3-day event:

1. **Concurrent users at peak:** Target 150–180 active; anything above = upgrade path validated
2. **Onboarding success rate:** Aim for >90% get-online on first attempt
3. **Median user throughput:** Target 1.5–2 Mbps (shows throttle working)
4. **AP stability:** Fewer than 2 reboots per AP over 3 days
5. **5G WAN uptime:** >98% (if drops, document duration + cause)
6. **User complaint count:** Track "no WiFi," "slow WiFi," "login loop" separately
7. **Login re-attempt rate:** Should drop after first 4 hours (indicates sticky sessions)

---

## Post-Pilot Decision Tree

### If Metrics OK (Proceed to Phase 2 Full Deployment)

- Add 2–4 more APs to increase capacity to 400–500 concurrent users
- Add Starlink as primary WAN (more reliable than 5G alone)
- Upgrade to commercial gateway (Cisco ASA / Fortigate / Ubiquiti UDM-Pro)
- Keep Linux backup gateway for contingency

### If Metrics Marginal (Extend Pilot or Modify)

- Rent APs for 2 more weeks; do second small event with 5–6 APs
- Add backup 5G router; test WAN redundancy
- Tune traffic shaping; increase per-user limit to 3 Mbps if 5G stable
- Revisit Phase 2 budget with actual data

### If Critical Failures (Abort or Major Redesign)

- 5G coverage insufficient in venue: plan Starlink + 5G dual-WAN from start
- User density exceeds 300 concurrent: budget for 8–10 APs in Phase 2
- Linux gateway too complex: move to managed appliance in Phase 2

---

## Budget Summary

| Category | Cost |
|---|---:|
| Hardware rental (3 days) | £550 |
| WAN (5G SIM + data) | £150 |
| Consumables + contingency | £220 |
| Setup labor + expertise (optional) | £200 |
| **PILOT TOTAL** | **£1,120** |
| **Remaining from £4k budget** | **£2,880** |

---

## Notes

- **This is temporary:** All hardware is returnable. No long-term capex locked in.
- **Starlink optional:** Single 5G WAN is acceptable for pilot proof-of-concept. Only add Starlink if 5G unreliable.
- **Volunteer-friendly:** If Linux too complex, rent a managed gateway instead (~£80 adds to total, still under budget).
- **Data is valuable:** During pilot, log everything (DHCP, packet loss, user feedback). Use to justify Phase 2 investment.
- **Fast to scale:** If pilot validates demand, Phase 2 can deploy 12–16 APs within 2–3 weeks.

---

## Next Steps

1. **Confirm venue WiFi eligibility:** Is spectrum available (2.4 GHz + 5 GHz unblocked)?
2. **Scout antenna placement:** 3-zone map with outlet/power locations?
3. **Identify Linux volunteer** or confirm managed gateway rental availability
4. **Book rental hardware** (2–3 weeks lead time typical)
5. **Schedule setup day training** for event-day team
