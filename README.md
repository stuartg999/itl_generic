## itl_generic - VxWorks&reg; 7 Board Support Package (BSP) for Intel&reg; 32/64-bit 



<!--Copyright&copy; 2015-2018 Wind River Systems, Inc.-->

<!--The right to copy, distribute, modify or otherwise make use of this software-->
<!--may be licensed only pursuant to the terms of all applicable Wind River-->
<!--license agreement.-->

## Supported Boards

The **itl_generic** board support package (BSP) supports a variety of 32-bit and 64-bit Intel&reg; Architecture processors for VxWorks&reg; 7. It can therefore be used to boot VxWorks&reg; 7 on most Intel&reg;  boards.


## Supported Devices

| Hardware Interface     | VxBus Driver Name | VxWorks Driver Component | VxBus Driver Module Source File |
| ---------------------- | ---------------------- | ----------- | ----------- |
| UART:0                 | tbd   | DRV_SIO_PCI_NS16550 | vxbIaNs16550Sio.c |
| SATA                   | tbd    | tbd | vxbPciAhciSata.c |
| Timer                  | tbd     | DRV_TIMER_I8253 | vxbI8253Timer.c |
| Timer                  | tbd    | DRV_TIMER_IA_HPET | vxbIaHpetTimer.c |
| Timer                  | tbd    | DRV_TIMER_LOAPIC | vxbLoApicTimer.c |
| Timestamp              | tbd | DRV_TIMER_IA_TIMESTAMP | vxbIntelTimestamp.c |
| USB2-Host              | tbd     | tbd  | tbd |
| USB2-Host              | tbd     | tbd  | tbd |
| USB3-Host              | tbd     | tbd  | tbd |
| Ethernet               | tbd    | INCLUDE_GEI825XX_VXB_END | vxbGei825xxEnd.c |
| Ethernet               | tbd    | INCLUDE_FEI8255X_VXB_END | vxbFei8255xEnd.c |
| Ethernet               | tbd     | INCLUDE_RTL8169_VXB_END | vxbRtl8169End.c |
| Ethernet               | tbd    | DRV_VXBEND_TEI82598 | vxbTei82598End.c |
| RTC                    | tbd    | DRV_TIMER_MC146818 | vxbMc146818Rtc.c |
| Graphics               | tbd       | DRV_VGA_M6845 | vxbM6845Vga.c |
| Graphics               | tbd     | DRV_CONSOLE_EFI | vxbEfiConsole.c |
| PCI                    | tbd       | DRV_IA_PCI_BUS | vxbIaPciBus.c |
| PS2                    | tbd       | DRV_KBD_I8042 | vxbI8042Kbd.c |
| SDHC/MMC               | tbd   | DRV_PCI_SDHC_CTRL | vxbPciSdhcCtrl.c |
| CAN-PCIx/200 (SJA1000) | tbd | INCLUDE_PCI_SJA1000 | vxbPciSja1000.c |

