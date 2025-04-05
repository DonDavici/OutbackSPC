# Credits

Special thanks goes to the following for the basis of this modified code:

* https://github.com/SirUli/FroniusSmartmeter <br>
* https://github.com/bradcagle/dbus-btbattery <br>
* https://github.com/victronenergy/velib_python <br>

And all other authors and community members of victron which developed or helped to develop drivers, services and componentes
# Inverters
This is a driver for VenusOS devices.

The driver will communicate with PIP/Axpert/Infinisolar/Voltronic/PowLand/OutBack and other compatible solar inverters via Bluetooth <br/>
and publish this data to the VenusOS system.

!!! System need to have bluetooth supported panel, in range!
see images
![screenshot](docs/panel.png)
![screenshot](docs/panel_LCD.png)

## Prerequisites
You need to setup some depenacies on your VenusOS first:

1) SSH to IP assigned to VenusOS device<br/>

2) Resize/Expand file system<br/>
`/opt/victronenergy/swupdate-scripts/resize2fs.sh`

3) Update opkg<br/>
`opkg update`

4) Install pip<br/>
`opkg install python3-pip`

5) Install build essentials as bluepy has some C code that needs to be compiled<br/>
`opkg install packagegroup-core-buildessential`

6) Install glib-dev required by bluepy<br/>
`opkg install libglib-2.0-dev`

7) Install bluepi<br/>
`pip3 install bluepy`

## Installation
When you install with one of these methods the system will take care of starting the service and keep it running. You might want to start things manually. IN this case do not use SetupHelper method. Install manually put skip the install script. After that you can proceed with configuration and testing manually section.

### Install with SetupHelper from kwindrem

This script is meant to be installed through the SetupHelper of kwindrem. 
https://github.com/kwindrem/SetupHelper

Use the following details:

Package Name: `OutbackSPC`<br>
GitHub User: `DonDavici` <br>
Github branch or tag: `main` <br>


### Install manually
1) Install git<br>
`opkg install git`

2) switch to victronenergy directory
`cd /opt/victronenergy`

3) Clone dbus-btbattery repo<br/>
`git clone https://github.com/DonDavici/OutbackSPC.git`


4) run install script
`run ./installservice.sh`


5) Reboot your system
`reboot`

## Configuration
#### Due to the fact that we have to connect to the device to read the data (I don't think it is has implemented notifiy functionality - maybe I am wrong ;-) we have to trust and pair our device manually before we can use it with our service.

1) select our service directory
`cd /opt/victronenergy/OutbackSPC`
or if you used SetupHelper
`cd /data/OutbackSPC`

2) scan for nearby devices
`./scan.py`

After you have found out the mac adress of your device you have to make the device connectable.<br/> 
replace xx:xx:xx:xx:xx:xx with the Bluetooth address of your Inverter<br/>

4) start bluetoothctl<br>
`bluetoothctl`
5) trust the device<br>
`trust xx:xx:xx:xx:xx:xx`
6) pair with the device<br>
`pair xx:xx:xx:xx:xx:xx` (enter 000000 or 123456)
7) connect optionaly to verify connection<br>
`connect xx:xx:xx:xx:xx:xx`
8) exit bluetoothctl
`exit`
9) edit default_config.ini (set OUTBACK_ADDRESS)
`nano default_config.ini`
10) reboot system
`reboot`

## Test with output to console 
#### with config (OUTBACK_ADDRESS has to be set in default_config.ini)
`/opt/victronenergy/OutbackSPC/dbus-btoutback.py`

#### without config
`/opt/victronenergy/OutbackSPC/dbus-btoutback.py xx:xx:xx:xx:xx:xx`
>
### Logfiles
`/var/log/OutbackSPC`

# NOTES: This driver is is still in development and may not work as intended. Use at your own risk!

