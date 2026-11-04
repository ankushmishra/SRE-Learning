# Ubuntu Desktop Installation in VirtualBox

This note records the Ubuntu lab setup on a Windows 11 PC. Ubuntu is installed
inside a VirtualBox virtual machine (VM); it is not installed over Windows.

## Host and download

- Host OS: Windows 11 Home
- Processor: Intel Core Ultra 5
- Memory available to Windows: about 15.4 GB
- Ubuntu image: Ubuntu 26.04.1 LTS Desktop, 64-bit PC (AMD64)
- ISO file: `C:\Users\Ankush\Downloads\ubuntu-26.04.1-desktop-amd64.iso`
- The ISO was downloaded from Ubuntu's official release site and its SHA-256
  checksum was checked against Ubuntu's published `SHA256SUMS`.

## Virtual machine configuration

Created a VM named **SRE Learning Ubuntu VM** with:

- Guest OS: Ubuntu (64-bit)
- RAM: 6144 MB (6 GB)
- CPUs: 1 virtual CPU
- Virtual disk: 40 GB
- Graphics controller: VBoxSVGA
- Video memory: 128 MB
- 3D acceleration: disabled
- Ubuntu ISO attached as the optical drive
- Network: VirtualBox NAT

The VM's virtual disk is separate from the Windows disk. Ubuntu's “Erase disk
and install Ubuntu” option applies to this VM's virtual disk only.

## Installation steps completed

1. Installed Oracle VirtualBox on Windows using the standard installer.
2. Created the VM with the hardware settings above and attached the Ubuntu ISO.
3. Started from the ISO. The initial boot showed a graphics warning and a
   blank screen.
4. Powered off the VM, increased video memory to 128 MB, and tried safe
   graphics. The VM also stalled with an NMI backtrace warning.
5. Powered off the VM and reduced its processor count from 2 to 1.
6. Booted using **Ubuntu (safe graphics)**. Ubuntu then loaded the live
   environment and installer.
7. Chose **Erase disk and install Ubuntu**. This is safe here because the
   installer is operating on the VM's new 40 GB virtual disk, not the Windows
   drive.
8. Chose **No encryption** for this practice VM.
9. Left Active Directory settings empty; this is a personal lab, not a
   work-managed domain.
10. The installer reached **Setting up the system**. A “System program problem
    detected” dialog appeared; the recommended action was **Cancel**, not
    “Report problem.”

## Current status

The last observed screen was the installer’s **Setting up the system** progress
screen. A completed installation, restart prompt, and successful login have
not yet been confirmed. Continue with the steps below and update this section
after verifying the desktop starts from the virtual disk.

## Finish and verify

1. Wait for the installer to finish. If asked to restart, choose **Restart
   Now**.
2. If VirtualBox asks to remove the installation medium, press **Enter**.
   Otherwise, power off the VM after the restart request, open **Settings →
   Storage**, select the Ubuntu ISO optical drive, and remove/eject the ISO.
3. Start the VM again. It should boot from its virtual hard disk and show the
   Ubuntu login screen. Sign in with the local account created during setup.
4. Open Terminal in Ubuntu and run:

   ```bash
   whoami
   pwd
   uname -a
   python3 --version
   git --version
   ```

5. Record the username, Ubuntu version, and any missing tools in the Day 1
   notes. Do not record the account password in this repository.

## Troubleshooting notes

- A saved VM state can lock hardware settings. Close the VM and choose
  **Power Off** rather than **Save the machine state** before changing
  settings. If it is already saved, discard only the saved state; do not
  delete the VM or its virtual disk.
- If the boot screen goes blank, shut down the VM before changing graphics
  settings. Try VBoxSVGA, 128 MB video memory, and disabled 3D acceleration.
- If startup stalls with an NMI backtrace warning, try one virtual CPU and
  **Ubuntu (safe graphics)**.
- The Ubuntu installer may show disk-erasure warnings. Proceed only after
  confirming the selected target is the VM's virtual disk. Never select a
  Windows physical disk for this lab.
- The first live boot and installation can take several minutes.
