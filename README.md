## 📌 Project Overview

This repository documents the storage diagnostics, legacy partition table decommissioning, and native **Ext4** filesystem provisioning of a **3.0 TB external hard drive** (`TOSHIBA DT01ACA300`, `/dev/sda`) for cross-distribution Linux backup operations across **Pop!_OS (Debian/Ubuntu)** and **Nobara (Fedora/RHEL)** workstations.

## 🔍 Problem Diagnosis

- **Cross-Device Write Failures (`gio copy failed`):** Attempting to transfer files from local Linux storage to the external drive failed with `gio copy failed` (`cross_device_copy: true`) due to dirty legacy Windows NTFS volume states and root-owned mount permissions.
- **Fragmented Legacy Partition Layout:** Inspection of `/dev/sda` revealed a fragmented multi-partition layout left over from a prior Windows installation (multiple small system/recovery partitions alongside a `3.0 TB Basic data partition` and unmounted `No Automount` blocks).

## 🛠️ Storage Re-Provisioning & Filesystem Configuration

1. **Legacy Partition Table Wipe:**
   Unmounted all active volumes on `/dev/sda` and removed the fragmented legacy Windows and recovery partitions.

2. **GPT & Single-Volume Ext4 Formatting:**
   Initialized a clean **GUID Partition Table (GPT)** across the full `3.0 TB` (`3,000,592,982,016 bytes`) block device and formatted `/dev/sda1` as a native **Linux Filesystem (`Ext4`)**.

3. **Ownership & Cross-Distro Mount Validation:**
   Mounted `/dev/sda1` cleanly under user media paths (`/media/<USER>/back up drive` on Pop!_OS and `/run/media/<USER>/back up drive` on Fedora/Nobara), verifying standard `lost+found` initialization and non-root read/write permissions.

## ✅ Verification & Outcome

- **Full 2.7 TiB Usable Capacity Restored:** Consolidated fragmented partitions into a single, healthy native Linux volume.
- **Verified Multi-Gigabyte Backup Transfers:** Successfully transferred **20.7 GiB** of workstation backup archives (`6%` capacity utilized, `2.5 TiB` free) with zero cross-device copy errors across both Debian-based and Fedora-based hosts.
