# rtl8192eu drivers

**NOTE:** 

This branch is based on Mange's (https://github.com/Mange/rtl8192eu-linux-driver)
I have only modified it to ensure it installs correctly with my kernel version, as it wasn't installing on Debian Linux kernel 5.16.17.

Linux 5.16.17 #2.3.3 SMP Sun June 09 09:15:19 CST 2024 aarch64 GNU/Linux


**NOTE:** 

I agree with Mange's as below comment. Please do not ask for any advice. I have only modified it to ensure it installs correctly with the kernel version.

This is just a "mirror". I have no knowledge about this code or how it works. Expect no support from me or any contributors here. I just think GitHub is a nicer way of keeping track of this than random forum posts and precompiled binaries being sent by email. I don't want someone else to have to spend 5 days of googling and compiling with random patches until it works.

## Install

1. Clone this repository and change your directory to cloned path.

    ```shell
    git clone https://github.com/pure100kim/rtl8192eu-driver
    ```
    ```shell
    cd rtl8192eu-driver
    ```

2. make & make install
    ```sh
    make && sudo make install
    ```
    
3. Distributions based on Debian & Ubuntu have RTL8XXXU driver present & running in kernelspace. To use our RTL8192EU driver, we need to blacklist RTL8XXXU.

    ```shell
    echo "blacklist rtl8xxxu" | sudo tee /etc/modprobe.d/rtl8xxxu.conf
    ```

4. Force RTL8192EU Driver to be active from boot.
    ```shell
    echo -e "8192eu\n\nloop" | sudo tee /etc/modules
    ```

5. Newer versions of Ubuntu has weird plugging/replugging issue (Check #94). This includes weird idling issues, To fix this:

    ```shell
    echo "options 8192eu rtw_power_mgnt=0 rtw_enusbss=0" | sudo tee /etc/modprobe.d/8192eu.conf
    ```

6. Update changes to Grub & initramfs

    ```shell
    sudo update-grub; sudo update-initramfs -u
    ```

7. Reboot system to load new changes from newly generated initramfs.

    ```shell
    systemctl reboot -i
    ```

8. Check that your kernel has loaded the right module:
 
    ```shell
    sudo lshw -c network
    ```
   
You should see the line ```driver=8192eu```



network:1 
       description: Wireless interface
       
       physical id: a
       
       bus info: usb@3:1.1
       
       logical name: wlxa047d721927c
       
       serial: a0:47:d7:21:92:7c
       
       capabilities: ethernet physical wireless
       
       configuration: broadcast=yes driver=rtl8192eu driverversion=5.16.17 ip=xxx.xxx.x.xx multicast=yes wireless=IEEE 802.11




## Installing progesss message ##

   root@prue100kim:/home/pure100kim/rtl8192eu-linux-driver# make && sudo make install

   make ARCH=arm64 CROSS_COMPILE= -C /lib/modules/5.16.17-sun50iw9/build M=/home/pure100kim/rtl8192eu-linux-driver  modules

  make[1]: Entering directory '/usr/src/linux-headers-5.16.17-sun50iw9'

  CC [M]  /home/pure100kim/rtl8192eu-linux-driver/core/rtw_cmd.o

  CC [M]  /home/pure100kim/rtl8192eu-linux-driver/core/rtw_security.o

.

.

.

  MODPOST /home/pure100kim/rtl8192eu-linux-driver/Module.symvers

  CC [M]  /home/pure100kim/rtl8192eu-linux-driver/8192eu.mod.o

  LD [M]  /home/pure100kim/rtl8192eu-linux-driver/8192eu.ko

  make[1]: Leaving directory '/usr/src/linux-headers-5.16.17'

  install -p -m 644 8192eu.ko  /lib/modules/5.16.17/kernel/drivers/net/wireless/

  /sbin/depmod -a 5.16.17-sun50iw9

  root@pure100kim:/home/pure100kim/rtl8192eu-linux-driver#




## Using as AP

My usb dongle product photo bought Daiso

https://github.com/pure100kim/rtl8192eu-driver/blob/main/rtl8192eu.jpg






## Submitting patches
I have mentioned the branch above under the following conditions.

1. Fork repo
2. Do your patch in a topic branch
3. Open a pull request on GH, or send it by email to `Magnus Bergmark <magnus.bergmark@gmail.com>`.
4. I'll squash your commits when everything checks out and add it to `master`.



## Copyright and licenses

The original code is copyrighted, but I don't know by whom. The driver download does not contain license information; please open an issue if you are the copyright holder.

Most C files are licensed under GNU General Public License (GPL), version 2.

[driver-downloads]: http://support.dlink.com.au/Download/download.aspx?product=DWA-131
[direct-download]: ftp://files.dlink.com.au/products/DWA-131/REV_E/Drivers/DWA-131_Linux_driver_v4.3.1.1.zip
[initial-commit]: https://github.com/Mange/rtl8192eu-linux-driver/commit/1387cf623d54bc2caec533e72ee18ef3b6a1db29
