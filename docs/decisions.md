# Decision Log

One entry per decision. Newest at the top.

Template:

```markdown
## Title
- Date:
- Context:
- Options:
- Decision:
- Why:
- Trade-off:
```

---

## Pi-hole as the only DNS server

- Date: 2026-11-04
- Context: the router can hand out Pi-hole plus a fallback, but clients then pick either server, so ads leak through
- Options: Pi-hole only; Pi-hole + Quad9; two Pi-holes
- Decision: Pi-hole only, with a documented manual fallback to Quad9
- Why: complete filtering; the server should be up nearly always, and switching the router back takes a minute
- Trade-off: a server outage takes DNS down until fixed by hand; a second Pi-hole is the later fix

## Remove the discrete GPU

- Date: 2026-11-04
- Context: the tower came with a GTX 1050 Ti
- Decision: remove it and use the CPU's Quick Sync for Jellyfin
- Why: Quick Sync handles H.264 and HEVC (including 10-bit); most clients direct-play anyway; the card adds ~7–10W idle and needs NVIDIA drivers plus passthrough
- Trade-off: slower HDR-to-SDR tone mapping; the card stays on the shelf in case that becomes a problem

## Storage layout

- Date: 2026-11-04
- Context: one NVMe slot, 6 SATA ports, 4 bays, and several spare 2.5" drives on hand
- Decision: NVMe for Proxmox and VMs; FireCuda for media; spare 1TB and 500GB drives for scratch and optional VM backups; two bays kept free for a mirrored NAS pair in v3
- Why: fast storage where VMs run, free drives put to use, room left for RAID
- Trade-off: media isn't backed up until v3

## Matching RAM stick for 16GB

- Date: 2026-11-04
- Context: the tower had one 8GB stick; DDR4 prices are inflated in 2026
- Options: matching 8GB (16GB), single 16GB (24GB), 2×16GB kit (40GB)
- Decision: a used matching 8GB Ballistix stick
- Why: cheapest path to a matched dual-channel pair; 16GB covers v1 and Home Assistant
- Trade-off: tight once several VMs run; next upgrade is a matched pair in the free slots

## Server in the office behind an unmanaged switch

- Date: 2026-11-04
- Context: the server could plug straight into the router in the living room or sit in the office
- Decision: office, behind an unmanaged switch on the existing office run
- Why: next to the planned rack, easy hands-on access while learning, and the switch keeps the Mac Mini wired too
- Trade-off: fan noise in the office (recording is mostly direct; quiet takes move elsewhere), and one more device in the DNS path

## Desktop tower instead of the MacBook

- Date: 2026-11-04
- Context: an old gaming PC (Pentium G4600, B250 board) became available as an alternative to the 2012 MacBook Pro
- Options: MacBook Pro (4C/8T, 16GB fixed); desktop tower (2C/4T, upgradable)
- Decision: the tower; the MacBook becomes a portable Linux laptop
- Why: Quick Sync handles HEVC transcoding the MacBook can't; 6 SATA ports and 4 bays make v3 RAID possible; RAM upgradable to 64GB; no laptop workarounds (battery, lid switch, fan control)
- Trade-off: half the cores, ~10–15W more at idle, no built-in battery; CPU can be upgraded to a 7th-gen i5/i7 later

## Homepage for the dashboard

- Date: 2026-10-22
- Context: wanted one view of the host, VMs, and every service
- Options: Homepage, Homarr, Proxmox UI alone
- Decision: Homepage
- Why: live widgets for Proxmox, Pi-hole, Jellyfin, and Docker; configured in text files that live in Git
- Trade-off: configured by editing YAML rather than clicking

## Jellyfin for media

- Date: 2026-10-22
- Context: stream my own movies and shows at home
- Options: Jellyfin, Plex
- Decision: Jellyfin
- Why: fully free and open source, no account or cloud dependency
- Trade-off: less polished apps than Plex

## Pi-hole for DNS filtering

- Date: 2026-10-22
- Context: block ads and trackers for every device, including TVs and smart devices
- Options: Pi-hole, AdGuard Home
- Decision: Pi-hole
- Why: largest community, clear dashboard with measurable results
- Trade-off: encrypted upstream DNS needs extra setup (AdGuard has it built in)

## Tailscale for remote access

- Date: 2026-10-22
- Context: reach the homelab securely from outside the house
- Options: Tailscale, self-hosted WireGuard
- Decision: Tailscale
- Why: WireGuard underneath, no port forwarding, minimal setup
- Trade-off: depends on Tailscale's coordination servers; raw WireGuard is a possible later upgrade

## Docker Compose for services

- Date: 2026-10-22
- Context: run several services on one VM without them interfering
- Options: install directly on the OS, Docker Compose, one VM per service
- Decision: Docker Compose
- Why: isolated services, easy to rebuild, whole setup defined in files kept in Git
- Trade-off: Docker bypasses ufw, so published ports need explicit handling

## Ubuntu Server for the services VM

- Date: 2026-10-22
- Context: Linux distribution inside the Proxmox VM
- Options: Ubuntu Server 24.04 LTS, Debian 12
- Decision: Ubuntu Server
- Why: largest documentation base, long-term support
- Trade-off: slightly heavier than Debian

## Proxmox VE over bare-metal Ubuntu

- Date: 2026-10-22
- Context: single machine; want to experiment without breaking core services
- Options: Ubuntu + Docker directly, Proxmox + Ubuntu VM
- Decision: Proxmox
- Why: snapshots for safe changes, separate VMs for stable vs. experimental work, virtualization experience, web dashboard
- Trade-off: slower start, extra networking layer, ~1–2GB RAM overhead

## DHCP reservations instead of static IPs everywhere

- Date: 2026-10-22
- Context: which devices need fixed addresses
- Decision: fixed addresses only for the Proxmox host and services VM, managed as reservations on the router
- Why: only devices other things must find need a fixed IP; fixed IPs don't add security
- Trade-off: none significant

## Keep using the router's IoT network until v2

- Date: 2026-10-22
- Context: the router's IoT SSID uses client isolation on the same subnet, not a true VLAN
- Decision: keep using it now; replace with real VLANs in v2 alongside Home Assistant
- Why: blocks IoT devices from reaching computers today at no cost
- Trade-off: no fine-grained rules; blocks Home Assistant device discovery

## Small scope: v1 / v2 / v3

- Date: 2026-10-22
- Context: original plan was too broad to finish
- Decision: v1 = secure server + media + dashboard; v2 = home automation; v3 = backups and RAID
- Why: a finished, documented v1 beats a sprawling half-built plan
- Trade-off: no server-side backups until v3 (configs are in Git; DAW projects are backed up separately)

## Optical-bay SSD is optional (superseded)

_Superseded by "Desktop tower instead of the MacBook" and "Storage layout" (2026-11-04)._


- Date: 2026-10-22
- Context: whether to add a second drive now
- Decision: skip it until media outgrows the 500GB internal drive
- Why: without backups in v1, the only use was media; keeps v1 at $0
- Trade-off: limited media space
