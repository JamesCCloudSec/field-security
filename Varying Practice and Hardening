## Operational Security: Toolkit Expansion, Encryption, and Backup Readiness

**This document outlines my practical implementation and learning across system hardening, encrypted storage, partition control, and backup strategy. It reflects real-world tasks completed over two full working sessions while building and deploying a functional, portable security toolkit for client work and personal application.** 

**Status: Tested on personal machine only. Full deployment at client site (Rina's Traditions [Ethan]) is planned.**

**Account Separation and Windows Hardening**
- Built a clean admin-standard user model on my personal system to simulate client environment.
- Admin accounts: SysAdmin and LaptopAdmin created for privileged tasks only.
- Standard accounts: JamesDesktop and JamesLaptop used for daily operations.
- Configured systems to require password on wake, 5-minute auto-lock, and prevent privilege escalation.
- Verified usability from standard user perspective and confirmed elevated access prompts work as expected.
- VeraCrypt Encryption and Partition-Level Control

**Gained full understanding of VeraCrypt encryption modes:**
- Standard container-based encryption (.hc files)
- Full partition encryption for non-OS data
- Risks and complexity of system encryption
- Created a new 12GB partition on my primary SSD without affecting the OS.
- Successfully encrypted the new partition and mounted it using an alternate drive letter.
- Removed the drive letter from the encrypted volume for cleanliness.
- Verified that mounting and unmounting work flawlessly.

**Controlled Folder Access**
- Enabled Controlled Folder Access (CFA) on Windows Defender.
- Added protection to test folders.
- Whitelisted tools like SyncBackFree to ensure backup compatibility.
- Simulated a restricted access scenario to verify CFA behavior.

**Toolkit Visibility and USB Cleanliness**
- Identified the VTOYEFI partition from Ventoy.
- Used DiskPart to remove the drive letter from VTOYEFI, keeping the bootloader functional while hiding unnecessary partitions in Explorer.
- Observed and validated that Windows no longer displayed the partition while maintaining USB boot reliability.
- RescueZilla and System Imaging
- Learned the RescueZilla live boot and imaging process.
- Tested boot on Ventoy.
- Estimated image size (12–25GB compressed) for clean systems without games/media.
- Confirmed how to identify source disks and select appropriate external targets.
- Plan to use RescueZilla for Ethan’s system snapshot after deployment.

**SyncBackFree File-Level Backup**
- Set up trial backup profiles to simulate client use.
- Compared Backup vs Mirror vs Sync behavior.
- Verified default log storage path.
- Discovered email notifications are not available in SyncBackFree; noted the upgrade path to SyncBackSE.
- Developed a plan to combine backup logging with Bitwarden-secured vaults for high-integrity retention.

**Partition Management & Best Practices**
- Practiced partition resizing and volume creation.
- Learned Windows limitations (e.g. can’t hide EFI volumes via Disk Management UI).
- Used DiskPart to remove drive letters on sensitive or boot-related partitions.
- Verified behavior when encrypted partitions are mounted and system letters change.
- Security Stack Operational Readiness
- System hardening (accounts, UAC, CFA)
- Encryption workflows (containers and partitions)
- File and system-level backup tooling (SyncBackFree + RescueZilla)
- Toolkit prep (ventoy, partition cleanup, vault design)
- Usability and post-deployment client behavior planning

## This document reflects a fully simulated security deployment tested on a non-client system. Next steps include deployment at Ethan’s environment using this framework.

Maintained by: James Castro | Date: 2025-04-14

