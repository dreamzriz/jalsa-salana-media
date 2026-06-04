# Jalsa Salana UK 2026 - Permanent Production Deployment

## Purpose

Production-grade WiFi infrastructure for full marquee coverage (1200 x 600 feet) supporting 400–500 concurrent users with enterprise reliability, redundancy, and monitoring.

**Based on:** Pilot learnings + permanent capex investment

## Deployment Philosophy

- **Buy, not rent** (capex cost for reusability across future events)
- **Dual-WAN redundancy** (Starlink primary + 5G backup)
- **Enterprise gateway** (commercial firewall + managed switching)
- **Full marquee coverage** (12–16 APs + strategic placement)
- **Professional monitoring** (24/7 visibility + alerting)
- **Easy event repetition** (containerized config, rapid redeploy)

---

## Scope: Full Coverage + Priority Zones

**Primary coverage zones** (guaranteed excellent experience):
- Check-in / Registration
- Main seating / stage area
- Food / water service
- Operations / help desk
- Entrance circulation

**Secondary coverage zones** (good-to-acceptable):
- Marquee perimeter
- Exterior pathways
- Overflow seating areas
- Storage / equipment zones (hardwired backup if needed)

**Not covered** (by design):
- Areas outside marquee footprint
- Underground/shielded storage

---

## Phase 2 BOM (Permanent Production)

| Item | Qty | Notes | Budget |
|---|---:|---|---:|
| **Enterprise gateway (Cisco ASA / Fortigate 80F / Ubiquiti UDM-Pro)** | 1 | Dual-WAN, captive portal, VLAN, HA-ready | £2,500–3,500 |
| **Managed PoE+ switch (48 port, redundant PSU)** | 1 | Core infrastructure; future-proof | £1,200–1,800 |
| **Wi-Fi 6 APs (production tier)** | 16 | Ubiquiti U6-Pro / Cisco Meraki / Arista (matching set) | £6,400–8,000 |
| **Wi-Fi 6 AP spare (2x)** | 2 | Rapid replacement if failure | £400–500 |
| **Starlink kit (Business)** | 1 | Primary WAN (fixed IP, priority traffic) | £600–800 |
| **5G router + business SIM (12 mo)** | 1 | Backup WAN; SIM ~£40/mo | £500–800 |
| **Redundant UPS (10 kVA)** | 1 | Covers gateway, switch, WAN devices + spare capacity | £1,500–2,000 |
| **Outdoor-rated Cat6A infrastructure** | 1 lot | 150–200m runs, conduit, patch leads, mounts | £800–1,200 |
| **Access points racks + cable management** | 1 lot | 19" rack, PDU, patch panels, labeling system | £400–600 |
| **Network monitoring + management** | 1 | Centralized dashboard (Ubiquiti/Cisco/Arista native or Netdata) | £0–500 |
| **Professional installation + testing (2–3 days)** | 1 | Cabling, PoE verification, performance tuning | £1,500–2,500 |
| **Contingency / spares (10%)** | — | SFP transceivers, patch cables, power supplies | £800–1,500 |
| **TOTAL PRODUCTION SETUP** | — | — | **£17,500–24,000** |

---

## Recommended Gateway Options

### Option A: Cisco ASA 5506-X (Enterprise Standard)

- **Cost:** ~£2,500–3,000
- **Pros:** Industrial reliability, extensive VLAN/firewall features, support ecosystem
- **Cons:** Requires Cisco expertise; older UI
- **Capacity:** 500 Mbps throughput, 25,000+ concurrent sessions

### Option B: Fortinet FortiGate 80F (Modern SMB)

- **Cost:** ~£2,800–3,200
- **Pros:** Modern WebUI, good threat prevention, dual-WAN native
- **Cons:** Requires FortiCloud subscription (~£500/year)
- **Capacity:** 1 Gbps throughput, 30,000+ sessions

### Option C: Ubiquiti UniFi Dream Machine Pro (Simplest)

- **Cost:** ~£2,200–2,800
- **Pros:** Native management dashboard, easy captive portal, integrated DHCP/DNS
- **Cons:** Less enterprise-grade; smaller session capacity (~15,000)
- **Capacity:** 300 Mbps throughput (adequate for event)

