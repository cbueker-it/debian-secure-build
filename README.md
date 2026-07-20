**Debian Secure Built**

Secure Debian 13 Cinnamon workstation deployment with encrypted LVM, UFW firewall, connectivity validation, and SSH exposure remediation.

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

## LVM Write Confirmation

- Select the internal NVMe drive as the Debian installation target.
- Confirm the installer is applying partition changes to `/dev/nvme0n1`.
- Approve writing partition changes to disk.
- Begin Logical Volume Manager configuration.
- Red box: `/dev/nvme0n1` identifies the internal NVMe drive receiving the partition changes.
- Red box: `Write the changes to disks and configure LVM?` shows the installer confirmation prompt before disk changes are committed.
- Red box: `Yes` shows the approval option used to continue with LVM configuration.

<img src="images/lvm-write.jpg" alt="LVM Write Confirmation" width="700"/>

## Encrypted Volume Randomization

- Create the encrypted partition on the internal NVMe drive.
- Enter and confirm the encrypted volume passphrase.
- Allow Debian to overwrite `/dev/nvme0n1p3` with random data.
- Continue encrypted volume preparation before filesystem creation.
- Red box: the installer message shows Debian overwriting `/dev/nvme0n1p3` with random data.
- Red box: `Erasing data on /dev/nvme0n1p3` confirms the specific encrypted partition being prepared.
- Red box: the progress bar shows the randomization process running during encrypted volume setup.

<img src="images/encrypted-volume-random.jpg" alt="Encrypted Volume Randomization" width="700"/>

## Final Encrypted Partition Layout

- Review the final partition layout before completing disk partitioning.
- Confirm the EFI System Partition is present.
- Confirm the separate `/boot` partition is present.
- Confirm the encrypted volume is present.
- Confirm the LVM root volume is mounted at `/`.
- Confirm the swap volume is present.
- Confirm the SanDisk USB installer is listed separately from the internal NVMe drive.
- Finish partitioning and write changes to disk.
- Red box: `/dev/nvme0n1` identifies the internal NVMe drive used for the Debian installation.
- Red box: the LVM root and swap entries show the logical volumes created inside the encrypted storage layout.
- Red box: `Encrypted volume (nvme0n1p3_crypt)` shows the encrypted container used for the workstation installation.
- Red box: `Finish partitioning and write changes to disk` shows the final confirmation step before completing disk partitioning.

<img src="images/final-partition layout.jpg" alt="Final Encrypted Partition Layout" width="700"/>

## Workstation Software Selection

- Select the Debian desktop environment.
- Select Cinnamon for the workstation desktop environment.
- Select standard system utilities.
- Leave web server unchecked.
- Leave SSH server unchecked during installation.
- Continue the Debian software installation.
- Complete the installation.
- Remove the USB installation media.
- Reboot into the installed Debian system.
- Red box: `Choose software to install` identifies the installer stage used to select workstation software.
- Red box: `Cinnamon` shows the desktop environment selected for the Debian workstation.
- Red box: unchecked server roles show the system was not configured as a web server or SSH server during installation.
- Red box: `standard system utilities` shows standard Debian system tools were included.

<img src="images/software-selection.jpg" alt="Workstation Software Selection" width="700"/>

## First Boot Hostname and User Verification

- Boot into the installed Debian system.
- Enter the encryption passphrase during startup.
- Log into the Debian desktop.
- Open Terminal.
- Run `hostname`.
- Confirm the hostname is `CAB-DEB-DES-01`.
- Run `whoami`.
- Confirm the active user is `cbueker`.
- Red box: `CAB-DEB-DES-01` confirms the configured workstation hostname.
- Red box: `cbueker` confirms the active logged-in user account.

<img src="images/reboot-hostname-whoami.png" alt="First Boot Hostname and User Verification" width="700"/>

## UFW Firewall Baseline

- Install UFW.
- Check UFW status before enabling the firewall.
- Set the default firewall policy to deny incoming traffic.
- Set the default firewall policy to allow outgoing traffic.
- Enable UFW.
- Run `sudo ufw status verbose`.
- Confirm UFW is active.
- Confirm firewall logging is enabled.
- Confirm incoming traffic is denied by default.
- Confirm outgoing traffic is allowed by default.
- Red box: `Status: active` confirms the firewall is enabled.
- Red box: `Logging: on (low)` confirms firewall logging is active.
- Red box: `Default: deny (incoming), allow (outgoing)` confirms the baseline workstation firewall policy.
- Red box: `New profiles: skip` confirms no new application profiles were automatically applied.

<img src="images/ufw-active.png" alt="UFW Firewall Baseline" width="700"/>

## Network Connectivity Confirmation

- Run `ping -c 4 deb.debian.org`.
- Confirm Debian receives responses from `deb.debian.org`.
- Confirm the ping test reports `0% packet loss`.
- Run `sudo apt update`.
- Confirm Debian repositories are reachable.
- Confirm package metadata is current.
- Confirm the firewall does not block normal outbound traffic.
- Red box: `ping -c 4 deb.debian.org` shows the command used to test outbound network connectivity.
- Red box: the response times show successful replies from `deb.debian.org`.
- Red box: `4 packets transmitted, 4 received, 0% packet loss` confirms successful connectivity.
- Red box: `sudo apt update` shows the command used to test package repository access.
- Red box: `All packages are up to date` confirms APT successfully reached Debian repositories.

<img src="images/network-connectivity-confirmation.png" alt="Network Connectivity Confirmation" width="700"/>

## SSH Disabled and Port Review

- Review listening services with `ss -tulpen`.
- Identify SSH listening on port 22.
- Disable and stop SSH because remote access is not required for the baseline workstation.
- Run `sudo systemctl status ssh`.
- Confirm SSH is disabled.
- Confirm SSH is inactive.
- Rerun `ss -tulpen`.
- Confirm port 22 is no longer listening.
- Confirm remaining visible listening services are limited to expected local services, such as CUPS on localhost.
- Red box: `sudo systemctl status ssh` shows the command used to verify SSH service status.
- Red box: `disabled` confirms SSH is not configured to start automatically.
- Red box: `Active: inactive (dead)` confirms SSH is not currently running.
- Red box: `ss -tulpen` shows the command used to review listening sockets.
- Red box: `127.0.0.1:631` and `[::1]:631` show CUPS listening only on localhost.
- The absence of port `22` in the socket review confirms SSH is no longer exposed.

<img src="images/ssh-disabled-port-review.png" alt="SSH Disabled and Port Review" width="700"/>


