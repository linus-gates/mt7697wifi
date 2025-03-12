# mt7697wifi-driver
Compatible with linux version 4.14

This project had been chacked on sierra mango red with yocto project as part of kernel-modules recipe.

In order to use this driver:
1. Add the repo folder to wireless directory under linux kernel modules (sources/linux-msm/drivers/net/wireless).
2. Add the repo folder to the Makefile of wireless. Add this line:
    "obj-$(CONFIG_MT7697) += mt7697wifi/"
3. Add the repo folder to the Kconfig of wireless. Add this:
    "source "drivers/net/wireless/mt7697wifi/Kconfig"
4. Add the kernel-modules recipe to the image (sierra-image-base)
5. Create recipe that install "mtwifi" script (located: mt7697wifi-driver/scripts/mtwifi)
    to image rootfs location: "/etc/init.d/mtwifi"

Loading the wifi driver on sierra:
    - Run command: "/etc/init.d/mtwifi start uart 0"

The full options:
/etc/init.d/mtwifi <start/stop/restart> <uart/spi> <wlan number>

KNOWN ISSUE:
Communication via spi not working. Getting error: "usb 1-1.1: failed to read gpios"

# Connect to internet as client:
Add to yocto image: dhcpcd

1. Run "/etc/init.d/mtwifi start uart 0"
2. Run "ip link set wlan0 up"
3. Scan for AP: "iw dev wlan0 scan | grep SSID
4. Configure confgiration file: "wpa_passphrase SSID_NAME PASSWORD > /etc/wpa_supplicant.conf"
5. Connect to AP: "wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant.conf -D wext"
6. Get ip and routing configuration. Run: "dhcpcd"

# Configure the mtwifi script run on boot:
Create yocto recipe and add those lines:
    inherit update-rc.d

    INITSCRIPT_NAME = "mtwifi"
    INITSCRIPT_PARAMS = "start 99 S . stop 99 S ."

    do_install() {
    install -d ${D}/etc/init.d/
    install -m 0755 ${WORKDIR}/mt7697wifi-driver/scripts/mtwifi ${D}/etc/init.d/
    install -m 0755 ${WORKDIR}/mt7697wifi-driver/scripts/pa_wifi.sh ${D}/etc/init.d/
    }

    FILES:${PN} += "/etc"
