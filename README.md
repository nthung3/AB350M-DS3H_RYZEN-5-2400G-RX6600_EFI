<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>
### GA-AB350M-DS3H-Hackintosh (OPENCORE 0.8.3)
	</key>
	<string>

### Before you give this EFI a try, make sure you read [this](#set-cpu-core-count), [this](#generating-your-own-serial-and-editing-rom)

This repo includes an OpenCore EFI for the GA-AB350M-DS3H motherboard running macOS Big Sur and Monterey.

Testing on:

Model | GA-AB350M
------------- | ---------------
CPU | AMD Ryzen 5 2400G
GPU | AMD Radeon RX 6600
RAM | 16 GB DDR4-3200 (Corsair Vengance RGB)
Disk (NVMe) | WD Black SN750 (Windows 10)
Disk (SATA) | KINGSPEC SATA 480GB (macOS)
Disk (Data) | HDD 500G
macOS | Monterey

## What works?

- Audio
- Boot
- USB
- Ethernet
- GPU acceleration
- Sleep
- [Power Management](#AMD-CPU-Power-Management)

## What doesn't work?

- microphone output

## Set CPU core count

Core Count patch needs to be modified to boot your system, if not using a 6 core CPU. Find the two `algrey - Force cpuid_cores_per_package` patches and alter the `Replace` value only.

Changing `B8000000 0000`/`BA000000 0000`/`BA000000 0090`* to `B8 <CoreCount> 0000 0000`/`BA <CoreCount> 0000 0000`/`BA <CoreCount> 0000 0090`* substituting `<CoreCount>` with the hexadecimal value matching your physical core count.

**Note:** *The two different values reflect the patch for different versions of macOS. Be sure to change all three if you boot macOS 10.15 to macOS 12*

See the table below for the values matching your CPU Core Count.

| CoreCount | Hexadecimal |
|--------|---------|
|   4 Core  | `04` |
|   6 Core  | `06` |
|   8 Core  | `08` |
|   12 Core | `0C` |
|   16 Core | `10` |

  So for example on a 8 Core CPU like the 2400G the `Replace` value would result in these replace values, `B8 08 0000 0000`/`BA 08 0000 0000`/`BA 08 0000 0090` (default configuration in this EFI)

## BIOS settings

[Dortania BIOS Guide](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html#amd-bios-settings)

Note: TPM can be left enabled if dual booting Windows 11.

## How to install

Download this repo and place the EFI folder into your internal drive's EFI partition... That's it.

## How to Install macOS:

There are two ways you can make a USB installer:

1. Have a working install of macOS, download the Installer from the App Store, then make a bootable Installer with the `createinstallmedia` command
2. If you are using Windows, use [macrecovery.py](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) from the offical OpenCore release package.

After you have created a bootable Installer, copy the EFI folder to the EFI partition of the installer drive and install as usual (GUID Partiton Map; APFS). After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## Generating your own serial and Editing ROM

Use GenSMBIOS (https://github.com/corpnewt/GenSMBIOS) to generate a serial for MacPro7,1

use PlistEdit Pro or any decent plist editor to manually enter the details in the following sections of the config (as shown in the video): (SystemSerialNumber, MLB, and UUID)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

You should also put in your ethernet adapter's MAC address into the ROM section.

## AMD CPU Power Management

While macOS might not officially support AMD CPU Power management, there are community efforts to add it. Specifically being `SMCAMDProcessor.kext` and `AMDRyzenCPUPowerManagement.kext`, both of which are included in the EFI, but diasbled by deafult.

Warning: They're known to create stability issues as well, if you're receiving random kernel panics or issues booting, do keep in mind these kexts may be the culprit, hence why they're disabled by default.

</string>
</dict>
</plist>
