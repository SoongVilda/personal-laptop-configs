# 1. cachyos x86-64-v3 repositories
```
wget https://mirror.cachyos.org/cachyos-repo.tar.xz
tar xvf cachyos-repo.tar.xz
cd cachyos-repo
sudo ./cachyos-repo.sh
```
## 1.1 performance tweaks
```
sudo pacman -S --needed cachyos-settings linux-cachyos linux-cachyos-headers
```

# 2. Intel Xe GPU drivers & Vulkan
```
sudo pacman -S --needed intel-ucode vulkan-intel intel-compute-runtime intel-gmmlib intel-gpu-tools intel-graphics-compiler intel-media-driver intel-media-sdk libmfx libva-utils lib32-vulkan-intel glu libva-mesa-driver mesa mesa-demos mesa-utils mesa-vdpau vulkan-mesa-layers lib32-glu lib32-libva-mesa-driver lib32-mesa lib32-mesa-demos lib32-mesa-utils lib32-mesa-vdpau lib32-vulkan-mesa-layers vulkan-icd-loader vulkan-extra-layers vulkan-extra-tools vulkan-headers thermald
```
## 2.1 Enable thermald
```
sudo systemctl enable thermald
```
```
sudo systemctl start thermald
```
# 3. Wine and dependencies, goddies for Steam + Proton.
```
sudo pacman -S --needed wine wine-gecko wine-mono wine-nine giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses ocl-icd lib32-ocl-icd libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader
```

# 4. KDE software
I usually install everything.

## 4.1 KDE basic
```
sudo pacman -S --needed plasma kde-graphics kde-multimedia kde-utilities 
```

### 4.4.1 Kate
```
sudo pacman -S --needed kate
```

# 5 Intel GPU
[Enable GuC / HuC firmware loading](https://wiki.archlinux.org/title/intel_graphics#Enable_GuC_/_HuC_firmware_loading)
```
sudo nano /etc/modprobe.d/i915.conf
```
Older Intel CPUs should use `options i915 enable_guc=2`
```
options i915 enable_guc=3
options i915 enable_fbc=1
options i915 fastboot=1
```
