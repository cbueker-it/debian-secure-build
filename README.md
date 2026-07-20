**Debian Secure Build**

Secure Debian 13 Cinnamon workstation deployment with encrypted LVM, UFW firewall, connectivity validation, and SSH exposure remediation.

Project Overview

I ran into a practical workflow problem while building my technical portfolio. My Dell Latitude laptop worked, but the screen space was limited, and I was doing more documentation, screenshot review, browser research, terminal work, and GitHub writing at the same time. The smaller screen slowed down my workflow and made it harder to organize technical documentation efficiently.

This made me realize I needed a larger workstation setup. I wanted an economical build, but I also needed enough power to support Linux administration work, documentation, and future virtual machines. I chose an HP EliteDesk Mini because it gave me a small physical footprint, strong hardware, and enough system resources for a practical Linux workstation.

This project documents my secure deployment of a Debian 13 Cinnamon workstation on an HP EliteDesk Mini. My goal was to build a lean, secure, and powerful Linux workstation with encrypted storage, a basic firewall baseline, verified network connectivity, and reduced unnecessary service exposure.

This documentation is written to reflect a repeatable technical work instruction. I included key screenshots as evidence of important configuration and validation steps.

Objectives

- Deploy Debian 13 on physical workstation hardware.
- Configure encrypted LVM during installation.
- Verify the final partition layout before completing installation.
- Install a desktop workstation environment.
- Confirm successful first boot, hostname, and user account.
- Enable a baseline UFW firewall policy.
- Validate outbound network and package repository access.
- Disable SSH when remote access is not required.
- Review listening ports after service changes.

Environment

- Hardware: HP EliteDesk Mini
- Operating System: Debian 13
- Desktop Environment: Cinnamon
- Storage Configuration: Encrypted LVM
- Firewall: UFW
- Network: Wireless home network
- Installation Media: Bootable Debian USB installer

Prepare Installation Media and Hardware

- Create a bootable Debian 13 USB installer using a USB drive.
- Connect the HP EliteDesk Mini to the monitor.
- Connect the wired USB keyboard.
- Connect the mouse.
- Connect the HP power adapter.
- Insert the Debian USB installer.
- Power on the HP EliteDesk Mini.
- Open the HP boot menu.
- Select the Debian USB installer from the UEFI boot options.
- Launch the Debian graphical installer.
- Select language, location, and keyboard settings.
- Configure wireless networking for the installer.
- Configure the hostname as CAB-DEB-DES-01.
- Leave the domain name blank because the system is not joined to an organizational domain.
- Open the disk partitioning step.
- Select guided partitioning with encrypted LVM.

LVM Write Confirmation

- Select the internal NVMe drive as the Debian installation target.
- Confirm the installer is applying partition changes to `/dev/nvme0n1`.
- Approve writing partition changes to disk.
- Begin Logical Volume Manager configuration.
- `/dev/nvme0n1` identifies the internal NVMe drive receiving the partition changes.
- `Write the changes to disks and configure LVM?` shows the installer confirmation prompt before disk changes are committed.
- `Yes` shows the approval option used to continue with LVM configuration.

<img src="images/lvm-write.jpg" alt="LVM Write Confirmation" width="700"/>

Encrypted Volume Randomization

- Create the encrypted partition on the internal NVMe drive.
- Enter and confirm the encrypted volume passphrase.
- Allow Debian to overwrite `/dev/nvme0n1p3` with random data.
- Continue encrypted volume preparation before filesystem creation.
- The installer message shows Debian overwriting `/dev/nvme0n1p3` with random data.
- `Erasing data on /dev/nvme0n1p3` confirms the specific encrypted partition being prepared.
- The progress bar shows the randomization process running during encrypted volume setup.

<img src="images/encrypted-volume-random.jpg" alt="Encrypted Volume Randomization" width="700"/>

Final Encrypted Partition Layout

- Review the final partition layout before completing disk partitioning.
- Confirm the EFI System Partition is present.
- Confirm the separate `/boot` partition is present.
- Confirm the encrypted volume is present.
- Confirm the LVM root volume is mounted at `/`.
- Confirm the swap volume is present.
- Confirm the SanDisk USB installer is listed separately from the internal NVMe drive.
- Finish partitioning and write changes to disk.
- `/dev/nvme0n1` identifies the internal NVMe drive used for the Debian installation.
- The LVM root and swap entries show the logical volumes created inside the encrypted storage layout.
- `Encrypted volume (nvme0n1p3_crypt)` shows the encrypted container used for the workstation installation.
- `Finish partitioning and write changes to disk` shows the final confirmation step before completing disk partitioning.

