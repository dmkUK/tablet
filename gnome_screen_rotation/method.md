# How I got there
(Around the houses and up the garden path)

I took many wrong turns, wondering why nothing was happening when I tried altering values in the sensor matrix. Error number one was researching rotation matrices (this is about direction not rotation) and number two was not watching the indentation, **this is _extremely_ important**.

## Commands
`cat /sys/devices/virtual/dmi/id/modalias` to see dmi of tablet where 
  bvn =   (BIOS vendor)
  bvr =   (BIOS version)
  bd  =   (BIOS date)
  svn =   (system vendor)
  pn  =   (product name)
  pvr =   (product version)
  rvn =   (board vendor)
  rn  =   (board name)
  rvr =   (board version)
  cvn =   (chassis vendor)
  ct  =   (chassis type)
  cvr =   (chassis version)  
`cat /usr/lib/udev/hwdb.d/60-sensor.hwdb` to see the whole configuration file  
`cat /usr/lib/udev/hwdb.d/60-sensor.hwdb | grep -i 'toshiba'` to filter down to the tablet manufacturer  
`udevadm info -q path -n /dev/iio:device*` finds the sensor in this case 'INVN6500' at 'device0'  
`udevadm info --export-db` to verify the sensor is detected, returns the platform, name and more for the sensor  
`udevadm info --export-db | grep iio` to filter down to the sensor 
`gdbus introspect --system --dest net.hadess.SensorProxy --object-path /net/hadess/SensorProxy`  
`monitor-sensor` to check the sensors output  
`sudo dmesg | grep -i toshiba`  diagnostic messages for the tablet manufacturer (finds "DMI: TOSHIBA TOSHIBA WT10-A-102/Type2")  
`sudo dmesg | grep -i invn6500` diagnostic messages for the sensor (finds the sensor was using identity matrix ie. no change)  
`sudo systemd-hwdb update`  update the system with any changes made to configuration
`sudo udevadm trigger -v -p DEVNAME=/dev/iio:device0`  update the system with any changes made to configuration  
`sudo systemctl restart iio-sensor-proxy.service` restart the service to see the effects of any changes made to configuration

## Useful websites
1. https://gitlab.freedesktop.org/hadess/iio-sensor-proxy
2. https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/drivers/firmware/dmi-id.c
3. https://github.com/systemd/systemd
4. https://www.freedesktop.org/software/systemd/man/hwdb.html
5. https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=dfc57732ad38f93ae6232a3b4e64fd077383a0f1
6. https://people.skolelinux.org/pere/blog/Modalias_strings___a_practical_way_to_map__stuff__to_hardware.html

