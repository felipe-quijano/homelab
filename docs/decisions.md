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
- Trade-off: less polished apps than Plex; 4K transcoding will struggle on this CPU

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
- Context: single laptop; want to experiment without breaking core services
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

## Optical-bay SSD is optional

- Date: 2026-10-22
- Context: whether to add a second drive now
- Decision: skip it until media outgrows the 500GB internal drive
- Why: without backups in v1, the only use was media; keeps v1 at $0
- Trade-off: limited media space