For more information about how to configure these VxBus drivers beyond their defaults, refer to the  [VxWorks BSP Supplement](https://knowledge.windriver.com "VxWorks BSP Supplement") on the **Wind River Knowledge Library**.


## Unsupported Devices

| Hardware Interface     |
| ---------------------- |
| NVRAM                  |
| Video-Decode           |
| Audio                  |
| LPC                    |


## Prerequisites

1. Setting up the workstation:
  * Install the Wind River&reg; VxWorks&reg; 7 operating system on the workstation.
  * Configure a workspace directory from the command-line interface (CLI).

  For example, on Linux:
  ```
  $ cd WindRiver
  $ ./wrenv.linux –p vxworks-7
  $ mkdir workspace
  $ wrtool -data ~/WindRiver/workspace
  Active workspace is '/home/wruser/WindRiver/workspace'.
  WrTool, version 4.14.0
  Copyright (c) 2013 - 2016 Wind River Systems, Inc.
  
  WindRiver> cd workspace
  workspace>
  ```
  ​  For example, on Windows:
  ```
  C:\> cd WindRiver
  C:\WindRiver> wrenv –p vxworks-7
  C:\WindRiver> wrtool -data C:\WindRiver\workspace
  Active workspace is 'C:\WindRiver\workspace'.
  WrTool, version 4.13.0
  Copyright (c) 2013 - 2016 Wind River Systems, Inc.
  
  WindRiver> cd workspace
  workspace> 
  ```
2. Setting up the target:
  * Attach the target to a video monitor, a USB keyboard and a physical network cable.
  * Power on the target and enter the target's BIOS configuration. Change the BIOS boot settings to boot first from a USB flash drive. Apply this setting and then power off the target.

3. Preparing the USB flash drive boot device:
  * Locate a USB flash drive with a capacity of at least 2GB. You will boot the target board from this USB flash disk.
  * Locate the third-party program **mkbt.exe**. This program is publicly available on the Internet. You will use **mkbt.exe** during USB flash disk preparation. In the instructions that follow, you will create a VxWorks bootApp project. Once you have created a VxWorks bootApp project (vip_itl_generic_nehalem_bootapp), you must install the third-party program **mkbt.exe** to the project root directory.

4. Preparing the USB flash drive master boot code:
  * Locate a file named **bootsect.bin** in the Wind River product installation. Navigate to **installDir\vxworks-7\pkgs_v2\os\board\intel**  to locate the BSP directories. You will find the **bootsect.bin** file in various Intel&reg; BSP directories, although it is not in the itl_generic BSP directory. Once you have created a VxWorks bootApp project in the instructions that follow, copy the **bootsect.bin** file to the project root directory.

5. Locating essential documents within the Wind River Knowledge Library:
  * [VxWorks 7 Configuration and Build Guide](https://knowledge.windriver.com/Content_Lookup?id=043026 "VxWorks 7 Configuration and Build Guide")
  * [VxWorks 7 Boot Loader User's Guide](https://knowledge.windriver.com/Content_Lookup?id=044519 "VxWorks 7 Boot Loader User's Guide")

## Getting Started
### Introduction
You can use the **itl_generic** BSP to boot many different Intel&reg; targets in many different ways.

This document presents the most direct strategy for booting your target board successfully into VxWorks&reg;. This boot strategy is workable from both Linux and Windows workstations and will boot the majority of Intel&reg; target boards.

While these instructions involve the command-line interface, you may also employ Workbench to achieve the same goals.


### Creating the VxWorks&reg; Source Build Project

Follow [Creating a VxWorks Source Build Project from the Command Line]( <https://knowledge.windriver.com/en-us/000_Products/000/020/000/020/000/000_Configuration_and_Build_Guide,_Edition_9/010/030> "Creating a VxWorks Source Build Project from the Command Line") to create your VxWorks&reg; source build project.

**NOTE:** At step 3, substitute the following command:

```
workspace> prj vsb create -force -bsp itl_generic_1_0_7_0 -cpu NEHALEM -ilp32 vsb_NEHALEM_32_up -S -up
workspace> cd vsb_NEHALEM_32_up
```

Then build the VSB:

```
vsb_NEHALEM_32_up> wrtool prj build
vsb_NEHALEM_32_up> cd ..
```
### Creating the VxWorks&reg; Boot Application Project

The VxWorks&reg; boot application is a configuration profile that must be assigned to a new VxWorks&reg; image project. All available configuration profiles are described in [Configuration Profiles](https://knowledge.windriver.com/Content_Lookup?id=043026 "Configuration Profiles").

Follow the stages of [Preparing a Multi-Stage Boot Loader for Intel 64-bit Processors](https://knowledge.windriver.com/Content_Lookup?id=044519 "Preparing a Multi-Stage Boot Loader for Intel 64-bit Processors") to create and build the bootApp project.

**NOTE 1:** As you follow these stages, Wind River recommends you modify the following stage commands to align with the itl_generic BSP:<br>
**Stage 1-3** Omit these stages because these tasks have already been completed.<br>
**Stage 4:** Substitute with the following command:
```
workspace> prj vip create -force -vsb vsb_NEHALEM_32_up -profile PROFILE_BOOTAPP -ilp32 itl_generic_1_0_7_0 gnu vip_itl_generic_nehalem_bootapp
workspace> cd vip_itl_generic_nehalem_bootapp
```
**Stage 6:** Substitute with the following command: 
```
vip_itl_generic_nehalem_bootapp> prj vip component add INCLUDE_FAST_REBOOT
```
**Stage 8:** You set the default boot line differently on Linux host as compared to Windows host.
On Windows:
```
vip_itl_generic_nehalem_bootapp> prj vip parameter set DEFAULT_BOOT_LINE "\"fs(0,0)host:/vxWorks h=90.0.0.3 e=90.0.0.50 u=user o=gei"\"
```
On Linux:
```
vip_itl_generic_nehalem_bootapp> prj vip parameter set DEFAULT_BOOT_LINE '"fs(0,0)host:/vxWorks h=90.0.0.3 e=90.0.0.50 u=user o=gei"'
```
**NOTE 2:** The default boot line instructs the bootApp to load the VxWorks&reg; kernel image file from the USB flash disk root directory.<br>
**NOTE 3:** The default boot line assumes the bootApp will attach to an Intel&reg; Pro 1000 **gei** network device. If your Intel target does not contain this network device, you must set this boot option to another supported network device instead. The alternative boot devices are **fei**, **tei** or **rtl**.<br>
**Stage 9:** Using the **prj vip component add** command, add the following components to the VIP:

| **INCLUDE_FEI8255X_VXB_END** |
| ---------------------------- |
| **INCLUDE_GEI825XX_VSB_END** |
| **INCLUDE_RTL8169_VXB_END**  |
| **DRV_VXBEND_TEI82598**      |
| **INCLUDE_PC_CONSOLE**       |
| **DRV_KBD_USB**              |
| **INCLUDE_SYS_WARM_AHCI**    |

Complete bootApp project creation by running the following command:
```
vip_itl_generic_nehalem_bootapp> prj vip component remove DRV_KBD_I8042
```

### Creating the VxWorks&reg; Image Project
[Configuring a VxWorks Kernel Image Project from the Command Line](<https://knowledge.windriver.com/en-us/000_Products/000/020/000/020/000/000_Configuration_and_Build_Guide,_Edition_9/050/010> "Configuring a VxWorks Kernel Image Project from the Command Line") gives a description of how to create a VxWorks image project from the command-line interface. Wind River recommends you create the VIP as follows:

```
vip_itl_generic_nehalem_bootapp> cd ..
workspace> prj vip create -force -vsb vsb_NEHALEM_32_up -profile PROFILE_INTEL_GENERIC itl_generic_1_0_7_0 gnu vip_itl_generic_nehalem_kernel_up
workspace> cd vip_itl_generic_nehalem_kernel_up
```
Using the **prj vip component add** command, add the following components to the VIP:

| **INCLUDE_FAST_REBOOT**      |
| ---------------------------- |
| **INCLUDE_FEI8255X_VXB_END** |
| **INCLUDE_GEI825XX_VXB_END** |
| **INCLUDE_RTL8169_VXB_END**  |
| **DRV_VXBEND_TEI82598**      |
| **INCLUDE_PC_CONSOLE**       |
| **DRV_KBD_USB**              |

Complete VIP creation by running the following commands:
```
vip_itl_generic_nehalem_kernel_up> prj vip component remove DRV_KBD_I8042
vip_itl_generic_nehalem_kernel_up> prj vip bundle add BUNDLE_STANDALONE_SHELL
vip_itl_generic_nehalem_kernel_up> prj build
```
### Preparing the USB flash drive
#### Preparation Procedure on Windows

Create a bootable VxWorks&reg; boot app USB flash drive by following [Creating a Bootable USB Drive (Windows Host)](<https://knowledge.windriver.com/en-us/000_Products/000/020/000/060/010/000_Boot_Loader_User's_Guide,_Edition_13/030/010/000> "Creating a Bootable USB Drive (Windows Host)").

**NOTE 1:** As described in [Prerequisites](#Prerequisites), copy **mkbt.exe** and **bootsect.bin** to the bootApp root project directory.
**NOTE 2:** If you receive an I/O error when attempting to run clean from **diskpart** then you will be directed to the Windows system event log, where the error that has been logged will report **Cannot zero sectors on disk**. In this case, Wind River recommends that you restart this preparation procedure with a different USB flash disk. If that is not possible, repeat the procedure to create the USB flash disk partition on a Linux workstation instead. Once you have created the primary partition from the Linux workstation, return to the Windows workstation and re-run the USB flash disk preparation again on Windows with the USB flash disk.

#### Preparation Procedure on Linux
Follow [Creating a Bootable USB Drive (Linux Host)](<https://knowledge.windriver.com/en-us/000_Products/000/020/000/060/010/000_Boot_Loader_User's_Guide,_Edition_13/030/010/010> "Creating a Bootable USB Drive (Linux Host)") to create a bootable USB drive containing the VxWorks&reg; boot app.

**NOTE 1:** At step 4b you must also configure the **mtools** configuration file ***~/.mtoolsrc***:
```
# echo "mtools_skip_check=1" > ~/.mtoolsrc
```
**NOTE 2:** At step 4c, **mformat** will sometimes return an error:
```
# mformat u: -B $WIND_BASE/host/x86-linux2/bin/vxld.bin
mformat: Hidden (2048) does not match sectors (63)
```
In this case, subtract the value 8 from the 'hidden' sector count and apply this new value to the '-H' parameter. Re-run the **mformat** command and no error is generated:
```
# mformat u: -H 2040 -B $WIND_BASE/host/x86-linux2/bin/vxld.bin
```
### Copy the VxWorks Kernel Image File to the USB Flash Disk
In a previous step, you set the VxWorks boot application boot parameters to look for the VxWorks&reg; executable image file at the root of the USB flash disk.

Copy this file to the USB flash disk. For example:

```
# cd ..
# cd vip_itl_generic_nehalem_kernel_up/default
# mcopy vxWorks u:
# sync
# eject /dev/sda1
```
### Booting the Target
Remove the USB flash drive from the workstation.

Make sure the target is powered off and insert the USB flash disk into the target's USB connector.

Apply power to the target, and observe the VxWorks&reg; boot application on the target console while it loads the VxWorks&reg; kernel.

Once the target has booted, the VxWorks&reg; banner becomes visible on the target console.

![](./docs/vxworks_banner_2.PNG)

## Additional Documents
| Document Name                                                |
| :----------------------------------------------------------- |
| [VxWorks 7 Intel Architecture Guide](https://knowledge.windriver.com/Content_Lookup?id=045126 "VxWorks 7 Intel Architecture Guide") |
| [VxWorks 7 Device Configuration for the itl_generic BSP](https://knowledge.windriver.com/Content_Lookup?id=045923 "VxWorks 7 Device Configuration for the itl_generic BSP") |
| [VxWorks 7 Programmers Guide](https://knowledge.windriver.com/Content_Lookup?id=044250 "VxWorks 7 Programmers Guide") |
| [VxWorks 7 Boot Loader Users Guide](https://knowledge.windriver.com/Content_Lookup?id=044519 "VxWorks 7 Boot Loader Users Guide") |
| [VxWorks 7 Board Support Package Supplement](https://knowledge.windriver.com "VxWorks 7 Board Support Package Supplement")                  |
| [VxWorks 7 Configuration and Build Guide](https://knowledge.windriver.com/Content_Lookup?id=043026 "VxWorks 7 Configuration and Build Guide") |