**Recommendation:** **FortiGate 80F** (best balance of features, price, support)

---

## AP Strategy: 16-AP Full Coverage

### Zone Distribution

| Zone | APs | Rationale |
|---|---:|---|
| Check-in / registration | 3 | High density, quick onboarding |
| Main seating block A | 4 | Core event; largest user pool |
| Main seating block B | 3 | Secondary seating cluster |
| Food / water service | 2 | Transient, high churn |
| Operations / help desk | 1 | Staff + support tickets |
| Exterior circulation | 2 | Entrance/exit pathways |
| Spare (in rack) | 2 | Fast replacement, load balancing |
| **TOTAL** | **16** | 1 spare per 8 APs deployed |

### AP Specifications

- **Model:** Ubiquiti U6-Pro OR Cisco Meraki MR46E (consistency matters)
- **Deployment:** All same generation + firmware for easy troubleshooting
- **Power:** PoE+ 802.3at (55W max)
- **Backhaul:** Ethernet wired (no mesh; mesh adds latency + interference)
- **Band steering:** Enabled (2.4 GHz for range, 5 GHz for throughput)
- **Air time fairness:** Enabled (prevents slow clients from dragging down fast ones)

---

## Network Architecture (Permanent)

### Topology

```
                   ┌─────────────────┐
                   │  Starlink WAN   │ (Primary, fixed IP)
                   └────────┬────────┘
                            │
            ┌───────────────┴────────────────┐
            │                                │
    ┌───────▼──────────────────────────────┐ │
    │  Enterprise Gateway / Firewall       │ │
    │  (Dual-WAN, VLAN, Captive Portal)   │ │
    └───────┬───────────────┬──────────────┘ │
            │               │                │
            │        ┌──────▼────────┐       │
            │        │  5G Router    │       │
            │        │  (Backup WAN) │◄──────┘
            │        └───────────────┘
            │
    ┌───────▼────────────────────┐
    │ Managed PoE+ Switch (48)   │
    │ (Redundant PSU, L3 capable)│
    └─────┬──────────────────────┘
          │
    ┌─────┴─────────────────────────────────────────────────┐
    │                                                        │
 ┌──▼────┐ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐     ┌──▼────┐
 │ AP 1  │ │ AP 2  │ │ AP 3  │ │ AP 4  │ │ ...   │     │ AP 16 │
 │ (PoE) │ │ (PoE) │ │ (PoE) │ │ (PoE) │ │(PoE)  │     │ (PoE) │
 └───────┘ └───────┘ └───────┘ └───────┘ └───────┘     └───────┘
    │         │         │         │         │              │
 (Cat6A to marquee zones - outdoor rated runs)
```

### VLAN Design

| VLAN | Name | Purpose | Subnet | IPs | Notes |
|---|---|---|---|---:|---|
| 10 | Guest | Event attendees | 10.10.0.0/24 | 200 | Dynamic DHCP |
| 20 | Management | Network admin + monitoring | 10.20.0.0/25 | 100 | Static, restricted |
| 30 | Staff / Ops | Internal operations | 10.30.0.0/25 | 100 | Static, local services |
| 40 | IoT (future) | Cameras, sensors, etc. | 10.40.0.0/25 | 100 | Reserved, segregated |

### DHCP Configuration

- **Guest DHCP:** 250 leases (headroom for 400+ concurrent with short timeouts)
- **Lease time:** 6 hours (event-phase); 24 hours (non-peak)
- **DNS:** Gateway primary, public DNS (Cloudflare 1.1.1.1) secondary
- **Default gateway:** Gateway LAN IP
- **Option 6 (DNS):** Gateway IP only (prevent DNS rebind attacks)

---

## Captive Portal & Authentication

### Portal Features

- **Single-page signup** (no external redirect)
- **QR code login** (mobile-friendly)
- **Optional fields:** Email (for follow-up), accept terms + privacy
- **Session duration:** 24 hours (single login, no daily re-auth)
- **Rate limiting:** 5 login attempts per IP per 10 minutes

