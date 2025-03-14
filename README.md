# How to build
**1. Install yocto dependencies**
```
sudo apt install -y chrpath diffstat g++ gawk gcc liblz4-tool make rpcbind
```

**2. Git clone poky and meta-rapsberrypi**
```sh
mkdir sources
cd sources
git clone https://github.com/yoctoproject/poky.git -b scarthgap
git clone https://github.com/agherzan/meta-raspberrypi.git -b scarthgap
cd ..
```

**3. Enter poky**
```sh
source sources/poky/oe-init-build-env build
cd ..
```

**4. Add the raspberry pi layer to bblayers.conf**<br>
`bblayers.conf`
```
# POKY_BBLAYERS_CONF_VERSION is increased each time build/conf/bblayers.conf
# changes incompatibly
POKY_BBLAYERS_CONF_VERSION = "2"

SRCDIR := "${@os.path.abspath(os.path.dirname(d.getVar('FILE', True)) + '/../../sources')}"

BBLAYERS ?= " \
  ${SRCDIR}/poky/meta \
  ${SRCDIR}/poky/meta-poky \
  ${SRCDIR}/poky/meta-yocto-bsp \
  ${SRCDIR}/meta-raspberrypi \
  "
```

**5. Set MACHINE in local.conf to one of the supported boards**<br>
The support boards are listed in `meta-raspberrypi/conf/machine`<br>
`build/conf/local.conf`
```
...
MACHINE ??= "raspberrypi5"
...
```

**6. Build**<br>
**6.1 Trouble shooting before build**
>ERROR: Nothing RPROVIDES 'linux-firmware-rpidistro-bcm43455'

https://meta-raspberrypi.readthedocs.io/en/latest/ipcompliance.html#linux-firmware-rpidistro

**6.2 Build**
```sh
pushd build
bitbake core-image-base
popd
```

# How to flash
**Install bmaptools**
```
sudo apt install bmap-tools
```
**Flash the image**
```sh
pushd build/tmp/deploy/images/raspberrypi5
sudo bmaptool copy core-image-base-raspberrypi5.rootfs-*.wic /dev/sdX
popd
```
