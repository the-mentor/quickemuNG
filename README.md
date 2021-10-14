<h1 align="center">
  <img src=".github/logo.png" alt="Quickemu" />
  <br />
  Quickemu
</h1>

<p align="center"><b>Quickly create and run optimised Windows, macOS and Linux desktop virtual machines.</b></p>
<div align="center"><img src=".github/screenshot.png" alt="Quickemu Screenshot" /></div>
<p align="center">Made with 💝 for <img src=".github/tux.png" align="top" width="18" /></p>

# Introduction

Quickly create and run highly optimised desktop virtual machines for Linux,
macOS and Windows; with just two commands. You decide what operating system you
want to run and Quickemu will figure out the best way to do it for you. For
example:

```bash
quickget ubuntu-mate hirsute
quickemu --vm ubuntu-mate-hirsute.conf
```

The original objective of the project was to enable quick testing of Linux
distributions where the virtual machine configurations can be stored anywhere,
such as external USB storage or your home directory, and no elevated permissions
are required to run the virtual machines. **Quickemu now also includes
comprehensive support for macOS and Windows**.

## Features

  * macOS High Sierra, Mojave, Catalina and Big Sur
  * Windows 8.1, 10 and 11 including TPM 2.0
  * Ubuntu, and all the official flavours, supported by `quickget`
  * Full SPICE support including host/guest clipboard sharing
  * VirtIO-webdavd file sharing for Linux and Windows guests
  * VirtIO-9p file sharing for Linux and macOS guests
  * Samba file sharing for Linux, macOS and Windows guests (*if `smbd` is installed on the host*)
  * VirGL acceleration
  * USB device pass-through
  * Smartcard pass-through
  * Automatic SSH port forwarding to guests
  * Network port forwarding
  * Full duplex audio
  * EFI and Legacy BIOS booting

