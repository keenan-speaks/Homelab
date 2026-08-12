# Caddy

## Purpose

Caddy provides reverse proxy access to selected services in my HomeLab.

It is not currently used for every application. Instead, it provides an additional access path for a small number of services while direct port access remains available.

---

## Why I Use It

Caddy allows multiple services to be reached through a single reverse proxy configuration.

In the current HomeLab setup, it is used primarily for:

* Homepage
* Kavita
* Jellyfin

This provides a cleaner access path without requiring each application to manage proxying or HTTPS independently.

---

## Current Routes

The current Caddy configuration routes requests as follows:

| External Route                    | Internal Service        |
| --------------------------------- | ----------------------- |
| HomeLab Tailscale address         | Homepage on port `3000` |
| HomeLab Tailscale hostname        | Homepage on port `3000` |
| Tailscale hostname on port `8443` | Kavita on port `5000`   |
| Tailscale hostname on port `8444` | Jellyfin on port `8096` |

Exact private network addresses and tailnet hostnames are intentionally omitted from this public documentation.

---

## Networking

Caddy listens on standard web ports `80` and `443`, along with additional ports used for selected proxied services.

It communicates with containers such as Homepage, Kavita, and Jellyfin through the shared Docker network.

For example:

```text
Client
  |
  v
Caddy
  |
  v
Jellyfin :8096
```

Caddy receives the client request and forwards it to the appropriate internal Docker service.

---

## Direct Access

Caddy is not required to access most HomeLab services.

Applications can still be reached directly through their published ports when connected to the local network or through Tailscale.

For example, Jellyfin can still be accessed directly through its normal service port even though a Caddy route also exists.

This means Caddy currently acts as an optional reverse proxy layer rather than the sole entry point to the HomeLab.

---

## Configuration

The Caddy configuration is stored at:

```text
/srv/apps/docker/caddy/Caddyfile
```

The Docker project is located under:

```text
/srv/apps/docker/caddy
```

---

## Maintenance

Caddy is managed using Docker Compose.

Typical maintenance includes:

* Reviewing proxy routes
* Adding or removing services
* Restarting Caddy after configuration changes
* Checking container status
* Testing service access after changes

Because Caddy is not the only access method, direct port access provides a useful fallback if the reverse proxy configuration is unavailable.
