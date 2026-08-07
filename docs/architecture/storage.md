# Storage Architecture

## Purpose

This document describes how storage is organized within my HomeLab.

The storage layout is designed to separate applications, persistent data, backups, and media while keeping the system easy to maintain and recover.

---

## Design Goals

The storage architecture follows several principles:

* Keep application data separate from media.
* Store persistent Docker data independently of the operating system.
* Simplify backups and disaster recovery.
* Organize data into predictable locations.

---

## Why `/srv`? (Service Folder)

Linux provides several standard directories for different purposes. This HomeLab stores application and service data under `/srv`, the standard location for data provided by system services.

Keeping HomeLab resources under `/srv` separates application data from the operating system, making the environment easier to organize, back up, and migrate to new hardware.

## Directory Layout

The primary storage areas include:

| Directory      | Purpose                                      |
| -------------- | -------------------------------------------- |
| `/srv/apps`    | Application files and Docker projects        |
| `/srv/storage` | User data, media libraries, and shared files |
| `/srv/backup`  | Backup destination for important data        |

---

## Media Organization

Media is organized into dedicated directories to simplify management by applications such as Jellyfin, Kavita, Navidrome, and RomM.

Examples include:

* Movies
* TV Shows
* Music
* Books
* Comics
* Manga
* ROMs
* Downloads

---

## Backup Strategy

Important application data and personal files are backed up on a consistent schedule that can be customized.

Large media libraries can be managed independently to reduce backup time and storage requirements.

---

## Design Decisions

The storage layout emphasizes consistency.

Using a small number of well-defined directories makes it easier to locate data, automate backups, and migrate the HomeLab to new hardware in the future.

(Tested with a Raspberry Pi 4b to Beelink Ser 5)