### Portal Software (Permanent)

- **If Ubiquiti:** Native UniFi portal (easy)
- **If Cisco/Fortinet:** Integrated captive portal (Cisco: DAC; Fortigate: FortiGate portal)
- **If DIY:** OpenNDS + custom HTML (opensource option)

### Branding

- Logo: Jamaat + event name
- Welcome message: Event dates, WiFi usage policy, emergency contact
- Social integration: Optional prompt to follow social media (not required for access)

---

## Traffic Policy & Shaping (Permanent)

### Per-User Limits

| Policy | Value | Reason |
|---|---|---|
| Download cap | 3 Mbps | Streaming throttle during peak |
| Upload cap | 1.5 Mbps | Backup/sync prevention |
| Max devices per user | 2 | Prevent account hoarding |
| Session timeout | 24 hours | Simplifies re-auth, tracks daily users |
| Idle timeout | 8 hours | Reclaims dormant sessions |

### Burst Allowance

- **Initial burst:** 10 Mbps for 5 seconds (good UX for page loads)
- **Sustained:** Drops to per-user limit after burst exhausted
- **Retry:** Full burst available after 30 seconds idle

### Priority Traffic (QoS)

| Traffic | Priority | Rationale |
|---|---|---|
| DNS | Highest | Must be instant |
| DHCP | High | Onboarding speed |
| HTTP/HTTPS | Medium | Web browsing |
| Video/streams | Low | Throttle aggressively |
| P2P (torrent/VPN) | Blocked | Bandwidth hog prevention |

---

## Monitoring & Alerting (24/7)

### Dashboard (Real-Time)

- **Active users:** By zone, by AP, by device type
- **Throughput:** Current download/upload, peak usage
- **AP health:** Signal strength, client count, packet loss, uptime
- **WAN status:** Both links live + latency to public DNS
- **Top talkers:** Which users/IPs consuming most bandwidth
- **DHCP pool:** Utilization %, lease activity

### Alerts (Event-Day + After)

Set thresholds for automated notification (email/SMS):

| Condition | Threshold | Action |
|---|---|---|
| AP offline | Any | Immediate alert + SMS to tech team |
| WAN latency | >100 ms | Warning (investigate, may failover) |
| DHCP exhaustion | >90% pool | Warning (add more IPs or reboot gateway) |
| User complaints | >5 per hour in chat | Escalate to team lead |
| Packet loss | >5% sustained | Investigate interference + AP placement |
| CPU utilization (gateway) | >85% | Monitor; consider capacity upgrade |

### Logs to Retain (Post-Event Analysis)

- DHCP leases (all onboarding + duration per user)
- Captive portal logins (timestamp, success/fail)
- Per-user traffic totals (for billing/stats)
- AP client association logs (roaming patterns, interference)
- WAN link status changes (uptime %)
- Gateway firewall drops (security insights)

**Log retention:** Keep for 90 days (cloud storage backup)

---

## Physical Installation

### Outdoor-Rated Cabling

