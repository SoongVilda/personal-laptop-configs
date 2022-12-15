# Guide for adjusting PKGBUILD
We need following package
```
sudo pacman -S pacman-contrib
```

## Let's start with simple PKGBUILD
Everybody on the Arch Linux already know [paru](https://aur.archlinux.org/packages/paru) or [yay](https://aur.archlinux.org/packages/yay), so this is a great to describe. You can take a look on full paru [PKGBUILD](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=paru).

### Upper part of PKGBUILD
This is an interesting part of `PKGBUILD` for us right now.
1. We need to take a look on `pkgver=` and `source=`
2. You can change number of version in `pkgver=`. For example: If one day will be released paru version 1.11.2, 1.11.5 or 1.28.0, you simply need change `pkgver=1.11.1` to `pkgver=1.28.0`
3. After changes use command `updpkgsums`
4. Last command `makepkg -si`
5. Wait until your package is done. It can takes a long time, that depends on performance your computer, mostly depend on your **CPU**.
#### PKGBUILD without changes
```
# Maintainer: Morgan <morganamilo@archlinux.org>
pkgname=paru
pkgver=1.11.1
pkgrel=1
pkgdesc='Feature packed AUR helper'
url='https://github.com/morganamilo/paru'
source=("$pkgname-$pkgver.tar.gz::https://github.com/Morganamilo/paru/archive/v$pkgver.tar.gz")
backup=("etc/paru.conf")
arch=('i686' 'pentium4' 'x86_64' 'arm' 'armv7h' 'armv6h' 'aarch64')
license=('GPL3')
makedepends=('cargo')
depends=('git' 'pacman')
optdepends=('asp: downloading repo pkgbuilds' 'bat: colored pkgbuild printing' 'devtools: build in chroot')
sha256sums=('42cefa8cdf48e3aec3f9922235f1e1126a9fe3262f1644494e1be51980e520d8')
```

#### PKGBUILD with changed version 
```
# Maintainer: Morgan <morganamilo@archlinux.org>
pkgname=paru
pkgver=1.28.0
pkgrel=1
pkgdesc='Feature packed AUR helper'
url='https://github.com/morganamilo/paru'
source=("$pkgname-$pkgver.tar.gz::https://github.com/Morganamilo/paru/archive/v$pkgver.tar.gz")
backup=("etc/paru.conf")
arch=('i686' 'pentium4' 'x86_64' 'arm' 'armv7h' 'armv6h' 'aarch64')
license=('GPL3')
makedepends=('cargo')
depends=('git' 'pacman')
optdepends=('asp: downloading repo pkgbuilds' 'bat: colored pkgbuild printing' 'devtools: build in chroot')
sha256sums=('42cefa8cdf48e3aec3f9922235f1e1126a9fe3262f1644494e1be51980e520d8')
```
