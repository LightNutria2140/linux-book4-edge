## About

Linux & DTS to allow booting on the X1E80100 Samsung Galaxy Book4 Edge, derived from [vamanea/linux-magicbook](https://github.com/vamanea/linux-magicbook), as these devices seem to share some similarities. Essentials working, but some things remain untested.

Thread: https://bugs.launchpad.net/ubuntu-concept/+bug/2084591

(Note: This is the 16" tmp branch. 14" dts can be found [here](https://github.com/zensanp/linux-book4-edge/tree/x1e80100-book4e-7.2-14-tmp))

(Note: The dts for the 16" is decompiled from the one I am currently using, meaning it is hard to read since it is only hex.)

(Note: Most links point to zensanp's 14" repo. The features ticked below have been tested on the 16", except where listed under Known issues.)

## Status
Supported features:
- [x] Keyboard - needs udev [quirk](https://bugs.launchpad.net/ubuntu-concept/+bug/2084591/comments/87)
   - [x] Keyboard backlight - via OpenBSD's [samsabi](https://cvsweb.openbsd.org/log/src/sys/dev/i2c/samsabi.c,v?sort=File) samsung-emuec if not using the module
- [x] Touchpad
- [x] USB type-c
  - [x] USB hotplug - see Known issues for charging
  - [x] DP Altmode
- [x] UFS storage
- [x] HDMI port on the right side
- [x] Built-in display - initially fixed using the following [patch](https://bugs.launchpad.net/ubuntu-concept/+bug/2084591/comments/99)
- [x] GPU - needs firmware + updated Mesa (tested on 25.3.3). [Enable](https://github.com/zensanp/linux-book4-edge/blob/49323f22adcc3d54c47c985622bdab90f1663aab/arch/arm64/boot/dts/qcom/x1e80100-samsung-galaxy-book4-edge.dts#L839) only after updating, or black screen after boot.
- [x] Touchscreen - needs included [workaround](https://github.com/zensanp/linux-book4-edge/issues/7#issuecomment-5517366291)
- [x] Wifi - needs firmware. 14" version also needs a patched board-2.bin as discussed [here](https://github.com/zensanp/linux-book4-edge/issues/3)
- [x] BT - needs firmware + MAC address must be set [manually](https://github.com/zensanp/linux-book4-edge/issues/5) for device.
- [x] ADSP and CDSP - need firmware
  - [x] Battery indicator - needs separate [driver](https://github.com/zensanp/linux-book4-edge/issues/4) if not using samsung-emuec module
- [ ] Audio - speakers are disabled in the 16" dtb on purpose (no HW protection. Proceed at own risk.)
  - [ ] Mic
- [x] Sleep - Does it work? [Yes]. Does it save power? [Not really].
- [x] Webcam sensor detected (experimental dtb only) - Adapted from the [15.6" X1P42100 Book4 Edge (X1 Plus)](https://github.com/ciscobugger/book4-edge-linux). Needs libcamera patch
  - [ ] Webcam shows a picture (it's a separate experimental dtb, which can be found in the experiments folder)

## How to use
Boot a kernel built from the 14" branch together with the 16" dtb `x1e80100-samsung-galaxy-book4-edge-16.dtb`, since there is no 16" kernel yet. It is built from `arch/arm64/boot/dts/qcom/x1e80100-samsung-galaxy-book4-edge-16.dts`, which is decompiled from the dtb I use (see the notes above). It was tested to recompile with `dtc`; it has not been built through the kernel's own `make`.

## Known issues
- Charging: if no charger was plugged in at boot, connecting one later delivers almost no power (about 0.1 W in my case, even with the official 60 W Samsung charger), and the battery can run flat during use. After a charger has been unplugged, only the official charger charged it again in my tests (a 30 W charger negotiates 15 V / 2 A). The causes are not confirmed and this needs more testing.
- Camera: the sensor is detected, but no picture is shown yet.

## Credits
- zensanp (the 14" tree this is based on)
- vamanea/linux-magicbook
- ciscobugger (camera nodes)
- Anatase (device tree base)
- OpenBSD's samsabi
- Claude (Anthropic) was used as an assistant for troubleshooting and documentation
