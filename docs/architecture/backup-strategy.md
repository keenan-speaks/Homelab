# Backup Strategy 

## Purpose

My HomeLab uses automated backups to protect important application configurations, personal data, libraries, and selected Docker data.

The backup strategy prioritizes data that would be difficult or impossible to replace while allowing large, replaceable media collections to be excluded when appropriate. (Videos, music, etc.)

---

## Backup Location

Backups are stored separately from the primary application and storage directories.

| Directory      | Purpose                                |
| -------------- | -------------------------------------- |
| `/srv/apps`    | Applications and Docker configurations |
| `/srv/storage` | Primary data and media storage         |
| `/srv/backup`  | Backup destination                     |

Separating the backup destination from the primary data locations helps protect against failures affecting the main storage environment.

---

## Backup Configuration

The backup system uses a configuration file to define which directories should be protected.

The current backup set is organized into several categories.

### Critical Infrastructure

Docker configurations and application data under `/srv/apps/docker` are included so containerized services can be reconstructed after a failure or migration.

### Synced Workspace

The HomeLab workspace under `/srv/storage/Homelab` is included in the backup set.

### Private Data

The encrypted vault data under `/srv/storage/.vault-encrypted` is backed up in its encrypted form.

This allows private data to remain encrypted while stored on the backup drive.

### Libraries

Selected libraries are protected, including:

* Books
* Comics
* Manga
* Music
* ROMs

### Docker Named Volumes

Important Docker named volumes containing persistent application data are also included.

This protects data that does not reside directly within the normal `/srv/apps` directory structure.

---

## Media Exclusions

Large media directories such as movies, TV shows, and videos are currently excluded from the regular backup set.

These collections can consume significantly more storage than application configurations and important personal data.

Excluding them allows available backup capacity to prioritize data that is more difficult to reconstruct or replace. I find that digital media is best backed up on external devices because they do not require settings that need to be restored and thus can be restored quickly.

---

## Automation

Backups are executed automatically using a systemd timer.

This allows the backup process to run on a regular schedule without requiring manual intervention.

Backup logs provide a record of completed operations and help identify warnings or failures.

---

## Recovery

The backup system is designed to support recovery after:

* Storage failure
* Operating system reinstallation
* Hardware migration
* Application configuration loss
* Accidental data loss

Keeping application configurations and persistent data separate from the operating system simplifies reconstruction of the HomeLab environment.

---

## Design Decisions

The backup strategy prioritizes recoverability rather than attempting to duplicate every file in the HomeLab.

Important configurations, private data, libraries, and persistent application data receive priority, while large replaceable media can be excluded when storage capacity makes full duplication impractical.

This approach keeps the backup system manageable while protecting the data most important to rebuilding the environment.