<img src="images/final-partition layout.jpg" alt="Final Encrypted Partition Layout" width="700"/>

Workstation Software Selection

- Select the Debian desktop environment.
- Select Cinnamon for the workstation desktop environment.
- Select standard system utilities.
- Leave web server unchecked.
- Leave SSH server unchecked during installation.
- Continue the Debian software installation.
- Complete the installation.
- Remove the USB installation media.
- Reboot into the installed Debian system.
- `Choose software to install` identifies the installer stage used to select workstation software.
- `Cinnamon` shows the desktop environment selected for the Debian workstation.
- Unchecked server roles show the system was not configured as a web server or SSH server during installation.
- `standard system utilities` shows standard Debian system tools were included.

<img src="images/software-selection.jpg" alt="Workstation Software Selection" width="700"/>

First Boot Hostname and User Verification

- Boot into the installed Debian system.
- Enter the encryption passphrase during startup.
- Log into the Debian desktop.
- Open Terminal.
- Run `hostname`.
- Confirm the hostname is `CAB-DEB-DES-01`.
- Run `whoami`.
- Confirm the active user is `cbueker`.
- `CAB-DEB-DES-01` confirms the configured workstation hostname.
- `cbueker` confirms the active logged-in user account.

<img src="images/reboot-hostname-whoami.png" alt="First Boot Hostname and User Verification" width="700"/>

UFW Firewall Baseline

- Install UFW by running `sudo apt install ufw`.
- Check the firewall status by running `sudo ufw status verbose`.
- Confirm UFW is installed before applying firewall rules.
- Set the default incoming policy by running `sudo ufw default deny incoming`.
- Set the default outgoing policy by running `sudo ufw default allow outgoing`.
- Enable the firewall by running `sudo ufw enable`.
- Verify the active firewall configuration by running `sudo ufw status verbose`.
- Confirm UFW is active.
- Confirm firewall logging is enabled.
- Confirm incoming traffic is denied by default.
- Confirm outgoing traffic is allowed by default.
- `Status: active` confirms the firewall is enabled.
- `Logging: on (low)` confirms firewall logging is active.
- `Default: deny (incoming), allow (outgoing)` confirms the baseline workstation firewall policy.
- `New profiles: skip` confirms no new application profiles were automatically applied.

<img src="images/ufw-active.png" alt="UFW Firewall Baseline" width="700"/>

Network Connectivity Confirmation

- Run `ping -c 4 deb.debian.org`.
- Confirm Debian receives responses from `deb.debian.org`.
- Confirm the ping test reports `0% packet loss`.
- Run `sudo apt update`.
- Confirm Debian repositories are reachable.
- Confirm package metadata is current.
- Confirm the firewall does not block normal outbound traffic.
- `ping -c 4 deb.debian.org` shows the command used to test outbound network connectivity.
- The response times show successful replies from `deb.debian.org`.
- `4 packets transmitted, 4 received, 0% packet loss` confirms successful connectivity.
- `sudo apt update` shows the command used to test package repository access.
- `All packages are up to date` confirms APT successfully reached Debian repositories.

<img src="images/network-connectivity-confirmation.png" alt="Network Connectivity Confirmation" width="700"/>

SSH Disabled and Port Review

- Review listening services with `ss -tulpen`.
- Identify SSH listening on port 22.
- Disable and stop SSH because remote access is not required for the baseline workstation.
- Run `sudo systemctl status ssh`.
- Confirm SSH is disabled.
- Confirm SSH is inactive.
- Rerun `ss -tulpen`.
- Confirm port 22 is no longer listening.
- Confirm remaining visible listening services are limited to expected local services, such as CUPS on localhost.
- `sudo systemctl status ssh` shows the command used to verify SSH service status.
- `disabled` confirms SSH is not configured to start automatically.
- `Active: inactive (dead)` confirms SSH is not currently running.
- `ss -tulpen` shows the command used to review listening sockets.
- `127.0.0.1:631` and `[::1]:631` show CUPS listening only on localhost.
- The absence of port `22` in the socket review confirms SSH is no longer exposed.

<img src="images/ssh-disabled-port-review.png" alt="SSH Disabled and Port Review" width="700"/>


