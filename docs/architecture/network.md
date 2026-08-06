# Network Architecture

## Purpose

This document describes the network design of my Homelab and how its services communicate securely across the local network and remotely.

The primary goals of the network are:

* Secure remote access
* Reliable local connectivity
* Simple service management
* Network isolation where appropriate
* Easy maintenance and troubleshooting

---

## Network Components

The HomeLab network consists of the following core components:

* **Router** – Provides Internet connectivity and DHCP.
* **Gigabit Switch** – Connects the server and local devices.
* **Debian Server** – Hosts the Docker environment and self-hosted services.
* **Docker Networks** – Allow containers to communicate while remaining logically separated.
* **Caddy** – Reverse proxy for local service access.
* **AdGuard Home** – Network-wide DNS filtering and local name resolution.
* **Tailscale** – Secure remote access to the Homelab.
* **Gluetun** – VPN container used to isolate the download stack.

---

## Local Access

Most services are accessed from within the local network through Caddy using friendly hostnames or local addresses.

Examples include:

* Homepage
* Jellyfin
* Kavita
* RomM
* Portainer

This provides a consistent way to access services without exposing them directly to the Internet.

---

## Remote Access

Remote access is provided through Tailscale.

This allows me to securely connect to my Homelab while away from home without exposing management services through traditional port forwarding.

---

## Download Stack

Applications in the download stack route their outbound Internet traffic through the Gluetun VPN container.

This isolates download traffic from the rest of the HomeLab while allowing the applications to remain accessible from the local network for management.

---

## Design Decisions

Several decisions shaped the current network architecture:

* Docker is used to simplify deployment and maintenance.
* Tailscale was selected for secure remote access.
* AdGuard Home provides centralized DNS management.
* Gluetun isolates VPN-protected services from the rest of the environment.
* Services intended for administration remain accessible only through trusted local or Tailscale connections whenever possible.

---

## Future Improvements

Planned Improvements for the Homelab:

* Network segmentation using VLANs
* Additional monitoring and logging
* Improved traffic visualization
* Continued refinement of documentation
