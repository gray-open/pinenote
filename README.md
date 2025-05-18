# PineNote Community Edition Notes and Resources

These are notes and resource links relating to the PineNote Community Edition, as shipped in May 2025.

## Resource Links

* [The Official PineNote Documentation](https://pine64.org/documentation/PineNote/_full/)
* [The PineNote Debian Getting Started Guide](https://pndeb.github.io/pinenote-tweaks/)
* [PineNote Debian Project Home](https://github.com/PNDeb/pinenote-debian-image)

## Initial Setup

First steps after receiving the device the following, all done over wifi via ssh.

1. [Apply the recommended official fix to the bootloader](#bootloader-fix)
2. [Backup the VCOM voltage reference](#backup-vcom-reference)
3. [Basic Waveform data backup](#basic-waveform-data-backup)
4. [Update the Debian OS](#debian-updates)
5. [Fix the broken Debian OS Update (Failure to boot)](#fix-the-broken-debian-update)

### Bootloader Fix

Devices in this batch required a bootloader fix, as per PINE64's official note here:

[Important note for buyers after October 2024](https://pine64.org/documentation/PineNote/_full/#important-note-for-buyers-after-october-2024)

> Dear new PineNote users, you may have noticed that suspend/resume is not  working on your new PineNote! You may also noticed that using  rkdeveloptool is not possible because you cannot trigger maskrom mode  using the magnet method.
>
> These issues are caused by differences in testing procedures of the shipped Debian-based image, and factory  flashing procedures. Namely, the factory used a Windows-based flashing  system, and it turned out that this system does overwrite crucial parts  of the u-boot bootloader with same only-partially working data. The  effect is that the PineNote will boot, but not suspend.
>
> [Fixing the issue](https://pine64.org/documentation/PineNote/_full/#fixing-the-issue)
>
> Fortunately the PineNote ships with a complete set of bootloader files and fixing  the issue is a matter of a few shell commands:, which are flashing a  working U-Boot to get MASKROM with magnet working on PineNote:
>
> 1. Login via UART-adapter or ssh, or use the Gnome-Terminal (found in the quick  slots of the dashboard - switch to BW+Dithering mode for faster screen  responses):
>    - username: `user`
>    - password: `1234`
> 2. Gain root access by executing `sudo su - root` (the password is: `1234`).
> 3. Execute the following (two) commands (as root):
>
> ```console
> cd /root/uboot
> 
> bash install_stable_1056mhz_uboot.sh
> ```
>
> 1. Turn off the PineNote by executing `init 0`
> 2. Done. The pinenote should now have a proper U-Boot installed, with rkdeveloptool-support and suspend.
>
> ------
>
> The output of step 3 should read:
>
> ```console
> root@pinenote:~/uboot# bash install_stable_1056mhz_uboot.sh
> 568+0 records in
> 568+0 records out
> 290816 bytes (291 kB, 284 KiB) copied, 0.00419942 s, 69.3 MB/s
> 8192+0 records in
> 8192+0 records out
> 4194304 bytes (4.2 MB, 4.0 MiB) copied, 0.447542 s, 9.4 MB/s
> If not errors were reported, then the 1056 MHz u-boot/ram-blob was installed
> Please reboot
> root@pinenote:~/uboot# init 0
> ```

### Backup VCOM Reference

> **IMPORTANT ! Store your individual VCOM voltage now**
>
> Each eink (EBC) panel must be operated with a specific biasing voltage, the so-called VCOM voltage. At this point, we do not have the capabilities to measure this voltage on a given PineNote. Therefore, it is important to read out the factory-set VCOM voltage once and write it down. Hopefully you won't need it in the future, but this could become relevant in case the provided U-Boot boot loader gets overwritten by flashing a boot loader that overwrites the VCOM voltage.
>
> You can read out the VCOM voltage by executing the following command in a shell (either in GNOME, using the UART console, or via ssh):
>
> ```
> cat /sys/module/tps65185_regulator/drivers/i2c\:tps65185/3-0068/regulator/regulator.29/microvolts
> ```

### Basic Waveform Data Backup 

Simply copied `/usr/lib/firmware` from the device, although only the `rockchip` directory is the waveform data. Full contents were:

```bash
/usr/lib/firmware/
├── BCM2033-FW.bin
├── BCM2033-MD.hex
├── brcm
│   ├── BCM43430A1.hcd
│   ├── BCM43430B0.hcd
│   ├── BCM4345C0.hcd
│   ├── BCM4345C5.hcd
│   ├── bcm43xx-0.fw
│   ├── bcm43xx_hdr-0.fw
│   ├── brcmfmac43012-sdio.bin -> ../cypress/cyfmac43012-sdio.bin
│   ├── brcmfmac43012-sdio.clm_blob -> ../cypress/cyfmac43012-sdio.clm_blob
│   ├── brcmfmac43143.bin
│   ├── brcmfmac43143-sdio.bin
│   ├── brcmfmac43236b.bin
│   ├── brcmfmac43241b0-sdio.bin
│   ├── brcmfmac43241b4-sdio.Advantech-MICA-071.txt
│   ├── brcmfmac43241b4-sdio.bin
│   ├── brcmfmac43241b4-sdio.Intel Corp.-VALLEYVIEW C0 PLATFORM.txt
│   ├── brcmfmac43241b5-sdio.bin
│   ├── brcmfmac43242a.bin
│   ├── brcmfmac4329-sdio.bin
│   ├── brcmfmac4330-sdio.bin
│   ├── brcmfmac4330-sdio.Prowise-PT301.txt
│   ├── brcmfmac43340-sdio.ASUSTeK COMPUTER INC.-TF103CE.txt
│   ├── brcmfmac43340-sdio.bin -> ../cypress/cyfmac43340-sdio.bin
│   ├── brcmfmac43340-sdio.Insyde-VESPA2.txt
│   ├── brcmfmac43340-sdio.meegopad-t08.txt
│   ├── brcmfmac43340-sdio.pov-tab-p1006w-data.txt
│   ├── brcmfmac43340-sdio.predia-basic.txt
│   ├── brcmfmac4334-sdio.bin
│   ├── brcmfmac4335-sdio.bin
│   ├── brcmfmac43362-sdio.ASUSTeK COMPUTER INC.-ME176C.txt
│   ├── brcmfmac43362-sdio.bin -> ../cypress/cyfmac43362-sdio.bin
│   ├── brcmfmac43362-sdio.cubietech,cubietruck.txt
│   ├── brcmfmac43362-sdio.kobo,aura.txt -> brcmfmac43362-sdio.WC121.txt
│   ├── brcmfmac43362-sdio.kobo,tolino-shine2hd.txt -> brcmfmac43362-sdio.WC121.txt
│   ├── brcmfmac43362-sdio.lemaker,bananapro.txt -> brcmfmac43362-sdio.cubietech,cubietruck.txt
│   ├── brcmfmac43362-sdio.WC121.txt
│   ├── brcmfmac4339-sdio.bin -> ../cypress/cyfmac4339-sdio.bin
│   ├── brcmfmac43430a0-sdio.bin
│   ├── brcmfmac43430a0-sdio.ilife-S806.txt
│   ├── brcmfmac43430a0-sdio.jumper-ezpad-mini3.txt
│   ├── brcmfmac43430a0-sdio.ONDA-V80 PLUS.txt
│   ├── brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.beagle,beaglev-starlight-jh7100-a1.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.beagle,beaglev-starlight-jh7100-r0.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.bin -> ../cypress/cyfmac43430-sdio.bin
│   ├── brcmfmac43430-sdio.clm_blob -> ../cypress/cyfmac43430-sdio.clm_blob
│   ├── brcmfmac43430-sdio.friendlyarm,nanopi-r1.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.Hampoo-D2D3_Vi8A1.txt
│   ├── brcmfmac43430-sdio.ilife-S806.txt
│   ├── brcmfmac43430-sdio.MUR1DX.txt
│   ├── brcmfmac43430-sdio.raspberrypi,3-model-b.txt
│   ├── brcmfmac43430-sdio.raspberrypi,model-zero-2-w.txt -> brcmfmac43430-sdio.raspberrypi,3-model-b.txt
│   ├── brcmfmac43430-sdio.raspberrypi,model-zero-w.txt -> brcmfmac43430-sdio.raspberrypi,3-model-b.txt
│   ├── brcmfmac43430-sdio.sinovoip,bananapi-m64.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.sinovoip,bpi-m2-plus.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.sinovoip,bpi-m2-ultra.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.sinovoip,bpi-m2-zero.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.sinovoip,bpi-m3.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43430-sdio.starfive,visionfive-v1.txt -> brcmfmac43430-sdio.AP6212.txt
│   ├── brcmfmac43455-sdio.acepc-t8.txt
│   ├── brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.beagle,am5729-beagleboneai.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.bin -> ../cypress/cyfmac43455-sdio.bin
│   ├── brcmfmac43455-sdio.clm_blob -> ../cypress/cyfmac43455-sdio.clm_blob
│   ├── brcmfmac43455-sdio.MINIX-NEO Z83-4.txt
│   ├── brcmfmac43455-sdio.pine64,pinebook-pro.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,pinenote-v1.1.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,pinenote-v1.2.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,pinephone-pro.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,quartz64-a.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,quartz64-b.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,rockpro64-v2.0.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,rockpro64-v2.1.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,soquartz-blade.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,soquartz-cm4io.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.pine64,soquartz-model-a.txt -> brcmfmac43455-sdio.AW-CM256SM.txt
│   ├── brcmfmac43455-sdio.raspberrypi,3-model-a-plus.txt -> brcmfmac43455-sdio.raspberrypi,3-model-b-plus.txt
│   ├── brcmfmac43455-sdio.raspberrypi,3-model-b-plus.txt
│   ├── brcmfmac43455-sdio.raspberrypi,4-compute-module.txt -> brcmfmac43455-sdio.raspberrypi,4-model-b.txt
│   ├── brcmfmac43455-sdio.raspberrypi,4-model-b.txt
│   ├── brcmfmac43455-sdio.raspberrypi,5-model-b.txt -> brcmfmac43455-sdio.raspberrypi,4-model-b.txt
│   ├── brcmfmac43455-sdio.Raspberry Pi Foundation-Raspberry Pi 4 Model B.txt -> brcmfmac43455-sdio.raspberrypi,4-model-b.txt
│   ├── brcmfmac43455-sdio.Raspberry Pi Foundation-Raspberry Pi Compute Module 4.txt -> brcmfmac43455-sdio.raspberrypi,4-model-b.txt
│   ├── brcmfmac4350c2-pcie.bin
│   ├── brcmfmac4350-pcie.bin
│   ├── brcmfmac4354-sdio.bin -> ../cypress/cyfmac4354-sdio.bin
│   ├── brcmfmac4354-sdio.clm_blob -> ../cypress/cyfmac4354-sdio.clm_blob
│   ├── brcmfmac43569.bin
│   ├── brcmfmac4356-pcie.bin -> ../cypress/cyfmac4356-pcie.bin
│   ├── brcmfmac4356-pcie.clm_blob -> ../cypress/cyfmac4356-pcie.clm_blob
│   ├── brcmfmac4356-pcie.gpd-win-pocket.txt
│   ├── brcmfmac4356-pcie.Intel Corporation-CHERRYVIEW D1 PLATFORM.txt
│   ├── brcmfmac4356-pcie.Xiaomi Inc-Mipad2.txt
│   ├── brcmfmac4356-sdio.AP6356S.txt
│   ├── brcmfmac4356-sdio.bin -> ../cypress/cyfmac4356-sdio.bin
│   ├── brcmfmac4356-sdio.clm_blob -> ../cypress/cyfmac4356-sdio.clm_blob
│   ├── brcmfmac4356-sdio.firefly,firefly-rk3399.txt -> brcmfmac4356-sdio.AP6356S.txt
│   ├── brcmfmac4356-sdio.khadas,vim2.txt -> brcmfmac4356-sdio.AP6356S.txt
│   ├── brcmfmac4356-sdio.vamrs,rock960.txt -> brcmfmac4356-sdio.AP6356S.txt
│   ├── brcmfmac43570-pcie.bin -> ../cypress/cyfmac43570-pcie.bin
│   ├── brcmfmac43570-pcie.clm_blob -> ../cypress/cyfmac43570-pcie.clm_blob
│   ├── brcmfmac4358-pcie.bin
│   ├── brcmfmac43602-pcie.ap.bin
│   ├── brcmfmac43602-pcie.bin
│   ├── brcmfmac4366b-pcie.bin
│   ├── brcmfmac4366c-pcie.bin
│   ├── brcmfmac4371-pcie.bin
│   ├── brcmfmac4373.bin
│   ├── brcmfmac4373-sdio.bin -> ../cypress/cyfmac4373-sdio.bin
│   ├── brcmfmac4373-sdio.clm_blob -> ../cypress/cyfmac4373-sdio.clm_blob
│   ├── brcmfmac54591-pcie.bin -> ../cypress/cyfmac54591-pcie.bin
│   └── brcmfmac54591-pcie.clm_blob -> ../cypress/cyfmac54591-pcie.clm_blob
├── cypress
│   ├── cyfmac43012-sdio.bin
│   ├── cyfmac43012-sdio.clm_blob
│   ├── cyfmac43340-sdio.bin
│   ├── cyfmac43362-sdio.bin
│   ├── cyfmac4339-sdio.bin
│   ├── cyfmac43430-sdio.bin
│   ├── cyfmac43430-sdio.clm_blob
│   ├── cyfmac43455-sdio.bin
│   ├── cyfmac43455-sdio.clm_blob
│   ├── cyfmac4354-sdio.bin
│   ├── cyfmac4354-sdio.clm_blob
│   ├── cyfmac4356-pcie.bin
│   ├── cyfmac4356-pcie.clm_blob
│   ├── cyfmac4356-sdio.bin
│   ├── cyfmac4356-sdio.clm_blob
│   ├── cyfmac43570-pcie.bin
│   ├── cyfmac43570-pcie.clm_blob
│   ├── cyfmac4373-sdio.bin
│   ├── cyfmac4373-sdio.clm_blob
│   ├── cyfmac54591-pcie.bin
│   └── cyfmac54591-pcie.clm_blob
├── regulatory.db -> /etc/alternatives/regulatory.db
├── regulatory.db-debian
├── regulatory.db.p7s -> /etc/alternatives/regulatory.db.p7s
├── regulatory.db.p7s-debian
├── regulatory.db.p7s-upstream
├── regulatory.db-upstream
├── rockchip
│   ├── ebc_batch2.wbf
│   ├── ebc_orig.wbf
│   ├── ebc.wbf
│   └── rockchip_ebc_default_screen.bin
├── STLC2500_R4_00_03.ptc
├── STLC2500_R4_00_06.ssf
├── STLC2500_R4_02_02_WLAN.ssf
└── STLC2500_R4_02_04.ptc

4 directories, 148 files
```

### Debian Updates

Via ssh just the typical:

```bash
sudo apt-get update
sudo apt-get upgrade
```

Then rebooted.

### Fix the Broken Debian Update

The device failed to reboot after the initial Debian update. After the initial boot options screen the device got stuck in a loop on the boot splash screen. The screen flashed every few seconds, some terminal output was briefly visible. It never progressed past this point.

Although visually broken, the OS, given enough time, had booted normally and was accessible via ssh. Connecting via ssh revealed the upgrade was not complete, a few additional steps resolved the boot issue:

```bash
sudo apt-get update
sudo apt-get upgrade
```

Normal boot still broken. Rebooted and connected via SSH again:

```bash
sudo apt-get update
sudo apt-get dist-upgrade
```

Rebooted. Boot problems resolved.