- **Cat6A+ with PVC jacket:** Withstands UV, temperature swings, foot traffic
- **Conduit runs:** Protected under marquee fabric where possible
- **Connectors:** IP67-rated RJ45 (weatherproof)
- **PoE injector:** At switch (don't run PoE over long outdoor runs; inject at entry point)

### AP Mounting

- **Ceiling mounts:** Inside marquee tent poles (secure + high visibility)
- **Antenna orientation:** Omni downward (covers marquee floor optimally)
- **Height:** 8–12 feet above ground (balance coverage vs interference)
- **Spacing:** 40–60 feet between APs (minimize co-channel interference)

### Cable Organization

- **19" rack for gateway + switch + UPS** (typically near primary entrance/power source)
- **Cable trays** from rack to marquee poles (support long Cat6A runs)
- **Color coding:** Blue=guest, green=mgmt, red=ops (easy troubleshooting)
- **Labels at every endpoint:** AP name, VLAN, PoE status

---

## Redundancy & Failover

### WAN Redundancy

| Scenario | Primary | Secondary | Recovery |
|---|---|---|---|
| Starlink working, 5G working | Starlink | 5G (standby) | Auto failover if Starlink latency >1s |
| Starlink down | 5G takes all traffic | N/A | Manual switch or pre-configured trigger |
| 5G down | Starlink | None | Auto continues on Starlink |
| Both down | Degraded WiFi (local only) | None | Emergency hotspot or manual WAN reset |

### Gateway Redundancy (Optional, Phase 2+)

For mission-critical events:
- Deploy **two gateways in active-active** (same network config, stateless traffic distribution)
- Use VRRP (Virtual Router Redundancy Protocol) for automatic failover
- Shared PoE switch provides single point of failure (mitigate with second switch, 10G interconnect)

**Cost add:** ~£3,500–5,000 (consider only if SLA >99.5%)

### AP Redundancy

- **2 spare APs in rack** (pre-configured, quick swap)
- **If AP fails:** Manually swap in spare, reconfigure in management dashboard (~5 minutes)
- **Failed AP logs:** Review for interference/rogue client patterns

---

## Event-Day Checklist (Permanent Setup)

### Pre-Event (2 Weeks Prior)

- [ ] Verify all AP firmware up-to-date (download to laptop, not OTA during event)
- [ ] Test captive portal on multiple devices (iOS, Android, older Android)
- [ ] Rehearse gateway failover (simulate Starlink failure, test 5G takeover)
- [ ] Generate event-specific QR codes + signage (print, laminate)
- [ ] Brief tech team on alert thresholds + escalation procedure
- [ ] Backup gateway config to secure cloud storage (encrypted)
- [ ] Test UPS battery (ensure holds gateway + switch + WAN devices for 30+ min)

### Setup Day (4–5 Hours Before Event)

- [ ] Power up UPS → gateway → switch → all APs (sequential, 30 sec each)
- [ ] Verify all APs light up green (DHCP + SSH reachable)
- [ ] Check gateway DHCP pool status (show first 10 leases in admin console)
- [ ] Test WAN: ping 8.8.8.8 from gateway (both Starlink + 5G)
- [ ] Simulate user login: join SSID on test phone, verify portal opens
- [ ] Run speed test from test device (should see ~2.5 Mbps down, max-burst briefly)
- [ ] Check monitoring dashboard loads (verify real-time stats updating)
- [ ] Post QR code signage + WiFi password signs at check-in
- [ ] Brief team on tech contact + escalation (publish Slack/WhatsApp channel for alerts)

### During Event

**Hour 0–1 (Onboarding rush):**
- [ ] Monitor gateway CPU + DHCP pool % (expect spikes)
- [ ] Watch first 10 user portal logins live (screenshot for QA)
- [ ] Check AP client distribution (should be roughly equal; rebalance if skewed)

**Hour 1–6 (Steady state):**
- [ ] Check gateway once per 30 min (uptime, DHCP lease churn, WAN latency)
- [ ] Monitor user complaints (Slack channel); respond with standard troubleshooting
- [ ] If AP goes offline: swap with spare AP (document serial, reason)

**Hour 6+ (Winding down):**
- [ ] Keep monitoring active (users may surge in late hours)
- [ ] Ensure UPS battery doesn't drop below 30% (recharge if needed)

### Post-Event (Evening, 1–2 Hours)

- [ ] Export all logs (DHCP, firewall, AP association) to USB + cloud
- [ ] Gracefully shut down gateway (sync config, safe reboot)
- [ ] Power down all equipment in reverse order (APs → switch → gateway → UPS)
- [ ] Disconnect all outdoor Cat6A runs (coil, label, store)
- [ ] Collect APs from marquee; inspect for damage; reset to factory (preserve config backup)
- [ ] Generate post-event report: active user peak, onboarding success %, top issues
- [ ] Deactivate 5G SIM if not multi-month (stop billing)

---

## Success Metrics (Production)

| Metric | Target | Consequence if Missed |
|---|---|---|
| **Concurrent users at peak** | 400–500 active | Capacity planning for next event |
| **Onboarding success rate** | >95% | Portal UX review needed |
| **Median user throughput** | 2–3 Mbps | Traffic policy tuning |
| **AP uptime per AP** | >99% | Investigate interference / rogue clients |
| **WAN uptime (combined)** | >99.5% | Dual-WAN strategy validation |
| **User support tickets** | <20 per 1000 users | Documentation adequacy check |
| **Captive portal complaints** | <5 per 1000 users | Portal UX / device compatibility review |
| **Average session duration** | 45–90 min | Behavioral insights for marketing |

---

## Cost-to-Event Ratio

| Scenario | Total Capex | Events Needed to Break Even | Per-Event Cost |
|---|---|---:|---:|
| 1 event/year (5 years) | £20,000 | 5 | £4,000 |
| 2 events/year (5 years) | £20,000 | 10 | £2,000 |
| 3+ events/year (5 years) | £20,000 | 15+ | <£1,500 |

**Recommendation:** If 2+ events planned per year, permanent setup is financially justified.

---

## Long-Term Maintenance & Upgrades

### Year 1 (Post-Event)

- Firmware updates for all APs (quarterly)
- Captive portal customization (branding, terms updates)
- WAN cost optimization (re-negotiate 5G SIM pricing)

### Year 2–3

- AP performance trending (monitor client density metrics; consider adding capacity)
- Gateway license renewal (if Fortinet/Cisco; ~£500–1,000/year)
- Cabling inspection (weathering, UV damage; plan conduit upgrades)

### Year 4–5

- Consider next-gen AP refresh (WiFi 7 when mature + price drops)
- Evaluate cloud management (migrate to Ubiquiti Cloud or Meraki Dashboard if not already)
- Plan redundant gateway upgrade (active-active setup if demand warrants)

---

## Risk Mitigation

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Starlink terminal failure | Low | High (loss of primary WAN) | Keep spare terminal (~£400, battery-powered backup) |
| AP RF interference (rogue signal) | Medium | Medium (localized coverage gaps) | RF survey pre-event; change channels if needed |
| DHCP pool exhaustion | Low | High (new users can't login) | Monitor pool usage; increase lease timeout if needed |
| Gateway CPU overload | Low | High (traffic forwarding stops) | Monitor CPU; upgrade gateway if sustained >80% |
| Physical cable damage | Medium | Medium (AP goes offline) | Run cable under protective conduit; spare Cat6A onsite |
| Tech team unavailable (key person sick) | Low | High (no real-time troubleshooting) | Cross-train 2 people; document runbooks |

---

## Budget Allocation (Optional Enhancements)

### If Budget >£25,000

- Add **dual-gateway active-active** setup: +£3,500
- Add **second PoE switch for redundancy**: +£1,200
- Add **enterprise monitoring (Meraki Dashboard)**: +£800/year
- Add **professional 24/7 support contract**: +£2,000/event

### If Budget £20,000–£25,000 (Recommended)

- Single gateway (Fortinet 80F)
- Single managed PoE switch
- 16 APs + 2 spare
- Starlink + 5G backup
- Basic monitoring (native dashboard)
- 1-person tech support onsite

### If Budget <£20,000

- Reduce to **12 APs** (coverage limit to 300–350 concurrent)
- Single WAN (Starlink only, accept higher risk)
- Smaller PoE switch (24 port, less future-proof)
- Open-source monitoring (Netdata)

---

## Next Steps to Production

1. **Confirm capex budget** (£20k–£24k for recommended setup)
2. **Select gateway vendor** (Fortinet 80F recommended; Ubiquiti if simpler preferred)
3. **Procure hardware** (8–12 week lead time for bulk orders)
4. **Schedule installation training** (1 day: cabling, AP mounting, gateway config)
5. **Test in controlled environment** (pre-event rehearsal with limited user load)
6. **Plan first live event** (go live with production hardware)
7. **Iterate & scale** (use post-event data to refine capacity for next event)

---

## References

- [Pilot Plan (£4k temporary)](jalsa-wifi-temporary-pilot-4k.md)
- [Cost-Optimized Starter (5 AP rental)](jalsa-wifi-cost-optimized-5-ap.md)
- [Linux-Based Low-Cost Option](jalsa-wifi-linux-low-cost-plan.md)
