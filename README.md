# arm64 update U-Boot on device (Rock64)

#### Method A
#### 1.)	Install Debian package for Rockchip U-Boot

	sudo apt install u-boot-rockchip

#### 2.)	Flashing U-Boot to our Pine64 Rock64 SBC

	lsblk		(remember storage device name --> mmcblk0 for instance)

	sudo u-boot-install-rockchip /dev/mmcblkX

#### 3.)	Reboot the machine

	sudo reboot
 
#### Method B
#### 1.)	Install Compiler for building U-Boot on our Pine64 Rock64 SBC

	sudo apt install device-tree-compiler build-essential libssl-dev python3-dev bison
	sudo apt install flex libssl-dev swig gcc-aarch64-linux-gnu gcc-arm-none-eabi
	sudo apt install gcc make git python3-setuptools

#### 2.)	Build U-Boot on our Pine64 Rock64 SBC

	cd /home
	git clone https://github.com/ARM-software/arm-trusted-firmware
	cd arm-trusted-firmware
	git tag           (remember last stable --> lts-v2.12.1 for instance)
	git checkout lts-v2.12.1
	make PLAT=rk3328 bl31
  
	cd ..
  
	git clone git://git.denx.de/u-boot.git
	cd u-boot
	git tag           (remember last stable --> v2025.01 for instance)
	git checkout v2025.01
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

---------------------------------------------------------------------------------------------------

# arm64 update U-Boot on device (H64B)

#### 1.)	Install Compiler for building U-Boot on our Pine64 H64B SBC

	sudo apt install device-tree-compiler build-essential libssl-dev python3-dev bison
 	sudo apt install flex libssl-dev swig gcc-aarch64-linux-gnu gcc-arm-none-eabi
  	sudo apt install gcc make git python3-setuptools

#### 2.)	Build U-Boot on our Pine64 H64B SBC

	cd /home
 	git clone https://github.com/ARM-software/arm-trusted-firmware
  	cd arm-trusted-firmware
   	git tag           (remember last stable --> lts-v2.12.1 for instance)
	git checkout lts-v2.12.1
	make PLAT=sun50i_h6 bl31

	cd ..
  
	git clone git://git.denx.de/u-boot.git
	cd u-boot
	git tag           (remember last stable --> v2025.01 for instance)
	git checkout v2025.01
	ln -s /home/youruser/arm-trusted-firmware/build/sun50i_h6/release/bl31/bl31.bin bl31.bin
	make BL31=bl31.bin pine_h64_defconfig
	make -j2 BL31=bl31.bin
  
	cd ..

	cp -r /home/youruser/u-boot/u-boot-sunxi-with-spl.bin /home/youruser/

#### 3.)	Flashing U-Boot to our Pine64 H64B SBC


	cd /home/youruser/
  
	lsblk             (remember storage device name --> mmcblk0 for instance)

	sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/mmcblkX bs=1024 seek=8 conv=notrunc

#### 4.)	Reboot the machine

	sudo reboot

 Note: Ignore the warning below, the just build u-boot image is still functional and will work.

 	Image 'u-boot-sunxi-with-spl' has faked external blobs and is non-functional: scp.bin
	
	Image 'u-boot-sunxi-with-spl' is missing optional external blobs but is still functional: scp
	
	/binman/u-boot-sunxi-with-spl/fit/images/scp/scp (scp.bin):
   	SCP firmware is required for system suspend, but is otherwise optional.
   	Please read the section on SCP firmware in board/sunxi/README.sunxi64
	
	Some images are invalid
