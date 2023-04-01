# Screen Rotation

Getting the screen rotation to work on a Toshiba WT10-a-102 Tablet.

This tablet was not matched in `/usr/lib/udev/hwdb.d/60-sensor.hwdb`  
Fortunately the sensor has the same orientation as the similar Toshiba WT10-a-103 tablet that is in the file:
```
#########################################
# Toshiba
#########################################

# Toshiba Encore WT10A tablet
sensor:modalias:acpi:INVN6500*:dmi:*:svnTOSHIBA:pnTOSHIBAWT10-A-103:*
 ACCEL_MOUNT_MATRIX=0, -1, 0; -1, 0, 0; 0, 0, 1
```
It therefore only needs a file at `/etc/udev/hwdb.d/61-sensor-local.hwdb` containing the lines:
```
# Toshiba Encore WT10A tablet
sensor:modalias:acpi:INVN6500*:dmi:*:svnTOSHIBA:pnTOSHIBAWT10-A-102:*
 ACCEL_MOUNT_MATRIX=0, -1, 0; -1, 0, 0; 0, 0, 1
``` 
 as per the insructions in `/usr/lib/udev/hwdb.d/60-sensor.hwdb`
