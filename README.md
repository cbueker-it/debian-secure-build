**Debian Secure Built**

Secure Debian 13 Cinnamon workstation deployment with encrypted LVM, UFW firewall, connectivity validation, and SSH exposure remediation.

Prepare Installation Media and Hardware

Create a bootable Debian 13 USB installer using a USB drive.
Connect the HP EliteDesk Mini to the monitor.
Connect the wired USB keyboard.
Connect the mouse.
Connect the HP power adapter.
Insert the Debian USB installer.
Power on the HP EliteDesk Mini.
Open the HP boot menu.
Select the Debian USB installer from the UEFI boot options.
Launch the Debian graphical installer.
Select language, location, and keyboard settings.
Configure wireless networking for the installer.
Configure the hostname as CAB-DEB-DES-01.
Leave the domain name blank because the system is not joined to an organizational domain.
Open the disk partitioning step.
Select guided partitioning with encrypted LVM.

LVM Write Confirmation

- Select the internal NVMe drive as the Debian installation target.
- Confirm the installer is applying partition changes to `/dev/nvme0n1`.
- Approve writing partition changes to disk.
- Begin Logical Volume Manager configuration.

<img src="images/lvm-write.jpg" alt="LVM Write Confirmation" width="700"/>

Encrypted Volume Randomization

Create the encrypted partition on the internal NVMe drive.
Enter and confirm the encrypted volume passphrase.
Allow Debian to overwrite /dev/nvme0n1p3 with random data.
Continue encrypted volume preparation before filesystem creation.

<img src="images/encrypted-volume-random.jpg" alt="Encrypted Volume Randomization" width="700"/>

Final Encrypted Partition Layout

Review the final partition layout before completing disk partitioning.
Confirm the EFI System Partition is present.
Confirm the separate /boot partition is present.
Confirm the encrypted volume is present.
Confirm the LVM root volume is mounted at /.
Confirm the swap volume is present.
Confirm the SanDisk USB installer is listed separately from the internal NVMe drive.
Finish partitioning and write changes to disk.

<img src="images/final-partition layout.jpg" alt="Final Encrypted Partition Layout" width="700"/>

Workstation Software Selection

Select the Debian desktop environment.
Select Cinnamon for the workstation desktop environment.
Select standard system utilities.
Leave web server unchecked.
Leave SSH server unchecked during installation.
Continue the Debian software installation.
Complete the installation.
Remove the USB installation media.
Reboot into the installed Debian system.

<img src="images/software-selection.jpg" alt="Workstation Software Selection" width="700"/>

First Boot Hostname and User Verification
Boot into the installed Debian system.
Enter the encryption passphrase during startup.
Log into the Debian desktop.
Open Terminal.
Run hostname.
Confirm the hostname is CAB-DEB-DES-01.
Run whoami.
Confirm the active user is cbueker.

<img src="images/reboot-hostname-whoami.png" alt="First Boot Hostname and User Verification" width="700"/>

UFW Firewall Baseline

Install UFW.
Check UFW status before enabling the firewall.
Set the default firewall policy to deny incoming traffic.
Set the default firewall policy to allow outgoing traffic.
Enable UFW.
Run sudo ufw status verbose.
Confirm UFW is active.
Confirm firewall logging is enabled.
Confirm incoming traffic is denied by default.
Confirm outgoing traffic is allowed by default.

<img src="images/ufw-active.png" alt="UFW Firewall Baseline" width="700"/>

Network Connectivity Confirmation
Run ping -c 4 deb.debian.org.
Confirm Debian receives responses from deb.debian.org.
Confirm the ping test reports 0% packet loss.
Run sudo apt update.
Confirm Debian repositories are reachable.
Confirm package metadata is current.
Confirm the firewall does not block normal outbound traffic.

<img src="images/network-connectivity-confirmation.png" alt="Network Connectivity Confirmation" width="700"/>

SSH Disabled and Port Review

Review listening services with ss -tulpen.
Identify SSH listening on port 22.
Disable and stop SSH because remote access is not required for the baseline workstation.
Run sudo systemctl status ssh.
Confirm SSH is disabled.
Confirm SSH is inactive.
Rerun ss -tulpen.
Confirm port 22 is no longer listening.
Confirm remaining visible listening services are limited to expected local services, such as CUPS on localhost.

<img src="images/ssh-disabled-port-review.png" alt="SSH Disabled and Port Review" width="700"/>
