# Ventoy
Ventoy is an open source tool to create bootable USB drive for ISO/WIM/IMG/VHD(x)/EFI files.
With ventoy, you don't need to format the disk over and over, you just need to copy the ISO/WIM/IMG/VHD(x)/EFI files to the USB drive and boot them directly.        

You can copy many files at a time and ventoy will give you a boot menu to select them.
You can also browse ISO/WIM/IMG/VHD(x)/EFI files in local disks and boot them.
x86 Legacy BIOS, IA32 UEFI, x86_64 UEFI, ARM64 UEFI and MIPS64EL UEFI are supported in the same way.
Most types of OS supported (Windows/WinPE/Linux/ChromeOS/Unix/VMware/Xen...)     

#### 1100+ image files are tested | 90%+ distributions in [Distrowatch](https://distrowatch.com) are supported

### Features:

* 100% open source (license)
* Very simple to use (Get started)
* Fast (limited only by the speed of copying iso file)
* Can be installed in USB/Local Disk/SSD/NVMe/SD Card
* Directly boot from ISO/WIM/IMG/VHD(x)/EFI files, no extraction needed
* Support to browse and boot ISO/WIM/IMG/VHD(x)/EFI files in local disk Notes
* No need to be continuous in disk for ISO/WIM/IMG/VHD(x)/EFI files
* Both MBR and GPT partition styles are supported
* x86 Legacy BIOS, IA32 UEFI, x86_64 UEFI, ARM64 UEFI, MIPS64EL UEFI supported
* IA32/x86_64 UEFI Secure Boot supported Notes
* Linux Persistence supported Notes
* Windows auto installation supported Notes
* Linux auto installation supported Notes
* Variables Expansion supported for Windows/Linux auto installation script Notes
* FAT32/exFAT/NTFS/UDF/XFS/Ext2(3)(4) supported for main partition
* ISO files larger than 4GB supported
* Menu alias, Menu tip message supported
* Password protect supported
* Native boot menu style for Legacy & UEFI
* Most type of OS supported, 1100+ iso files tested
* Linux vDisk(vhd/vdi/raw...) boot solution Notes
* Not only boot but also complete installation process
* Menu dynamically switchable between ListView and TreeView mode Notes
* "Ventoy Compatible" concept
* Plugin Framework and GUI plugin configurator
* Injection files to runtime enviroment
* Boot configuration file dynamically replacement
* Highly customizable theme and menu style
* USB drive write-protected support
* USB normal use unaffected
* Data nondestructive during version upgrade
* No need to update Ventoy when a new distro is released

#### [SOURCE](https://github.com/ventoy/Ventoy) | [HOME](https://www.ventoy.net/)

#### Install:
```
kcp -i ventoy
```
