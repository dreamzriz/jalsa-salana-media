# Jalsa Salana UK 2026 - Linux-Based Low-Cost Wi-Fi Plan (Charity Friendly)

## Goal

Build the lowest practical-cost guest Wi-Fi platform for one marquee using Linux and open-source software, while keeping basic reliability.

## Best Use Case

- You want to start with 5 APs now
- You can accept limited concurrent performance compared with enterprise solutions
- You have at least one volunteer with Linux networking experience

## Expected Capacity (5 AP Starter)

- Good user experience: about 120 to 180 active users
- Acceptable/basic: up to about 220 active users
- Not suitable for guaranteed 500 concurrent users without adding more APs

## Low-Cost Reference Architecture

- Internet WAN1: Starlink (primary)
- Internet WAN2: 5G router/business SIM (backup)
- Linux gateway: mini PC running Debian/Ubuntu LTS
- Managed PoE switch
- 5 x Wi-Fi 6 AP + 1 spare AP
- Captive portal + VLANs + DHCP + traffic shaping on Linux

## Minimum Hardware BOM (Starter)

| Item | Qty | Budget Notes |
|---|---:|---|
| Refurbished mini PC (Intel i5/i7, 16 GB RAM, SSD) | 1 | Very cost-effective as Linux gateway |
| USB 2.5GbE NIC or second NIC | 1 | Needed if mini PC has only one Ethernet port |
| Managed PoE+ switch (16 or 24 port) | 1 | Leave room for future AP expansion |
| Wi-Fi 6 APs (deployed) | 5 | Use same model to simplify support |
| Wi-Fi 6 AP (spare) | 1 | Rapid replacement |
| Starlink kit | 1 | Primary WAN |
| 5G router + business data SIM | 1 | Backup WAN |
| UPS | 1 | Keep gateway/switch online during power events |
| Outdoor Cat6A + patch leads + mounts | 1 lot | Install essentials |

## Suggested Open-Source Software Stack

- OS: Debian 12 or Ubuntu Server 24.04 LTS
- Firewall/NAT: nftables
- DHCP/DNS: dnsmasq
- Captive portal: OpenNDS
- AAA/session policy: FreeRADIUS
- Monitoring: Netdata + vnStat
- Traffic shaping: tc with CAKE/FQ_CoDel

## Network Segmentation (VLAN)

- VLAN 10: Guest Wi-Fi
- VLAN 20: Network management
- VLAN 30: Staff/operations

DHCP sizing:

- Guest scope: at least 600 IPs for starter phase
- Lease time: 4 hours during peak event periods

## Captive Portal Flow (24-hour Re-Login)

1. User joins SSID
2. QR code or portal intercept opens welcome page
3. Terms + privacy notice displayed
4. Optional Facebook visit/follow prompt shown
5. Internet access granted
6. Session expires at 24 hours, user must re-login

## Cost Estimate (UK, Starter Phase)

### Purchase model (charity-optimized)

- Approx GBP 5,500 to GBP 15,000 total

### Managed help add-on (optional)

- Add approx GBP 1,500 to GBP 6,000 for setup support/event-day engineer

## Trade-Offs You Should Accept

- More manual setup and testing effort
- Higher dependence on volunteer technical skill
- Less polished management UI than commercial controllers

## Command-Level Build Checklist (Ubuntu/Debian)

Note: commands below are a practical starting template and should be adapted to final interface names.

### 1) Base packages

```bash
sudo apt update
sudo apt install -y nftables dnsmasq freeradius freeradius-utils opennds vnstat curl git
```

### 2) Enable core services

```bash
sudo systemctl enable --now nftables
sudo systemctl enable --now dnsmasq
sudo systemctl enable --now freeradius
sudo systemctl enable --now opennds
sudo systemctl enable --now vnstat
```

### 3) IP forwarding

```bash
echo 'net.ipv4.ip_forward=1' | sudo tee /etc/sysctl.d/99-jalsa-forwarding.conf
sudo sysctl --system
```

### 4) Basic nftables NAT skeleton (example)

```bash
sudo tee /etc/nftables.conf >/dev/null <<'EOF'
#!/usr/sbin/nft -f
flush ruleset

table inet filter {
  chain input {
    type filter hook input priority 0;
    policy drop;
    iifname "lo" accept
    ct state established,related accept
    tcp dport {22} accept
    udp dport {53,67,68,1812,1813} accept
    tcp dport {53,2050} accept
  }

  chain forward {
    type filter hook forward priority 0;
    policy drop;
    ct state established,related accept
    iifname "br-guest" oifname "wan0" accept
    iifname "br-guest" oifname "wan1" accept
  }
}

table ip nat {
  chain postrouting {
    type nat hook postrouting priority 100;
    oifname "wan0" masquerade
    oifname "wan1" masquerade
  }
}
EOF
sudo systemctl restart nftables
```

### 5) dnsmasq guest scope example

```bash
sudo tee /etc/dnsmasq.d/guest.conf >/dev/null <<'EOF'
interface=br-guest
dhcp-range=10.10.0.10,10.10.2.250,255.255.252.0,4h
dhcp-option=3,10.10.0.1
dhcp-option=6,1.1.1.1,8.8.8.8
EOF
sudo systemctl restart dnsmasq
```

### 6) Traffic shaping baseline (replace rates to match WAN)

```bash
sudo tc qdisc replace dev wan0 root cake bandwidth 300mbit nat
sudo tc qdisc replace dev wan1 root cake bandwidth 100mbit nat
```

### 7) OpenNDS + FreeRADIUS integration

- Configure OpenNDS splash page and auth mode
- Configure FreeRADIUS user/session policy
- Set portal session timeout to 86400 seconds (24h)

## Event-Day Operations Checklist

1. Confirm WAN1/WAN2 failover
2. Test QR onboarding on Android and iOS
3. Verify 24-hour re-login policy
4. Check AP client count every 30 minutes
5. Keep one pre-configured spare AP ready
6. Monitor top bandwidth users and enforce shaping

## Expansion Path

- Start now: 5 AP
- If congestion appears: add 3 AP (total 8)
- Next step: add 4 AP (total 12)
- For full stronger coverage target: move toward 16 AP baseline

## Recommendation

This Linux path is the best way to keep cost low for a charity, but only if you allocate time for testing and have at least one technical owner during the event.
