# arm64 update U-Boot on device (Rock64)

#### 1.)	Install Compiler for building U-Boot on our Pine64 Rock64 SBC

	sudo apt install device-tree-compiler build-essential libssl-dev python3-dev bison
	sudo apt install flex libssl-dev swig gcc-aarch64-linux-gnu gcc-arm-none-eabi
	sudo apt install gcc make git

#### 2.)	Build U-Boot on our Pine64 Rock64 SBC

	cd /home
	git clone https://github.com/ARM-software/arm-trusted-firmware
	cd arm-trusted-firmware
	git tag           (remember last stable --> v2.5 for instance)
	git checkout v2.5
	make PLAT=rk3328 bl31
  
	cd ..
  
	git clone git://git.denx.de/u-boot.git
	cd u-boot
	git tag           (remember last stable --> v2021.07 for instance)
	git checkout v2021.07
	ln -s /home/youruser/arm-trusted-firmware/build/rk3328/release/bl31/bl31.elf bl31.elf
	make BL31=bl31.elf rock64-rk3328_defconfig
	make -j2 BL31=bl31.elf all u-boot.itb
  
	cd ..
  
	cp -r /home/youruser/u-boot/idbloader.img /home/youruser/
	cp -r /home/youruser/u-boot/u-boot.itb /home/youruser/

#### 3.)	Flashing U-Boot to our Pine64 Rock64 SBC

	cd /home/youruser/
  
	lsblk             (remember storage device name --> mmcblk1 for instance)
  
	sudo dd if=idbloader.img of=/dev/mmcblkX seek=64 conv=notrunc
	sudo dd if=u-boot.itb of=/dev/mmcblkX seek=16384 conv=notrunc

#### 4.)	Reboot the machine

	sudo reboot


# arm64 update U-Boot on device (H64B)

#### 1.)	Install Compiler for building U-Boot on our Pine64 H64B SBC

	sudo apt install device-tree-compiler build-essential libssl-dev python3-dev bison
 	sudo apt install flex libssl-dev swig gcc-aarch64-linux-gnu gcc-arm-none-eabi
  	sudo apt install gcc make git

#### 2.)	Build U-Boot on our Pine64 H64B SBC

	cd /home
 	git clone https://github.com/ARM-software/arm-trusted-firmware
  	cd arm-trusted-firmware
   	git tag           (remember last stable --> v2.5 for instance)
    	git checkout v2.5
     	make PLAT=sun50i_h6 bl31

	cd ..
  
	git clone git://git.denx.de/u-boot.git
	cd u-boot
	git tag           (remember last stable --> v2021.07 for instance)
	git checkout v2021.07
	ln -s /home/youruser/arm-trusted-firmware/build/sun50i_h6/release/bl31/bl31.bin bl31.bin
	make BL31=bl31.bin pine_h64_defconfig
	make -j2 BL31=bl31.bin
  
	cd ..

	cp -r /home/youruser/u-boot/u-boot-sunxi-with-spl.bin /home/youruser/

#### 3.)	Flashing U-Boot to our Pine64 H64B SBC


	cd /home/youruser/
  
	lsblk             (remember storage device name --> mmcblk1 for instance)

	sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/mmcblkX bs=1024 seek=8 conv=notrunc

#### 4.)	Reboot the machine

	sudo reboot
