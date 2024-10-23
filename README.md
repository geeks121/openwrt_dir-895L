

**Custom OpenWrt Firmware for D-Link DIR-895L (Hardware Version A3)**

Hi everyone,

Since D-Link has discontinued firmware support for the DIR-895L, I decided to build a custom OpenWrt firmware to extend the functionality of the router. If you're using firmware **DIR895LA1_FW115b02** on hardware version A3, you may have noticed that with DD-WRT, only the 2.4 GHz band and one 5 GHz radio work. To address this, I built an OpenWrt image, based on the D-Link DIR-885L, which has very similar hardware (except for Wi-Fi differences).

### Why I Built This Custom Firmware:
- **DD-WRT limitations:** Only the 2.4 GHz and one 5 GHz radio worked with DD-WRT.
- **OpenWrt flexibility:** OpenWrt offers a more flexible and customizable environment, and by tweaking the firmware, I was able to unlock better support for dual 5 GHz radios and other features.

### Firmware Modifications:
- **Removed:** `brcmfmac-firmware-4366b1-pcie`
- **Added:** `brcmfmac-firmware-4366c0-pcie`
  
This change ensures better Wi-Fi radio support for the DIR-895L.

### Additional Tools and Packages:
- **nano**
- **wget**
- **git**
- **kmod-usb-net-cdc-ncm**
- **kmod-usb-net-huawei-cdc-ncm**
- **kmod-usb-net-rndis**
- **Luci** (OpenWrt web interface) with **SSL** support
- **mwan3** (for load balancing)
- **luci-app-mwan3** (Luci interface for mwan3)

### OpenWrt Version:
- **OpenWrt 23.05.5-bcm53xx**

### What’s Working:
- Full **2.4 GHz** radio support
- Dual **5 GHz** radio support
- **Load balancing** via mwan3
- **USB tethering** support

### What Needs Manual Setup:
- **Wireless radios** need to be manually enabled the first time after installation.
- **LEDs** also need to be configured manually through OpenWrt’s settings.

### What’s Not Working:
- Still testing, but expect similar performance to the DIR-885L (as the custom firmware is based on that model).

### Installation Steps:
The installation process is similar to flashing DD-WRT. You can follow the guide here:  
[DD-WRT Installation Guide](http://forums.dlink.com/index.php?topic=72749.0)

### Compatible Firmware:
Before flashing, you may want to download the latest official firmware:
- **DIR-895L REVA FIRMWARE 1.13b03:**  
  [Legacy D-Link Firmware](https://legacyfiles.us.dlink.com/DIR-895L/REVA/FIRMWARE/)  
  [Australian D-Link Firmware](https://files.dlink.com.au/products/DIR-895L/REV_A/Firmware/)

- **DIR895LA1_FW115b02:**  
  [D-Link Support](https://support.dlink.com.au/Download/download.aspx?product=DIR-895L)

### Download My Custom Firmware:
I’ve built the OpenWrt image using the OpenWrt image builder, and you can download it from here:  
[Download Custom OpenWrt Image (MediaFire)](https://www.mediafire.com/file/9h1qwajtg5epy3u/openwrt-23.05.5.zip/file)

or you can download using repo

### Create Your Own Custom OpenWrt Firmware:
For those who prefer to create their own custom firmware, here’s a simple guide using Linux:

1. **Download the OpenWrt Image Builder:**  
   [Image Builder for OpenWrt 23.05.5 (bcm53xx)](https://mirror-03.infra.openwrt.org/releases/23.05.5/targets/bcm53xx/generic/)
   
   Extract the tar.xz file and then run the following command to build the custom firmware:
   
   ```
   make -j4 image PROFILE=dlink_dir-885l \
   PACKAGES="-brcmfmac-firmware-4366b1-pcie brcmfmac-firmware-4366c0-pcie kmod-usb-net-cdc-ncm kmod-usb-net-huawei-cdc-ncm nano wget git luci luci-ssl mwan3 luci-app-mwan3"
   ```

   **For a minimalist build:**
   ```
   make image PROFILE=dlink_dir-885l \
   PACKAGES="-brcmfmac-firmware-4366b1-pcie brcmfmac-firmware-4366c0-pcie luci luci-ssl"
   ```

   The minus sign `-` before a package means removing it. I included **Luci** because the default image builder doesn’t include a web interface.

2. **Google Colab Build Method:**  
   If you don’t have Linux set up locally, you can use Google Colab to build your own custom OpenWrt firmware.  
   Here’s a Google Colab notebook to help you out:  
   [Custom OpenWrt Build on Google Colab](https://colab.research.google.com/drive/18ReVMggGn2FMd04QfLoQhIDNHFm3lueZ?usp=sharing)

---