Quickemu is a wrapper for the excellent [QEMU](https://www.qemu.org/) that
attempts to automatically *"do the right thing"*, rather than expose exhaustive
configuration options.

We have a Discord for this project: [![Discord](https://img.shields.io/discord/712850672223125565?color=0C306A&label=WimpysWorld%20Discord&logo=Discord&logoColor=ffffff&style=flat-square)](https://discord.gg/sNmz3uw)

See this (old) video where I explain some of my motivations for creating Quickemu.

[![Replace VirtualBox with Bash & QEMU](https://img.youtube.com/vi/AOTYWEgw0hI/0.jpg)](https://www.youtube.com/watch?v=AOTYWEgw0hI)

## Requirements

  * [QEMU](https://www.qemu.org/) (*6.0.0 or newer*)
  * [bash](https://www.gnu.org/software/bash/) (*4.0 or newer*)
  * [Coreutils](https://www.gnu.org/software/coreutils/)
  * [grep](https://www.gnu.org/software/grep/)
  * [jq](https://stedolan.github.io/jq/)
  * [procps](https://gitlab.com/procps-ng/procps)
  * [python3](https://www.python.org/)
  * [macrecovery](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery)
  * [mkisofs](http://cdrtools.sourceforge.net/private/cdrecord.html)
  * [usbutils](https://github.com/gregkh/usbutils)
  * [util-linux](https://github.com/karelzak/util-linux)
  * [sed](https://www.gnu.org/software/sed/)
  * [spicy](https://gitlab.freedesktop.org/spice/spice-gtk)
  * [swtpm](https://github.com/stefanberger/swtpm)
  * [Wget](https://www.gnu.org/software/wget/)
  * [xrandr](https://gitlab.freedesktop.org/xorg/app/xrandr)
  * [zsync](http://zsync.moria.org.uk/)

# Install Quickemu

## Ubuntu

Quickemu is available from a PPA for Ubuntu users. The Quickemu PPA also
includes a back port of QEMU 6.0.0 for 20.04 (Focal) and 21.04 (Hirsute).

```bash
sudo apt-add-repository ppa:flexiondotorg/quickemu
sudo apt install quickemu
```

## Other Linux

```bash
git clone https://github.com/wimpysworld/quickemu
cd quickemu
```

# Usage

## Ubuntu Guest

`quickget` will automatically download an Ubuntu release and also create the
virtual machine configuration.

```bash
quickget ubuntu focal
quickemu --vm ubuntu-focal.conf
```

  * Complete the installation as normal.
  * Post-install:
    * Install the SPICE agent (`spice-vdagent`) to enable copy/paste and USB redirection
      * `sudo apt install spice-vdagent`
    * Install the SPICE WebDAV agent (`spice-webdavd`) to enable file sharing.
      * `sudo apt install spice-webdavd`

### Ubuntu devel (daily-live) images

`quickget` can also download/refresh devel images via `zsync` for Ubuntu
developers and testers.

```bash
quickget ubuntu devel
quickemu --vm ubuntu-devel.conf
```

You can run `quickget ubuntu devel` to refresh your daily development image as
often as you like, it will even automatically switch to a new series.

### Ubuntu Flavours

All the official Ubuntu flavours are supported, just replace `ubuntu` with your
preferred flavour.

  * `kubuntu`
  * `lubuntu`
  * `ubuntu-budgie`
  * `ubuntu-kylin`
  * `ubuntu-mate`
  * `ubuntu-studio`
  * `xubuntu`

## Linux Guest

  * Download a .iso image of a Linux distribution
  * Create a VM configuration file; for example `debian-bullseye.conf`

```bash
guest_os="linux"
disk_img="debian-bullseye/disk.qcow2"
iso="debian-bullseye/firmware-11.0.0-amd64-DVD-1.iso"
```

  * Use `quickemu` to start the virtual machine:

```bash
quickemu --vm debian-bullseye.conf
```

  * Complete the installation as normal.
  * Post-install:
    * Install the SPICE agent (`spice-vdagent`) to enable copy/paste and USB redirection.
    * Install the SPICE WebDAV agent (`spice-webdavd`) to enable file sharing.

## macOS Guest

`quickget` automatically downloads a macOS recovery image and creates a virtual
machine configuration.

```bash
quickget macos catalina
quickemu --vm macos-catalina.conf
```

macOS `high-sierra`, `mojave`, `catalina` and `big-sur` are supported.

  * Use cursor keys and enter key to select the **macOS Base System**
  * From **macOS Utilities**
    * Click **Disk Utility** and **Continue**
      * On macOS Catalina and Big Sur
        * Select `Apple Inc. VirtIO Block Media` from the list and click **Erase**.
      * On macOS Mojave and High Sierra
        * Select `QEMU HARDDISK Media` (~68.72GB) from the list and click **Erase**.
    * Enter a `Name:` for the disk and click **Erase**.
    * Click **Done**.
    * Close Disk Utility
  * From **macOS Utilities**
    * Click **Reinstall macOS** and **Continue**
  * Complete the installation as you normally would.
    * On the first reboot use cursor keys and enter key to select **macOS Installer**
    * On the subsequent reboots use cursor keys and enter key to select the disk you named

The default macOS configuration looks like this:

```bash
guest_os="macos"
img="macos-catalina/RecoveryImage.img"
disk_img="macos-catalina/disk.qcow2"
macos_release="catalina"
```

  * `guest_os="macos"` instructs Quickemu to optimise for macOS.
  * `macos_release="catalina"` instructs Quickemu to optimise for a particular macOS release.
    * For example VirtIO Network and Memory Ballooning are available in Big Sur, but not previous releases.
    * And VirtIO Block Media (disks) are supported/stable in Catalina and newer.

### macOS compatibility

There are some considerations when running macOS via Quickemu.

  * `quickemu` will automatically download the required [OpenCore](https://github.com/acidanthera/OpenCorePkg)
    bootloader and OVMF firmware from [OSX-KVM](https://github.com/kholia/OSX-KVM).
  * Supported macOS releases:
    * High Sierra
    * Mojave
    * Catalina **(Recommended)**
    * Big Sur
  * Optimised by default
    * Host CPU vendor is detected and guest CPU is optimised accordingly.
    * [VirtIO Block Media](https://www.kraxel.org/blog/2019/06/macos-qemu-guest/) is used for the system disk where supported.
    * [VirtIO `usb-tablet`](http://philjordan.eu/osx-virt/) is used for the mouse.
    * VirtIO Network (`virtio-net`) is supported and enabled on macOS Big Sur but previous releases use `vmxnet3`.
    * VirtIO Memory Ballooning is supported and enabled on macOS Big Sur but disabled for other support macOS releases.
  * USB host pass-through is:
    * UHCI (USB 2.0) on macOS Catalina and earlier.
    * XHCI (USB 3.0) on macOS Big Sur.
  * Display resolution can only be changed via macOS System Preferences.
  * Full Duplex audio works on macOS High Sierra, Mojave and Catalina.
      * **macOS Big Sur has no audio at all**.
  * File sharing between guest and host is available via [virtio-9p](https://wiki.qemu.org/Documentation/9psetup).
  * **SPICE has limited support on macOS**:
    * Copy/paste via SPICE agent is not available.
    * File sharing via SPICE webdavd is not available.
    * USB pass-through via SPICE is not available.
    * Smartcard pass-through is not available.

## Windows 8.1, 10 & 11 Guests

`quickget` can automatically download Windows 8.1, [Windows 10](https://www.microsoft.com/en-gb/software-download/windows10ISO)
and [Windows 11](https://www.microsoft.com/en-gb/software-download/windows11)
along with the [VirtIO drivers for Windows](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/)
and creates a virtual machine configuration.

```bash
quickget windows 11
quickemu --vm windows-11.conf
```

  * Complete the installation as you normally would.
  * Post-install:
    * Run the VirtIO installer (`virtio-win-gt-x64`) from the CD Drive (E:).
    * Run the Guest Tools installer (`virtio-win-guest-tools`) from the CD Drive (E:).
    * Download and install [spice-webdavd](https://www.spice-space.org/download/windows/spice-webdavd/spice-webdavd-x64-latest.msi)
      * Enables file sharing between the host and guest.
    * Download and install [UsbDk](https://www.spice-space.org/download/windows/usbdk/)
      * Enables USB SPICE pass-through between the host and guest.

### Regional versions

By default `quickget` will download the *"English International"* release, but
you can optionally specify one of the supported languages: For example:

```bash
quickget windows 11 "Chinese (Traditional)"
```

The default Windows 11 configuration looks like this:

```bash
guest_os="windows"
disk_img="windows-11/disk.qcow2"
iso="windows-11/Win11_EnglishInternational_x64.iso"
fixed_iso="windows-11/virtio-win.iso"
tpm="on"
```

  * `guest_os="windows"` instructs `quickemu` to optimise for Windows.
  * `fixed_iso=` specifies the ISO image that provides VirtIO drivers.
  * `tpm="on"` instructs `quickemu` to create a software emulated TPM device using `swtpm`.

## FreeBSD Guest

`quickemu` supports FreeBSD production releases. FreeBSD support is maintained by `<kai@potabi.com>`.

```bash
quickget freebsd 13_0
quickemu --vm freebsd-13_0.conf
```

# SPICE

The following features are available while using the SPICE protocol:

  * Copy/paste between the guest and host
  * Host file sharing to the guest
  * USB device redirection

To use SPICE add `--display spice` to the Quickemu invocation, this requires that
the `spicy` client is installed, available from the `spice-client-gtk` package
in Debian/Ubuntu.

```bash
quickemu --vm ubuntu-focal.conf --display spice
```

# BIOS and EFI

Since Quickemu 2.1.0 `efi` is the default boot option. If you want to override
this behaviour then add the following line to you VM configuration to enable
legacy BIOS.

  * `boot="legacy"` - Enable Legacy BIOS boot

# Tuning CPU cores, RAM & disks

By default, Quickemu will calculate the number of CPUs cores and RAM to allocate
to a VM based on the specifications of your host computer. You can override this
default behaviour and tune the VM configuration to your liking.

Add additional lines to your virtual machine configuration:

  * `cpu_cores="4"` - Specify the number of CPU cores allocated to the VM
  * `ram="4G"` - Specify the amount of RAM to allocate to the VM
  * `disk_size="16G"` - Specify the size of the virtual disk allocated to the VM

## Disk preallocation

Preallocation mode (allowed values: `off` (default), `metadata`, `falloc`, `full`).
An image with preallocated metadata is initially larger but can improve performance
when the image needs to grow.

Specify what disk preallocation should be used, if any, when creating the system
disk image by adding a line like this to your VM configuration.

  * `preallocation="metadata"`

## CD-ROM disks

If you want to expose an ISO image from the host to guest add the following line
to the VM configuration:

   * `fixed_iso="/path/to/image.iso"`

## Floppy disks

If you're like [Alan Pope](https://popey.com) you'll probably want to mount a
floppy disk image in the guest. To do so add the following line to the VM
configuration:

  * `floppy="/path/to/floppy.img"`

# Network port forwarding

Add an additional line to your virtual machine configuration. For example:

  * `port_forwards=("8123:8123" "8888:80")`

In the example above:

  * Port 8123 on the host is forwarded to port 8123 on the guest.
  * Port 8888 on the host is forwarded to port 80 on the guest.

# USB redirection

Quickemu supports USB redirection via SPICE pass-through and host pass-through.

## SPICE redirection (recommended)

Using SPICE for USB pass-through is easiest as it doesn't require any elevated
permission, start Quickemu with `--display spice` and then select `Input` ->
`Select USB Device for redirection` from the menu to choose which device(s) you want
to attach to the guest.

## Host redirection

**USB host redirection is not recommended**, it is provided purely for backwards
compatibility to older versions of Quickemu. Using SPICE is preferred, see above.

Add an additional line to your virtual machine configuration. For example:

  * `usb_devices=("046d:082d" "046d:085e")`

In the example above:

  * The USB device with vendor_id 046d and product_id 082d will be exposed to the guest.
  * The USB device with vendor_id 046d and product_id 085e will be exposed to the guest.

If the USB devices are not writable, `quickemu` will display the appropriate
commands to modify the USB device(s) access permissions, like this:

```
 - USB:      Host pass-through requested:
              - Sennheiser Communications EPOS GTW 270 on bus 001 device 005 needs permission changes:
                sudo chown -v root:user /dev/bus/usb/001/005
                ERROR! USB permission changes are required 👆
```

# TPM

Since Quickemu 2.2.0 a software emulated TPM device can be added to guest
virtual machines. Just add `tpm="on"` to your VM configuration. `quickget` will
automatically add this line to Windows 11 virtual machines.

# All the options

Here are the usage instructions:

```
Usage
  quickemu --vm ubuntu.conf

You can also pass optional parameters
  --delete                : Delete the disk image.
  --display               : Select display backend. 'sdl' (default), 'gtk' or 'spice'
  --shortcut              : Create a desktop shortcut
  --snapshot apply <tag>  : Apply/restore a snapshot.
  --snapshot create <tag> : Create a snapshot.
  --snapshot delete <tag> : Delete a snapshot.
  --snapshot info         : Show disk/snapshot info.
  --status-quo            : Do not commit any changes to disk/snapshot.
  --fullscreen            : Starts VM in full screen mode (Ctl+Alt+f to exit)"
  --screen <screen>       : Use specified screen to determine the window size.
```

## Desktop shortcuts

Desktop shortcuts can be created for a VM, the shortcuts are saved in `~/.local/share/applications`. Here is an example of how to create a shortcut.

```bash
quickemu --vm ubuntu-focal-desktop.conf --shortcut
```

## Screen and window size

`qemu` will always default to the primary monitor to display the VM's window.

Without the `--screen` option, `quickemu` will look for the size of the smallest
monitor, and use a size that fits on said monitor.

The `--screen` option forces `quickemu` to use the size of the given monitor to
compute the size of the window. **It won't use that monitor to display the VM's
window if it's not the primary monitor**. This is useful if the primary monitor
if not the smallest one, and if the VM's window doesn't need to be moved around.

The `--screen` option is also useful with the `--fullscreen` option, again
because `qemu` will always use the primary monitor. In order for the fullscreen
mode to work properly, the resolution of the VM's window must match the
resolution of the screen.

To know which screen to use, type:

```bash
xrandr --listmonitors | grep -v Monitors
```

The command will output something like this:

```
 0: +*HDMI-0 2560/597x1440/336+1920+0  HDMI-0
 1: +DVI-D-0 1920/527x1080/296+0+0  DVI-D-0
```

The first number is what needs to be passed to the `--screen` option.

For example:

```bash
quickemu --vm vm.conf --screen 0
```

The above uses the 2560x1440 screen to compute the size of the window, which
Quickemu sizes to 2048x1152. Without the `--screen` option, Quickemu would have
used the 1920x1080 monitor which results in a window size of 1664x936.

# TODO

  - [ ] Add `spice-app` support via `virt-viewer`; *requires `virt-viewer` 8.0 or newer*
  - [ ] Add support for `ignore_msrs` for macOS. `echo "options kvm ignore_msrs=Y" >> /etc/modprobe.d/kvm.conf && update-initramfs -k all -u`
  - [ ] Add discrete GPU pass-through
  - [ ] Improve disk management

# References

Useful reference that assisted the development of Quickemu.

  * General
    * [QEMU’s documentation!](https://qemu.readthedocs.io/en/latest/)
    * <https://pve.proxmox.com/wiki/Qemu/KVM_Virtual_Machines>
    * <https://www.kraxel.org/blog/2020/01/qemu-sound-audiodev/>

  * macOS
    * <https://www.nicksherlock.com/2020/06/installing-macos-big-sur-on-proxmox/>
    * <https://passthroughpo.st/mac-os-adds-early-support-for-virtio-qemu/>
    * <https://github.com/kholia/OSX-KVM>
    * <https://github.com/thenickdude/KVM-Opencore>
    * <https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery>
    * <https://www.kraxel.org/blog/2017/09/running-macos-as-guest-in-kvm/>
    * <https://www.nicksherlock.com/2017/10/passthrough-of-advanced-cpu-features-for-macos-high-sierra-guests/>
    * <http://philjordan.eu/osx-virt/>
    * <https://github.com/Dids/clover-builder>
    * [OpenCore Configurator](https://mackie100projects.altervista.org)

  * Windows
    * <https://www.heiko-sieger.info/running-windows-10-on-linux-using-kvm-with-vga-passthrough/>
    * <https://leduccc.medium.com/improving-the-performance-of-a-windows-10-guest-on-qemu-a5b3f54d9cf5>
    * <https://frontpagelinux.com/tutorials/how-to-use-linux-kvm-to-optimize-your-windows-10-virtual-machine/>
    * <https://turlucode.com/qemu-command-line-args/>
    * <https://github.com/pbatard/Fido>
    * <https://www.catapultsystems.com/blogs/create-zero-touch-windows-10-iso/>

  * TPM
    * <https://qemu-project.gitlab.io/qemu/specs/tpm.html>
    * <https://www.tecklyfe.com/how-to-create-a-windows-11-virtual-machine-in-qemu/>

  * 9p & virtiofs
    * <https://wiki.qemu.org/Documentation/9p>
    * <https://wiki.qemu.org/Documentation/9psetup>
    * <https://www.kraxel.org/blog/2019/06/macos-qemu-guest/>
    * <https://superuser.com/questions/628169/how-to-share-a-directory-with-the-host-without-networking-in-qemu>
    * <https://virtio-fs.gitlab.io/>
