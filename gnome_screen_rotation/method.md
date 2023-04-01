# How I got there
(Around the houses and up the garden path)

I took many wrong turns, wondering why nothing was happening when I tried altering values in the sensor matrix. Error number one was researching rotation matrices (this is about direction not rotation) and number two was not watching the indentation, **this is _extremely_ important**.

## Commands
`cat /usr/lib/udev/hwdb.d/60-sensor.hwdb` to see the whole configuration file  
`cat /usr/lib/udev/hwdb.d/60-sensor.hwdb | grep -i 'toshiba'` to filter down to the tablet manufacturer  
`sudo dmesg | grep -i acpi`  
`sudo dmesg | grep -i toshiba`  
`sudo dmesg | grep -i invn6500`  
`udevadm info --export-db` to verify the sensor is detected  
`udevadm info --export-db | grep iio` to filter down to the sensor  
`gdbus introspect --system --dest net.hadess.SensorProxy --object-path /net/hadess/SensorProxy`  
`monitor-sensor` to check the sensors output  
`sudo systemd-hwdb update`  update the system with any changes made to configuration
`sudo udevadm trigger -v -p DEVNAME=/dev/iio:device0`  update the system with any changes made to configuration  
`sudo systemctl restart iio-sensor-proxy.service` restart the service to see the effects of any changes made to configuration


## Useful websites
1. https://gitlab.freedesktop.org/hadess/iio-sensor-proxy
2. https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/drivers/firmware/dmi-id.c
3. https://github.com/systemd/systemd
4. https://www.freedesktop.org/software/systemd/man/hwdb.html
5. https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=dfc57732ad38f93ae6232a3b4e64fd077383a0f1

