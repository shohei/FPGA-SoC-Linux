### Build Linux Kernel

There are two ways

1. run scripts/build-linux-kernel.sh (easy)
2. run this chapter step-by-step (annoying)

#### Download Linux Kernel Source

##### Clone from linux-stable.git

```
shell$ git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux-4.8.17-armv7-fpga
```

##### Checkout v4.8.17

```
shell$ cd linux-4.8.17-armv7-fpga
shell$ git checkout -b linux-4.8.17-armv7-fpga refs/tags/v4.8.17
```

#### Patch for armv7-fpga

```
shell$ patch -p0 < ../files/linux-4.8.17-armv7-fpga.diff
shell$ git add --update
shell$ git add arch/arm/configs/armv7_fpga_defconfig
shell$ git add arch/arm/boot/dts/zynq-pynqz1.dts
shell$ git commit -m "patch for armv7-fpga"
shell$ git tag -a v4.8.17-armv7-fpga -m "relase v4.8.17-armv7-fpga"
```

#### Setup for Build 

````
shell$ cd linux-4.8.17-armv7-fpga
shell$ export ARCH=arm
shell$ export CROSS_COMPILE=arm-linux-gnueabihf-
shell$ make armv7_fpga_defconfig
````

#### Build Linux Kernel and device tree

````
shell$ make deb-pkg
shell$ make zynq-zybo.dtb
shell$ make zynq-pynqz1.dtb
shell$ make socfpga_cyclone5_de0_sockit.dtb
````

#### Copy zImage and devicetree to target/zybo-zynq/boot/

```
shell$ cp arch/arm/boot/zImage            ../target/zynq-zybo/boot/zImage-4.8.17-armv7-fpga
shell$ cp arch/arm/boot/dts/zynq-zybo.dtb ../target/zynq-zybo/boot/devicetree-4.8.17-zynq-zybo.dtb
shell$ dtc -I dtb -O dts -o ../target/zynq-zybo/boot/devicetree-4.8.17-zynq-zybo.dts arch/arm/boot/dts/zynq-zybo.dtb
```

#### Copy zImage and devicetree to target/zybo-pynqz1/boot/

```
shell$ cp arch/arm/boot/zImage              ../target/zynq-pynqz1/boot/zImage-4.8.17-armv7-fpga
shell$ cp arch/arm/boot/dts/zynq-pynqz1.dtb ../target/zynq-pynqz1/boot/devicetree-4.8.17-zynq-pynqz1.dtb
shell$ dtc -I dtb -O dts -o ../target/zynq-pynqz1/boot/devicetree-4.8.17-zynq-pynqz1.dts arch/arm/boot/dts/zynq-pynqz1.dtb
```

#### Copy zImage and devicetree to target/de0-nano-soc/boot/

```
shell$ cp arch/arm/boot/zImage                              ../target/de0-nano-soc/boot/zImage-4.8.17-armv7-fpga
shell$ cp arch/arm/boot/dts/socfpga_cyclone5_de0_sockit.dtb ../target/de0-nano-soc/boot/devicetree-4.8.17-socfpga.dtb
shell$ dtc -I dtb -O dts -o ../target/de0-nano-soc/boot/devicetree-4.8.17-socfpga.dts arch/arm/boot/dts/socfpga_cyclone5_de0_sockit.dtb
```

