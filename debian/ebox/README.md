# SolarNode eBox 3300 Images

These images were created for the the [eBox 3300][1], based 
off the [SolarNode OS Setup Guide Debian 8][2]. They include support
for `librxtx` and `libyasdi` as well.

The image names are in the form `[OS]-[hardware]-[SD size]`. **Note**
that you can copy the image to a _larger_ SD card, but you must then
expand the root filesystem, or add another partition, to make use of
the additional space.

The *hardware* names are as follows:

 * `ebox3300mx` - the 512MB RAM version of the [eBox 3300MX][1]
 	
# How to copy images to SD card

To restore these onto a SD card, run the following command:

```shell
# Copy image to SD card located at /dev/sde
xz -cd solarnode-deb8.9-ebox3300mx-1GB.img.xz |dd of=/dev/sde bs=2M

# Sync to disk
sync

# Re-read the partition table
blockdev --rereadpt /dev/sde

# Just to be sure, let's check the root filesystem
e2fsck -f /dev/sde1
```

# Network setup

The OS will attempt to get a network connection using the built-in
ethernet device, and use DHCP to obtain an IP address, using the hostname
**solarnode**. Once the computer has fully booted after turning it on (this
can take several minutes) check your DHCP server to find what IP address was
allocated.

# Login user

The system contains a default user of `solar` with password `solar`. That user can
use `sudo` to become the `root` user. You can access the computer via `ssh` only.

# Base SolarNode framework

A base SolarNode framework has been installed in this image. Once the computer has
fully booted and the SolarNode framework has started (this can take several minutes
after the OS has booted and `ssh` is available) you can visit

	http://solarnode/

where `solarnode` is the IP address of the device, if your DNS server does not
support using the _solarnode_ hostname.


# Image partition info

The 1GB images are paritioned like this:

```
Device     Boot Start     End Sectors  Size Id Type
/dev/sde1  *    16384 1953791 1937408  946M 83 Linux
```

The image is copied with a `dd` command like this:

```
dd if=/dev/sde conv=sync,noerror bs=4k count=244224 of=solarnode-deb8.9-ebox3300mx-1GB.img
```

The image is then compressed, and then a digest computed like this:

```
xz -c -9 solarnode-deb8.9-ebox3300mx-1GB.img >solarnode-deb8.9-ebox3300mx-1GB.img.xz
sha256sum solarnode-deb8.9-ebox3300mx-1GB.img.xz >solarnode-deb8.9-ebox3300mx-1GB.img.xz.sha256
```


  [1]: http://www.compactpc.com.tw/ebox-3300MX.htm
  [2]: https://github.com/SolarNetwork/solarnetwork/wiki/Node-OS-Setup-Guide-Debian-8

