
# System Optimization for Ubuntu 24.04 LTS

## Overview
This document outlines a series of optimizations applied to enhance the performance and responsiveness of a system running Ubuntu 24.04 LTS. Each optimization is tailored to the specific configuration of the system, aiming to improve overall user experience, especially in a media-heavy workload environment.

## System Configuration
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

## Optimizations Applied

### 1. Swappiness Adjustment
- **Objective:** 
  - Reduce the system's reliance on swap space to improve performance by keeping more processes in RAM. This is particularly beneficial for systems with ample memory, like 16GB, where swapping can slow down performance.
- **Steps:**
  1. Open the sysctl configuration file:
     ```bash
     sudo vim /etc/sysctl.conf
     ```
  2. Add or modify the following line to set swappiness to 10:
     ```plaintext
     vm.swappiness=10
     ```
  3. Apply the changes:
     ```bash
     sudo sysctl -p
     ```

### 2. VFS Cache Pressure Adjustment
- **Objective:** 
  - Improve filesystem performance by lowering the rate at which the system reclaims memory used for caching directory and inode objects. This helps in retaining more cached data in RAM, making access faster, especially with frequent file operations.
- **Steps:**
  1. Open the sysctl configuration file:
     ```bash
     sudo vim /etc/sysctl.conf
     ```
  2. Add or modify the following line to set VFS cache pressure to 50:
     ```plaintext
     vm.vfs_cache_pressure=50
     ```
  3. Apply the changes:
     ```bash
     sudo sysctl -p
     ```

### 3. Process Scheduler Tuning
- **Objective:** 
  - Enhance system responsiveness by adjusting process scheduler parameters. These adjustments aim to reduce latency and improve the smoothness of multitasking, particularly under load.
- **Steps:**
  1. Edit the GRUB configuration file to add kernel scheduler parameters:
     ```bash
     sudo vim /etc/default/grub
     ```
  2. Modify the `GRUB_CMDLINE_LINUX_DEFAULT` line:
     ```plaintext
     GRUB_CMDLINE_LINUX_DEFAULT="quiet splash kernel.sched_latency_ns=6000000 kernel.sched_min_granularity_ns=750000 kernel.sched_wakeup_granularity_ns=1000000"
     ```
  3. Update GRUB and reboot to apply changes:
     ```bash
     sudo update-grub
     sudo reboot
     ```

### 4. I/O Scheduler Adjustment
- **Objective:** 
  - Optimize disk I/O performance by selecting an appropriate I/O scheduler based on the current hardware configuration. For HDDs, the "deadline" scheduler is recommended for minimizing latency, while the "noop" scheduler is better suited for SSDs.
- **Steps:**
  1. Edit the GRUB configuration file:
     ```bash
     sudo vim /etc/default/grub
     ```
  2. Modify the `GRUB_CMDLINE_LINUX_DEFAULT` line:
     ```plaintext
     GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=deadline"
     ```
  3. Update GRUB and reboot to apply changes:
     ```bash
     sudo update-grub
     sudo reboot
     ```

### 5. Wi-Fi Optimization
- **Objective:** 
  - Enhance Wi-Fi performance by disabling power-saving features and adjusting other settings to prioritize connectivity and throughput. Given that the laptop is always plugged in, power-saving features are less relevant, so we focus on maximizing performance.
- **Steps:**
  1. Disable power-saving for the Wi-Fi card:
     ```bash
     sudo vim /etc/modprobe.d/iwlwifi.conf
     ```
  2. Add the following line:
     ```plaintext
     options iwlwifi power_save=0
     ```
  3. (Optional) Disable U-APSD to reduce latency:
     ```bash
     sudo vim /etc/modprobe.d/iwlwifi.conf
     ```
     Add the following line:
     ```plaintext
     options iwlwifi uapsd_disable=1
     ```
  4. Apply the changes by updating the initramfs and rebooting:
     ```bash
     sudo update-initramfs -u
     sudo reboot
     ```

## Future Updates
This document will be continuously updated with any new optimizations or configurations applied to further enhance system performance. Keep an eye on this space for the latest improvements and tweaks.
