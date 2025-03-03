# mt7697wifi-driver

Notes:

-   There is random error (Somtimes happend, somtimes not). The error is:


    "root@swi-mdm9x28:~# modprobe mt7697q
    [   82.360284] spi_master spi2: mt7697spi_init(): bus_find_device_by_name('spi2.0') failed
    [   82.385637] spi_master spi2: channel_config_store() failed(-22)
    [   82.385673] spi_master spi2: mt7697spi_init(): cp2130_update_ch_config() failed(-22)
    modprobe: can't load module mt7697q (kernel/drivers/net/wireless/mt7697wifi/mt7697q-driver/mt7697q.ko): Invalid argument"

    And when running the modprobe mt7697q command again:


    "root@swi-mdm9x28:~# modprobe mt7697q
    [   84.998185] spi_master spi2: channel_config_store() failed(-22)
    [   84.998220] spi_master spi2: mt7697spi_init(): cp2130_update_ch_config() failed(-22)
    [   85.025616] spi_master spi2: channel_config_store() failed(-22)
    [   85.025650] spi_master spi2: mt7697spi_init(): cp2130_update_ch_config() failed(-22)
    modprobe: can't load module mt7697q (kernel/drivers/net/wireless/mt7697wifi/mt7697q-driver/mt7697q.ko): Invalid argument"

    The statistic error is in function cp2130_update_ch_config(). Check why.
