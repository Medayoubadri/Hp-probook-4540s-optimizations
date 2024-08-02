
# Intel HD Graphics 4000 Optimization on Ubuntu 24.04 LTS

## Laptop Configuration
- **Model:** Hewlett-Packard HP ProBook 4540s
- **Memory:** 16.0 GiB
- **Processor:** Intel® Core™ i7-3840QM × 8
- **Graphics:** Intel® HD Graphics 4000 (IVB GT2)
- **Disk Capacity:** 1.2 TB HDD (with plans to switch to SSD)
- **Operating System:** Ubuntu 24.04 LTS
- **Kernel Version:** Linux 6.8.0-39-generic
- **Windowing System:** X11
- **GNOME Version:** 46
- **Wi-Fi Card:** Intel Corporation Dual Band Wireless-AC 7260
- **Graphics Driver in Use:** i915

## Objective
The objective is to optimize the performance of the Intel HD Graphics 4000 on Ubuntu 24.04 LTS, enhancing system responsiveness, graphics performance, and overall smoothness, particularly for media-heavy tasks.

## Optimization Steps

### 1. Enable Intel SNA (Sandybridge's New Acceleration)
- **Objective:** This step is aimed at improving 2D rendering performance, which is particularly beneficial for general desktop usage and light graphics workloads.
- **Steps:**
  1. Open a terminal and create a new Xorg configuration file:
     ```bash
     sudo vim /etc/X11/xorg.conf.d/20-intel.conf
     ```
  2. Add the following content to enable SNA acceleration and tear-free rendering:
     ```plaintext
     Section "Device"
         Identifier "Intel Graphics"
         Driver "intel"
         Option "AccelMethod" "sna"
         Option "TearFree" "true"
     EndSection
     ```
  3. Save the file and reboot your system to apply the changes:
     ```bash
     sudo reboot
     ```

### 2. Adjust Power Management Settings
- **Objective:** These settings are aimed at optimizing power management features of the i915 driver, enhancing both performance and battery life by enabling deeper power-saving states and frame buffer compression.
- **Steps:**
  1. Open the i915 configuration file:
     ```bash
     sudo vim /etc/modprobe.d/i915.conf
     ```
  2. Add the following lines to enable the desired power management features:
     ```plaintext
     options i915 enable_rc6=1
     options i915 enable_fbc=1
     options i915 enable_psr=1
     options i915 enable_dc=2
     options i915 enable_sagv=1
     options i915 enable_ips=1
     ```
  3. Update the initramfs and reboot your system to apply the changes:
     ```bash
     sudo update-initramfs -u
     sudo reboot
     ```

### 3. Enable GuC/HuC Firmware
- **Objective:** Enabling GuC and HuC firmware optimizes GPU command submission and video processing performance, which is particularly beneficial for media-heavy applications and workloads.
- **Steps:**
  1. Edit the i915 configuration file to enable GuC and HuC:
     ```bash
     sudo vim /etc/modprobe.d/i915.conf
     ```
  2. Add the following line to the configuration:
     ```plaintext
     options i915 enable_guc=3
     ```
  3. Update the initramfs and reboot your system:
     ```bash
     sudo update-initramfs -u
     sudo reboot
     ```

## Future Updates
This document will be continuously updated with any new optimizations or configurations applied to the system. Keep an eye on this space for the latest improvements and tweaks.


