# Homepage

## Purpose

Homepage is the main dashboard for my HomeLab.

It provides a single interface for accessing self-hosted services, viewing service status, and monitoring key parts of the homelab environment.

---

## Why I Use It

As the number of services in my HomeLab grew, keeping track of URLs, ports, and application status became inconvenient.

Homepage provides a centralized dashboard that makes the environment easier to navigate and manage.

---

## Deployment

Homepage runs as a Docker container using the official Homepage image.

The container:

* Uses the `latest` Homepage image
* Restarts automatically unless manually stopped
* Publishes port `3000`
* Stores configuration outside the container
* Has read-only access to the Docker socket
* Connects to both its default Docker network and the shared `caddy_proxy` network

---

## Networking

Homepage is available directly through port `3000` on the HomeLab server.

It is also connected to the shared `caddy_proxy` Docker network so Caddy can communicate with it directly.

Allowed hosts include:

* The local HomeLab hostname
* The local service address on port `3000`
* The HomeLab Tailscale address
* The HomeLab Tailscale hostname

Exact private network addresses are intentionally omitted from this public repository.

---

## Configuration

Homepage configuration is stored at:

```text
/srv/apps/docker/homepage/config
```

Inside the container, this directory is mounted at:

```text
/app/config
```

Keeping the configuration outside the container allows Homepage to be recreated or updated without losing dashboard settings.

---

## Docker Integration

Homepage has read-only access to the Docker socket:

```text
/var/run/docker.sock
```

This allows Homepage to retrieve information about running Docker containers and display service status information on the dashboard.

The socket is mounted read-only to limit what the container can do with Docker.

---

## Integration With My HomeLab

Homepage acts as the visual front door to the HomeLab.

It provides links and status information for services such as:

* Caddy
* Jellyfin
* Kavita
* RomM
* Uptime Kuma
* Beszel
* Portainer
* n8n
* Download Stack services

Additional services can be added to the dashboard as the HomeLab evolves.

---

## Maintenance

Homepage can be managed with Docker Compose from:

```text
/srv/apps/docker/homepage
```

Common administrative tasks include:

* Updating the container image
* Restarting the service
* Editing dashboard configuration
* Adding or removing services
* Verifying container status

Because configuration is stored outside the container, routine container updates do not remove dashboard settings.
