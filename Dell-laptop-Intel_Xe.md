# 1. Intel Xe GPU drivers & Vulkan
```
sudo pacman -S --needed intel-ucode vulkan-intel intel-compute-runtime intel-gmmlib intel-gpu-tools intel-graphics-compiler intel-media-driver intel-media-sdk libmfx libva-utils lib32-vulkan-intel glu libva-mesa-driver mesa mesa-demos mesa-utils mesa-vdpau vulkan-mesa-layers lib32-glu lib32-libva-mesa-driver lib32-mesa lib32-mesa-demos lib32-mesa-utils lib32-mesa-vdpau lib32-vulkan-mesa-layers vulkan-icd-loader vulkan-extra-layers vulkan-extra-tools vulkan-headers thermald
```
## 1.1 Enable thermald
```
sudo systemctl enable thermald
```
```
sudo systemctl start thermald
```
# 2. Wine and dependencies, goddies for Steam + Proton.
```
sudo pacman -S --needed wine wine-gecko wine-mono giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses ocl-icd lib32-ocl-icd libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader
```
# 3. i915 parametrs
```
sudo nano /etc/modprobe.d/i915.conf
```
```                                           
options i915 enable_guc=3
options i915 enable_fbc=1
options i915 fastboot=1
```

# 4. Udev rules
```
sudo nano /etc/udev/rules.d/60-ioschedulers.rules 
```
```
# set scheduler for NVMe
ACTION=="add|change", KERNEL=="nvme[0-9]n[0-9]", ATTR{queue/scheduler}="none"
# set scheduler for SSD and eMMC
ACTION=="add|change", KERNEL=="sd[a-z]*|mmcblk[0-9]*", ATTR{queue/rotational}=="0", ATTR{queue/scheduler}="mq-deadline"
# set scheduler for rotating disks
ACTION=="add|change", KERNEL=="sd[a-z]*", ATTR{queue/rotational}=="1", ATTR{queue/scheduler}="bfq"
```

# 5. Sysctl tweaks - performance and security
```
sudo nano /etc/sysctl.d/99-cachyos-settings.conf
```
```
# The swappiness sysctl parameter represents the kernel's preference (or avoidance) of swap space. Swappiness can have a value between 0 and 100, the default value is 60. 
# A low value causes the kernel to avoid swapping, a higher value causes the kernel to try to use swap space. Using a low value on sufficient memory is known to improve responsiveness on many systems.
vm.swappiness = 1

# The value controls the tendency of the kernel to reclaim the memory which is used for caching of directory and inode objects (VFS cache). 
# Lowering it from the default value of 100 makes the kernel less inclined to reclaim VFS cache (do not set it to 0, this may produce out-of-memory conditions)
#vm.vfs_cache_pressure=50

# Contains, as a percentage of total available memory that contains free pages and reclaimable
# pages, the number of pages at which a process which is generating disk writes will itself start
# writing out dirty data (Default is 20).
vm.dirty_ratio = 5

# page-cluster controls the number of pages up to which consecutive pages are read in from swap in a single attempt. 
# This is the swap counterpart to page cache readahead. The mentioned consecutivity is not in terms of virtual/physical addresses, 
# but consecutive on swap space - that means they were swapped out together. (Default is 3)
# increase this value to 1 or 2 if you are using physical swap (1 if ssd, 2 if hdd)
vm.page-cluster = 0

# Contains, as a percentage of total available memory that contains free pages and reclaimable
# pages, the number of pages at which the background kernel flusher threads will start writing out
# dirty data (Default is 10).
vm.dirty_background_ratio = 5

# This tunable is used to define when dirty data is old enough to be eligible for writeout by the
# kernel flusher threads.  It is expressed in 100'ths of a second.  Data which has been dirty
# in-memory for longer than this interval will be written out next time a flusher thread wakes up
# (Default is 3000).
#vm.dirty_expire_centisecs = 3000

# The kernel flusher threads will periodically wake up and write old data out to disk.  This
# tunable expresses the interval between those wakeups, in 100'ths of a second (Default is 500).
vm.dirty_writeback_centisecs = 1500

# This action will speed up your boot and shutdown, because one less module is loaded. Additionally disabling watchdog timers increases performance and lowers power consumption
# Disable NMI watchdog
kernel.nmi_watchdog = 0

# Enable the sysctl setting kernel.unprivileged_userns_clone to allow normal users to run unprivileged containers.
kernel.unprivileged_userns_clone = 1

# To hide any kernel messages from the console
kernel.printk = 3 3 3 3

# Restricting access to kernel logs
kernel.dmesg_restrict = 1

# Restricting access to kernel pointers in the proc filesystem
kernel.kptr_restrict = 2

# Disable Kexec, which allows replacing the current running kernel. 
kernel.kexec_load_disabled = 1

# Increasing the size of the receive queue.
# The received frames will be stored in this queue after taking them from the ring buffer on the network card.
# Increasing this value for high speed cards may help prevent losing packets: 
net.core.netdev_max_backlog = 16384

# Increase the maximum connections
#The upper limit on how many connections the kernel will accept (default 128): 
net.core.somaxconn = 8192

# Increase the memory dedicated to the network interfaces
# The default the Linux network stack is not configured for high speed large file transfer across WAN links (i.e. handle more network packets) and setting the correct values may save memory resources: 
net.core.rmem_default = 1048576
net.core.rmem_max = 16777216
net.core.wmem_default = 1048576
net.core.wmem_max = 16777216
net.core.optmem_max = 65536
net.ipv4.tcp_rmem = 4096 1048576 2097152
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.udp_rmem_min = 8192
net.ipv4.udp_wmem_min = 8192

# Enable TCP Fast Open
# TCP Fast Open is an extension to the transmission control protocol (TCP) that helps reduce network latency
# by enabling data to be exchanged during the sender’s initial TCP SYN [3]. 
# Using the value 3 instead of the default 1 allows TCP Fast Open for both incoming and outgoing connections: 
net.ipv4.tcp_fastopen = 3

# Enable BBR
# The BBR congestion control algorithm can help achieve higher bandwidths and lower latencies for internet traffic
net.core.default_qdisc = cake
net.ipv4.tcp_congestion_control = bbr2

# TCP SYN cookie protection
# Helps protect against SYN flood attacks. Only kicks in when net.ipv4.tcp_max_syn_backlog is reached: 
net.ipv4.tcp_syncookies = 1

# Protect against tcp time-wait assassination hazards, drop RST packets for sockets in the time-wait state. Not widely supported outside of Linux, but conforms to RFC: 
net.ipv4.tcp_rfc1337 = 1

# By enabling reverse path filtering, the kernel will do source validation of the packets received from all the interfaces on the machine. This can protect from attackers that are using IP spoofing methods to do harm. 
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.all.rp_filter = 1

# Disable ICMP redirects
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

fs.inotify.max_user_watches = 524288
fs.file-max = 2097152

# Fix Mesa Intel performance support
dev.i915.perf_stream_paranoid = 0

# increase writeback interval  for xfs
fs.xfs.xfssyncd_centisecs = 10000

# disable core dumps
kernel.core_pattern = /dev/null
```
# 6. Esync 
```
sudo nano /etc/security/limits.d/99-esync.conf
```
```
* hard nofile 2097152
```
