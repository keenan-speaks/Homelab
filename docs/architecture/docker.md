# Docker Architecture

## Purpose

Docker is the foundation of my HomeLab. Nearly every application that I use in my homelab is deployed as a container, allowing services to remain isolated, portable, and easy to maintain. If one service breaks then the odds are greater that a problem wit the system  is isolated to a specific container. This speeds up troubleshooting when a problem occurs.

Using Docker Compose makes deployments reproducible and simplifies updates, backups, and migrations.

---

## Why Docker?

Docker packages an application together with everything it needs to run, including its libraries and dependencies.

This approach provides several advantages:

* Consistent deployments
* Simplified updates
* Easy backups
* Service isolation
* Hardware portability

If the HomeLab is migrated to another server, most applications can be restored by copying their configuration and starting the Docker Compose stack.

---

## Docker Compose

Each major service or application group has its own Docker Compose project.

This keeps related containers together while allowing services to be managed independently.

Examples include:

* Homepage
* Jellyfin
* Caddy
* AdGuard Home
* Open WebUI
* Download Stack
* Monitoring

---

## Data Storage

Container configuration and persistent data are stored outside the containers under the `/srv` directory.

Separating application data from containers allows services to be updated or recreated without losing configuration or user data.

---

## Networking

Docker networks allow containers to communicate securely without exposing every service directly to the local network.

Some services communicate only with other containers, while others are published for local or remote access.

The Download Stack uses a dedicated network configuration through Gluetun to isolate its outbound Internet traffic.

---

## Benefits

Using Docker provides several long-term benefits for the HomeLab:

* Easier maintenance
* Faster disaster recovery
* Cleaner upgrades
* Better organization
* Consistent deployments across different hardware

---

## Design Decisions

The HomeLab uses multiple independent Docker Compose projects instead of one large Compose file.

This modular approach allows services to be updated, restarted, or migrated individually without affecting unrelated applications.
