# Setting Up an Ubuntu Server VM on ARM using QEMU (on x86)

## 1. Install QEMU
To install QEMU on Ubuntu, run the following command:

```bash
sudo apt update
sudo apt install qemu-system-arm qemu-efi-aarch64 qemu-utils
```

## 2. Create a Disk Image
Next, create the disk image for your VM. This will be named `ubuntu-arm64.qcow2`:

```bash
qemu-img create -f qcow2 ubuntu-arm64.qcow2 20G
```

## 3. Download the Ubuntu Server ISO
Download the Ubuntu server ISO using the `wget` command:

```bash
wget -O ubuntu-24.04.1-live-server-arm64.iso https://releases.ubuntu.com/24.04/ubuntu-24.04.1-live-server-arm64.iso
```

## 4. Boot the Ubuntu Server VM with QEMU (ARM Emulation)
Run the following command to boot up the Ubuntu server VM emulating ARM:

```bash
qemu-system-aarch64 \
   -machine virt \
   -cpu cortex-a57 \
   -m 2048 \
   -smp 2 \
   -nographic \
   -bios /usr/share/qemu-efi-aarch64/QEMU_EFI.fd \
   -drive if=none,file=ubuntu-arm64.qcow2,id=hd0 \
   -device virtio-blk-device,drive=hd0 \
   -cdrom ./ubuntu-24.04.1-live-server-arm64.iso \
   -netdev user,id=net0 \
   -device virtio-net-device,netdev=net0 \
   -boot d
```

## 5. Follow Installation Instructions in the VM
The VM will now start. Follow these instructions:

1. **Try or Install Ubuntu**: Select the option to install Ubuntu.
2. **Wait for Downloading**: The installer will download necessary components.
3. **Continue in Basic Mode**: Choose basic mode if prompted.
4. **Update to the New Installer**: Select the option to update.
5. **Wait for Downloads**: Let the installer finish downloading files.
6. **Proceed Through the Installation**: Keep pressing "Done" and "Continue" as needed.
7. **Configure Profile**: Enter your name, server name, and set up a password.
8. **Enable OpenSSH**: Make sure to check the option to install OpenSSH server.
9. **Finish the Installation**: Complete the setup and when prompted, the system will ask for a reboot.

## 6. Boot from the Installed Disk
Instead of rebooting the system, shut down the VM and restart it using the following command to boot from the disk image:

```bash
qemu-system-aarch64 \
   -machine virt \
   -cpu cortex-a57 \
   -m 2048 \
   -smp 2 \
   -nographic \
   -bios /usr/share/qemu-efi-aarch64/QEMU_EFI.fd \
   -drive if=none,file=ubuntu-arm64.qcow2,id=hd0 \
   -device virtio-blk-device,drive=hd0 \
   -netdev user,id=net0 \
   -device virtio-net-device,netdev=net0
```
