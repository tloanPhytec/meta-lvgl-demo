**meta-lvgl-demo**

This layer provides a recipe for a LVGL demo application.

Tested with:

- PB-03123-001.A1 phyBOARD-Pollux Development Kit
- PHYTEC's BSP-Yocto-NXP-i.MX8MP-PD24.1.0 Linux Board Support Package as a base
- KPB-AV-010-100 LCD Display Add-On w/ Touch (LVGL demo will start on HDMI but touch/mouse interaction won't work as expected unless using touch display.)

**Steps to enable:**

First, setup the default PD24.1.0 BSP from PHYTEC:

```sh
wget https://download.phytec.de/Software/Linux/Yocto/Tools/phyLinux
chmod +x phyLinux
./phyLinux init

# SoC Platform : imx8mp
# Release      : BSP-Yocto-NXP-i.MX8MP-PD24.1.0
# MACHINE      : MACHINE=phyboard-pollux-imx8mp-3 DISTRO=ampliphy-vendor-xwayland

source sources/poky/oe-init-build-env
		
vi conf/local.conf

# Accept End User License Agreement (Uncomment #ACCEPT_FSL_EULA).
 
# Add the following line:
PREFERRED_VERSION_weston:imx-nxp-bsp = "10.0.5.imx"
```

**Note:** Here it is neccessary to downgrade the weston version due to the lvgl-demo having a specific requirement for weston's wl_shell extension, which is deprecated and removed in weston v12 (PD24.1.0's default weston version). The lvgl-demo application may be updated at a later date.
  
Add the meta-lvgl-demo to your BSP

```sh
cd $BUILDDIR/../sources
git clone https://github.com/tloanPhytec/meta-lvgl-demo.git -b scarthgap
cd $BUILDDIR
bitbake-layers add-layer $BUILDDIR/../sources/meta-lvgl-demo
```

Rebuild the image:

```sh
bitbake phytec-lvgldemo-image
```

Boot your PB-03123-001.A1 + KPB-AV-010-100 LCD Display Add-On using the new phytec-lvgldemo-image software image which can be found in *$BUILDDIR/deploy/images/phyboard-pollux-imx8mp-3*

The LVGL demo will start on the touch display automatically upon boot.
