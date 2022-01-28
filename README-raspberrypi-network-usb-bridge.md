# What Why Whatfor

My Home Assitant runs in a VM somewhere, but needs access to some USB devices elsewhere. So a small Raspberry Pi computer is running to make these available over network.


## System Environment

Raspberry Pi 2 model B, running Raspbian, with `ser2net` installed, with two USB devices:

- USB DSMR Reader for power/gas sensors "slimme meter"
- USB Arduino Mega => USB RFLINK Gateway => 433Hz antenna

And here is some command output on system specs

```
pi@raspberrypi:~ $ date
Fri 28 Jan 00:19:12 GMT 2022
pi@raspberrypi:~ $ uptime
 00:19:32 up  4:33,  1 user,  load average: 0.03, 0.03, 0.00
pi@raspberrypi:~ $ cat /etc/debian_version
9.8
pi@raspberrypi:~ $ cat /etc/os-release
PRETTY_NAME="Raspbian GNU/Linux 9 (stretch)"
NAME="Raspbian GNU/Linux"
VERSION_ID="9"
VERSION="9 (stretch)"
ID=raspbian
ID_LIKE=debian
HOME_URL="http://www.raspbian.org/"
SUPPORT_URL="http://www.raspbian.org/RaspbianForums"
BUG_REPORT_URL="http://www.raspbian.org/RaspbianBugs"
pi@raspberrypi:~ $ pwd
/home/pi
pi@raspberrypi:~ $ ls
barry-notes.txt
```


## Usage and Troubleshooting (`barry-notes.txt`)

Ser2net geinstalleerd zodat Home Assistant de USB apparaten op deze Pi kan bereiken.


Kijken of alles OK werkt:

    $ sudo service ser2net status

Instellingen:

    $ sudo nano /etc/ser2net.conf

Config voorbeeld:

    # DSMRv4 smart meters
    2001:raw:600:/dev/ttyUSB0:115200 NONE 1STOPBIT 8DATABITS XONXOFF LOCAL -RTSCTS

    # RFLINK
    6001:raw:600:/dev/ttyACM0:57600 8DATABITS NONE 1STOPBIT    # DSMRv4 smart meters

    # RFXTRX433XL
    10001:raw:0:/dev/ttyUSB1:38400 8DATABITS NONE 1STOPBIT

Herstarten na instelling wijzigen:

    $ sudo service ser2net restart

De handle van nieuwe USB apparaten vinden:

    $ dmesg | grep tty
    $ ls /dev/tty*

Het is belangrijk dat je ze steeds in dezelfde poort steekt. Test ook een reboot.
