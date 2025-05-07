<template>
  <div class="text-justify lh-base">
    <h3>Step 1 | Preparation</h3>
    Before diving into the installation, gather all the necessary tools and information. This
    includes a USB flash drive with at least 8GB of storage, a working desktop or laptop computer
    from which you'll perform the installation, a stable internet connection for downloading
    packages, and ensuring your computer has a USB port that allows booting from external media.
    You'll also need to download the official Arch Linux ISO file and a USB flashing tool like
    BalenaEtcher, Rufus, or Ventoy to write the ISO to your USB drive. Having a text editor handy
    (though one will be available in the live environment) and optionally an SSH client on another
    device can be helpful. Finally, keeping the Arch Linux Wiki readily accessible is highly
    recommended as your primary resource.
    <hr />
    <h3 class="mt-4">Step 2 | Preparing the Bootable USB Drive</h3>
    The next crucial step is to make your Arch Linux installation media. First, locate the
    downloaded Arch Linux ISO file on your computer. Then, download and install a USB flashing tool
    of your choice. Connect your USB drive to your computer, being absolutely sure that it doesn't
    contain any important data as the flashing process will erase everything. Launch the USB
    flashing tool, usually with administrator or root privileges. Within the tool, select the Arch
    Linux ISO file as the source image and your connected USB drive as the target device. Initiate
    the flashing process and wait for it to complete without interruption. Once the tool indicates
    success, safely eject the USB drive from your computer. Your bootable Arch Linux USB drive is
    now ready.
    <hr />
    <h3 class="mt-4">Step 3 | Booting into the Arch Linux Installation Environment</h3>
    To begin the installation, you need to boot your target computer from the USB drive you just
    created. Restart your computer and, during the initial startup sequence, press the
    <kbd>Del</kbd>, <kbd>F2</kbd>, <kbd>F12</kbd>, or <kbd>Esc</kbd> key (or the key specific to
    your manufacturer) to access the BIOS or UEFI settings. Navigate through the BIOS/UEFI menu to
    find the boot order settings. Change the boot order to prioritize booting from your USB drive,
    which might be listed as a removable device or under USB options. Save the changes you've made
    and exit the BIOS/UEFI. Your computer should automatically restart. If the boot order is
    correctly set, it will now boot from the Arch Linux USB drive, presenting you with a boot menu.
    Select the option to boot into the Arch Linux install environment. Once loaded, the system will
    attempt to establish an internet connection automatically, which you can verify using the
    command <kbd>ping archlinux.org</kbd>. If needed, use tools like <kbd>iwctl</kbd> (for Wi-Fi) or
    <kbd>dhcpcd</kbd> (for Ethernet) to manually configure your connection, referring to the Arch
    Wiki for guidance.
    <hr />
    <h3 class="mt-4">Step 4 | System Configuration and Initial Installation</h3>
    With a working internet connection in the live environment, the first step is to update the
    system clock using <kbd>timedatectl set-ntp true</kbd> and verify its status with
    <kbd>timedatectl status</kbd>. Next, identify your storage devices using <kbd>lsblk</kbd> to
    determine where you'll install Arch. Use a partitioning tool like <kbd>fdisk</kbd>,
    <kbd>cfdisk</kbd>, or <kbd>parted</kbd> to create the necessary partitions, typically including
    a root (<code>/</code>), swap, and optionally <code>/home</code> and an EFI System Partition
    (ESP) for UEFI systems. Format these partitions using <kbd>mkfs.ext4</kbd> for your main
    partitions, <kbd>mkswap</kbd> for swap, and <kbd>mkfs.fat -F32</kbd> for the ESP. Mount the root
    partition to <code>/mnt</code> using <kbd>mount /dev/your_root_partition /mnt</kbd>, and mount
    any other partitions like <code>/home</code> and <code>/boot/efi</code> under <code>/mnt</code>.
    Enable the swap partition with <kbd>swapon /dev/your_swap_partition</kbd>. Now, use the
    <kbd>pacstrap</kbd> script to install the essential Arch Linux packages with the command
    <kbd>pacstrap -K /mnt base linux linux-firmware nano dhcpcd vim</kbd>, adding any other
    necessary base packages. After installation, generate the file system table with
    <kbd>genfstab -U /mnt >> /mnt/etc/fstab</kbd> and review <kbd>/mnt/etc/fstab</kbd> to ensure
    accuracy.
    <hr />
    <h3 class="mt-4">Step 5 | Installing `git` and `base-devel` and `chroot`ing</h3>
    Before proceeding with Project Hyde, you need to install <kbd>git</kbd> and the
    <kbd>base-devel</kbd> group, which are prerequisites for cloning and potentially building
    software. While still in the live environment but with the base system installed to
    <code>/mnt</code>, execute the command <kbd>pacstrap -K /mnt git base-devel</kbd>. Once these
    are installed, you need to enter the newly installed system using the command
    <kbd>arch-chroot /mnt</kbd>. This changes your root directory to the mounted file system,
    allowing you to configure the installed Arch Linux system directly.
    <hr />
    <h3 class="mt-4">Step 6 | System Configuration within `chroot`</h3>
    Inside the `chroot` environment, set your time zone using
    <kbd>ln -sf /usr/share/zoneinfo/Region/City /etc/localtime</kbd> and run
    <kbd>hwclock --systohc</kbd>. Localize your system by editing <kbd>/etc/locale.gen</kbd>,
    uncommenting your desired locale(s), generating them with <kbd>locale-gen</kbd>, and setting the
    system-wide locale in <kbd>/etc/locale.conf</kbd>. Configure your network by setting a hostname
    with <kbd>echo yourhostname > /etc/hostname</kbd> and updating <kbd>/etc/hosts</kbd>. Set a
    strong password for the root user with the command <kbd>passwd</kbd>. Finally, install and
    configure a bootloader like GRUB (for both UEFI and BIOS) or systemd-boot (primarily for UEFI),
    following the specific installation commands and configuration steps for your chosen bootloader.
    For GRUB (UEFI), use <kbd>pacman -S grub efibootmgr</kbd>,
    <kbd
      >grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
      --boot-directory=/boot</kbd
    >, and <kbd>grub-mkconfig -o /boot/grub/grub.cfg</kbd>. For GRUB (BIOS), use
    <kbd>pacman -S grub os-prober</kbd> and
    <kbd>grub-install --target=i386-pc /dev/your_boot_drive</kbd>.
    <hr />
    <h3 class="mt-4">Step 7 | Installing Project Hyde</h3>
    With the base Arch system configured and a bootloader installed, you're ready to install Project
    Hyde. First, ensure you are still inside the `chroot` environment. Clone the Project Hyde
    repository to your home directory (which will be under <code>/root/</code> in `chroot`) using
    the command <kbd>git clone --depth 1 https://github.com/HyDE-Project/HyDE ~/HyDE</kbd>. Navigate
    into the Project Hyde scripts directory with <kbd>cd ~/HyDE/Scripts</kbd>. Now, execute the
    Project Hyde installation script using <kbd>./install.sh</kbd>. You can optionally customize the
    installation by creating or modifying <kbd>Scripts/pkg_user.lst</kbd> with additional packages
    you want to install and then running the script with this file as a parameter (e.g.,
    <kbd>./install.sh pkg_user.lst</kbd>). The script will automatically attempt to detect and
    install NVIDIA drivers if a compatible card is present and modify your bootloader configuration
    to enable NVIDIA DRM.
    <hr />
    <h3 class="mt-4">Step 8 | Finalizing and Rebooting</h3>
    After the Project Hyde installation script completes, exit the `chroot` environment by typing
    <kbd>exit</kbd>. Unmount all the mounted partitions with the command <kbd>umount -R /mnt</kbd>.
    Finally, reboot your system using the <kbd>reboot</kbd> command. Upon the first reboot, you
    should be greeted by the SDDM login screen (if a display manager was installed by the script or
    you had one from a previous step) or potentially a black screen. If you reach a black screen,
    you might need to log in via the console and start the display server or Hyprland manually with
    <kbd>exec Hyprland</kbd> (after installing it with Project Hyde). With a successful boot and
    login, you should now be experiencing your customized Arch Linux system powered by the Hyprland
    desktop environment from Project Hyde. Remember to consult the Project Hyde documentation for
    post-installation steps and further customization.
  </div>
</template>

<style scoped>
.text-justify {
  text-align: justify;
}

kbd {
  display: inline-block;
  padding: 0.2em 0.4em;
  font-size: 0.8em;
  color: #24292e;
  background-color: #f6f8fa;
  border: 1px solid #d1d5da;
  border-radius: 3px;
  font-family:
    ui-monospace,
    SFMono-Regular,
    SF Mono,
    Menlo,
    Consolas,
    Liberation Mono,
    monospace;
}

code {
  display: block; /* Make them take up the full line */
  padding: 0.8em;
  margin: 0.5em 0; /* Add some vertical spacing */
  font-size: 0.9em;
  color: #24292e;
  background-color: #f6f8fa;
  border-radius: 3px;
  font-family:
    ui-monospace,
    SFMono-Regular,
    SF Mono,
    Menlo,
    Consolas,
    Liberation Mono,
    monospace;
  overflow-wrap: break-word; /* Prevent horizontal scrolling */
}

code kbd {
  /* Style kbd within code blocks if needed */
  display: inline-block;
}
</style>
