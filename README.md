# rtl8192eu drivers 

**NOTE:** This branch is based on Mange's (https://github.com/Mange/rtl8192eu-linux-driver)

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
    ...
    make && sudo make install
    ...
    

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

  *-network:1
       description: Wireless interface
       physical id: a
       bus info: usb@3:1.1
       logical name: wlxa047d721927c
       serial: a0:47:d7:21:92:7c
       capabilities: ethernet physical wireless
       configuration: broadcast=yes driver=rtl8192eu driverversion=5.16.17-......ip=xxx.xxx.x.xx multicast=yes wireless=IEEE 802.11



## Using as AP

Reference: Intelbras IWA 3001 USB WiFi Adapter  
Devices using the 8192eu chip can serve as decent access points, with speeds up to ~50Mbps.  
 
Using hostapd to manage your AP, set the proper ht-capab field for this device, which is:  

`HT_CAPAB=[RX-STBC1][SHORT-GI-40][SHORT-GI-20][DSSS_CCK-40][MAX-AMSDU-7935]`

Optionally enable wideband, if you don't have neighbours:  
Note that while this will result in a increase in network throughput it may cause clients further away to fail connecting.  
It may also make the device work better with repeaters repeating its signal.  

`HT_CAPAB=[HT40+][RX-STBC1][SHORT-GI-40][SHORT-GI-20][DSSS_CCK-40][MAX-AMSDU-7935]` (for channels 1-7), or  
`HT_CAPAB=[HT40-][RX-STBC1][SHORT-GI-40][SHORT-GI-20][DSSS_CCK-40][MAX-AMSDU-7935]` (for channels 5-13)


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
