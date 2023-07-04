# OC-A317-51G-78XF
OpenCore config for Acer Aspire A317-51G-78XF. Works with macOS 10.15, 11, 12 and 13.

## Setup
1. Generate a `MacBookPro16,2` SMBIOS and put it into config.plist
2. Format your USB stick
   1. Open Disk Utility
   2. Press Cmd+2 (or View -> Show all devices)
   3. Format USB stick in HFS+ (Mac OS Extended (Journaled)) with GUID partition map.
3. Create USB
   `sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/USB` where `USB` is name of your flash drive
4. Mount EFI partition (use [MountEFI](https://github.com/corpnewt/MountEFI) utility for that)
5. Put `EFI` directory in EFI partition
6. Unmount your USB stick & reboot
7. Enter BIOS
   1. On `Main` tab press Ctrl + S to show hidden parameter. Set it to `AHCI`
   2. On `Main` tab set `F12 Boot Menu` parameter to `Enabled`
   3. On `Security` tab set `Supervisor password`
   4. On `Boot` tab set `Secure Boot` to `Disabled`
   5. On `Security` tab set `Supervisor password` to empty
   6. Save & Reboot
8. (optional) Boot from your USB stick and choose `Reset NVRAM`<br>
   Note. This likely will erase all UEFI boot entries. If you want to dualboot Mac OS X with Linux or Windows, please follow guide on [Dortania](https://dortania.github.io/OpenCore-Multiboot/oc/linux.html).
11. Boot from your USB stick again and choose installer
12. Perform usual installation
13. Mount EFI partition of your system drive
14. Copy `EFI` directory on EFI partition of your system drive

## Laptop Specifications
Acer Aspire A317-51G-78XF
- CPU: Intel Core i7-10510U
- GPU1: Intel UHD Graphics 620
- GPU2: NVIDIA MX250
- RAM: 8 GB
- Ethernet: RTL8111
- Wi-Fi: Qualcomm Atheros QCA9377
- PS/2 Keyboard
- I2C touchpad: Synaptics (SYNA7DB5)
- Audio codec: Realtek ALC255

## Hardware support
🟢 Works
- CPU Power Management
- Graphics acceleration (UHD620)
- Ethernet
- Touchpad
- Audio
- USB
- Battery Indicator
- etc

🟡 Partly 
- HDMI (see fix below)

🔴 Not works
- Wi-Fi (QCA9377)
- MX250
- Brightness keys


## HDMI fix (@Ukr55)
Note. When you apply this changes to config.plist, only HDMI will work (eDP does not work with following changes!). 

Remove all properties from config.plist -> DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)
and add the following in order to enable HDMI:
<table>
  <tr>
    <td><b>Property</b></td>
    <td><b>Type</b></td>
    <td><b>Value</b></td>
  </tr>
  <tr>
    <td>AAPL,ig-platform-id</td>
    <td>Data</td>
    <td>07009B3E</td>
  </tr>
  <tr>
    <td>device-id</td>
    <td>Data</td>
    <td>9B3E0000</td>
  </tr>
  <tr>
    <td>disable-external-gpu</td>
    <td>Data</td>
    <td>01000000</td>
  </tr>
  <tr>
    <td>framebuffer-con0-busid</td>
    <td>Data</td>
    <td>01000000</td>
  </tr>
  <tr>
    <td>framebuffer-con0-enable</td>
    <td>Data</td>
    <td>01000000</td>
  </tr>
  <tr>
    <td>framebuffer-con0-pipe</td>
    <td>Data</td>
    <td>12000000</td>
  </tr>
  <tr>
    <td>framebuffer-con0-type</td>
    <td>Data</td>
    <td>00080000</td>
  </tr>
  <tr>
    <td>framebuffer-con1-busid</td>
    <td>Data</td>
    <td>02000000</td>
  </tr>
  <tr>
    <td>framebuffer-con1-enable</td>
    <td>Data</td>
    <td>01000000</td>
  </tr>
  <tr>
    <td>framebuffer-con1-pipe</td>
    <td>Data</td>
    <td>12000000</td>
  </tr>
  <tr>
    <td>framebuffer-con1-type</td>
    <td>Data</td>
    <td>00080000</td>
  </tr>
  <tr>
    <td>framebuffer-con2-busid</td>
    <td>Data</td>
    <td>00000000</td>
  </tr>
  <tr>
    <td>framebuffer-con2-enable</td>
    <td>Data</td>
    <td>01000000</td>
  </tr>
  <tr>
    <td>framebuffer-con2-index</td>
    <td>Data</td>
    <td>FFFFFFFF</td>
  </tr>
  <tr>
    <td>framebuffer-con2-pipe</td>
    <td>Data</td>
    <td>12000000</td>
  </tr>
  <tr>
    <td>framebuffer-con2-type</td>
    <td>Data</td>
    <td>00080000</td>
  </tr>
  <tr>
    <td>framebuffer-fbmem</td>
    <td>Data</td>
    <td>00009000</td>
  </tr>
  <tr>
    <td>framebuffer-patch-enable</td>
    <td>Data</td>
    <td>01000000</td>
  </tr>
  <tr>
    <td>framebuffer-stolenmem</td>
    <td>Data</td>
    <td>00003001</td>
  </tr>
  <tr>
    <td>hda-gfx</td>
    <td>String</td>
    <td>onboard-1</td>
  </tr>
  <tr>
    <td>model</td>
    <td>String</td>
    <td>Intel UHD Graphics 620</td>
  </tr>
</table>
