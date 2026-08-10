# Security Architecture

## Purpose

This document describes the security practices used within my HomeLab.

The goal is to reduce unnecessary exposure, protect sensitive data, and provide secure access to services without making the environment unnecessarily complex.

---

## Security Approach

My HomeLab uses several layers of security rather than relying on a single tool.

These layers include:

* Local network isolation
* Secure remote access
* Encrypted connections
* VPN-isolated Internet traffic
* Encrypted private storage
* Access controls
* Backups
* Secret management

When appropriate the best approach to security is the rule of least priviledges. Never give anymore access to anyone/anything then they need to do their job.

---

## Remote Access

Remote access to the HomeLab is provided through Tailscale.

Tailscale creates an encrypted private network between authorized devices, allowing HomeLab services to be accessed remotely without exposing their management interfaces directly to the public Internet.

Traditional Internet-facing port forwarding is avoided whenever possible.

---

## Local Network Access

Most HomeLab services are intended to be accessed from trusted devices on the local network or through Tailscale.

Administrative interfaces are not intentionally exposed directly to the public Internet.

This reduces the number of services reachable by unsolicited external traffic.

---

## HTTPS and Reverse Proxy

Caddy provides reverse proxy functionality for supported HomeLab services.

Using a reverse proxy provides a centralized location for managing service access and encrypted HTTPS connections where configured.

Direct port-based access remains available for some services when appropriate.

---

## DNS

AdGuard Home provides centralized DNS services and network-wide filtering.

It helps block known advertising and tracking domains while also supporting local name resolution for HomeLab services.

DNS filtering is an additional protective layer and is not treated as a replacement for other security controls.

*Admittedly I set up adguard well, however I am finding issues with the Echo dot 3 being able to connect to the network. 8/9/2026

---

## Download Stack Isolation

The automated download stack is isolated behind Gluetun.

Applications configured to share Gluetun's network namespace route their outbound Internet traffic through an encrypted VPN tunnel.

Examples include:

* qBittorrent
* Prowlarr
* Sonarr
* Radarr
* Lidarr
* Bazarr

Gluetun also provides firewall controls that help prevent these applications from bypassing the VPN connection if the tunnel becomes unavailable.

Other HomeLab services are not routed through Gluetun unless explicitly configured to use it.

---

## Private Data

Sensitive personal files are stored using an encrypted vault.

The encrypted vault data is stored separately from its unlocked view and is backed up while encrypted.

This allows backup copies of private files to remain protected when the vault is locked.

---

## SSH

SSH is used for remote administration of the HomeLab server.

Public-key authentication is preferred for administrative connections where configured.

Private SSH keys are never stored in the public GitHub repository.

---

## Secrets and Credentials

Passwords, API keys, VPN credentials, authentication tokens, private keys, and other secrets are excluded from the public repository.

Sensitive configuration files such as `.env` files are protected through `.gitignore` rules and are replaced with sanitized examples when documentation requires them.

Public documentation should never contain production credentials.

---

## Backups

Backups form part of the overall security strategy by providing protection against accidental deletion, configuration errors, storage failures, and other forms of data loss.

Important configuration data, encrypted private data, selected libraries, and persistent application data are backed up separately from primary storage.

See `backup-strategy.md` for additional information.

---

## Security Principles

The HomeLab follows several basic security principles:

* Avoid unnecessary Internet exposure.
* Prefer encrypted connections.
* Keep secrets out of version control.
* Restrict administrative access to trusted devices.
* Isolate applications when appropriate.
* Maintain recoverable backups.
* Keep services and the operating system updated.
* Favor simple security controls that can be understood and maintained.

Security is treated as an ongoing process and will continue to evolve as the HomeLab grows.
