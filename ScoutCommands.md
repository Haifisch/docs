# power

## power.ischarging

#### Description
`int power.ischarging()`

Determines if the Scout is charging its battery via USB. This will also mirror the orange charging LED next to the USB port, which also shines when charging, and turns off when either fully-charged, or the Scout is not plugged in.

```js
> print power.ischarging
> 1
```

#### Parameters
None

#### Return Values
Returns `0` if the Scout is not charging, and `1` if it is charging.

#### Warning
The results of this command are undefined if you don't have a battery plugged into the Scout, and only powering it via USB.  This is due to the Lipo battery charger constantly trying to charge the non-existent battery.


## power.percent

#### Description
`int power.percent()`

All Scouts have an on-board fuel gauge that tells you the status of the battery. This returns the percentage of battery life left on the Scout.

```js
> print power.percent
> 85
```

#### Parameters
None

#### Return Values
Returns a value of 0 to 100 based on the percentage of charge the battery contains.

#### Warning
The results of this command are undefined if you don't have a battery plugged into the Scout, and only powering it via USB.  This is due to the fuel gauge being unable to determine how much battery charge is available when there is no battery present.


## power.voltage

#### Description
`int power.voltage()`

All Scouts have an on-board fuel gauge that tells you the status of the battery. This returns the current voltage of the battery, multiplied by 100.  Divide the result by 100 to get the actual battery voltage: e.g. `414` is really 4.14 volts.

```js
> print power.voltage
> 414
```

#### Parameters
None

#### Return Values
Returns a value of ~3.4 volts to ~4.2 volts, based on the current voltage of the battery.

#### Warning
The results of this command are undefined if you don't have a battery plugged into the Scout, and only powering it via USB.  This is due to the fuel gauge being unable to determine how much battery voltage is available when there is no battery present.


## power.enablevcc

#### Description
`void power.enablevcc()`

This provides 3.3 volts to the **3V3** header pin.  Turn this on if you want to power any external devices connected to the **3V3** header pin.

```js
> power.enablevcc
>
```

#### Parameters
None

#### Return Values
None, but after running this command, the **3V3** header will be at 3.3 volts.


## power.disablevcc

#### Description
`void power.disablevcc()`

This turns off power to the **3V3** header pin.  You would turn this off if you want to remove power from any external devices connected to the **3V3** pin, perhaps to save battery life.

```js
> power.disablevcc
>
```

#### Parameters
None

#### Return Values
None, but after running this command, the **3V3** header will be at 0 volts.


## power.sleep
#### Description
`void power.sleep(ms)`

Puts the Scout to sleep for *ms* milliseconds.  Upon waking up, it will continue to run the commands it was running prior to sleeping.  When a Scout is asleep, it cannot communicate with any other boards or the web.  The good news is that it'll be in an extremely low power state, so your battery will last much longer!

```js
> power.sleep(1000)
>
```

The Scout can be woken up by an external pin change event on pin D4, D5, D7, or when the fuel gauge's battery alarm is triggered.  If any of these pins change state from an external source, like a button, it will wake up the Scout.


#### Parameters
*ms* - The number of milliseconds to sleep.  One second equals 1000 milliseconds.  The maximum time a Scout can sleep is ~49 days.

#### Return Values
None, but after running this command, the Scout will be asleep.


## power.report

#### Description
`power.report`

Print a JSON response of the power status of the Scout. Included is the battery percent charge available, the voltage of the battery, if the battery is currently charging, and if power is available on the **3V3** header pin.

```js
> power.report
>
{
 "type":"power",
 "battery":100,
 "voltage":417,
 "charging":false,
 "vcc":true
}
```


#### Parameters
None

#### Return Values
A JSON representation of the current state of power for the Scout.

- *type* - The type of report returned.  In this case it will be the string *power*
- *battery* - The amount of battery percentage available in the battery. Range is 0 to 100.
- *voltage* - The voltage of the battery, x100--so *417* is 4.17 volts.  Range is between 3.4 volts and 4.2 volts.
- *charging* - A true/false value showing if the battery is currently charing.
- *vcc* - A true/false value showing if power is enabled to the **3V3** header pin that powers backpacks.

# mesh networking

## mesh.config
#### Description
`mesh.config(scoutId, troopId, channel=20)`

Set the mesh radio settings for this scout. You can set the unique ID of the scout, the troop that this scout is associated with, as well as what channel the scout should use to communicate with other scouts on the network.

```js
> mesh.config(1, 2, 20)
>
```

#### Parameters
- *scoutId* - The unique ID of this scout on the mesh network. Valid range is 0 to 65535.
- *troopId* - The ID of the troop this scout is associated with, also known as a PAN ID. Valid range is 0 to 65535.  
- *channel* - The 802.15.4 channel for this troop.  Valid range is 11 to 26. All scouts in a troop must be on the same channel. Default is *20*.

#### Return Values
None

## mesh.setpower
#### Description
`mesh.setpower(powerLevel)`

Set the mesh radio power settings for this scout.  The higher the power, generally there is longer range and less drop-outs, but requires more battery power.


```js
> mesh.setpower(1)
>
```

#### Parameters
- *powerLevel* - The power level set using the following map

  * *0*:   3.5 dBm
  * *1*:   3.3 dBm
  * *2*:   2.8 dBm
  * *3*:   2.3 dBm
  * *4*:   1.8 dBm
  * *5*:   1.2 dBm
  * *6*:   0.5 dBm
  * *7*:  -0.5 dBm
  * *8*:  -1.5 dBm
  * *9*:  -2.5 dBm
  * *10*: -3.5 dBm
  * *11*: -4.5 dBm
  * *12*: -6.5 dBm
  * *13*: -8.5 dBm
  * *14*: -11.5 dBm
  * *15*: -16.5 dBm

#### Return Values
None


## mesh.setdatarate(rate)
Set the mesh radio data rate, from 250kbit/sec up to 2Mbit/sec using the following map:

 * 0:    250 kb/s  | -100 dBm
 * 1:    500 kb/s  |  -96 dBm
 * 2:   1000 kb/s |  -94 dBm
 * 3:   2000 kb/s |  -86 dBm

The higher the speed chosen, the less signal sensitivity the radio has--which usually translates to less radio range in the real world.

## mesh.key(key)
Set the mesh radio security key, enabling the AES128 hardware encryption.  All Scouts in a troop should use the same key in order to communicate.  The key given should be 0-16 characters.

## mesh.resetkey()
Resets the mesh radio security key.

## mesh.joingroup(groupId)
Join this Scout to a mesh group

## mesh.leavegroup(groupId)
Remove this Scout from a mesh group

## mesh.ingroup(groupId)
Returns true/false if this Scout is in the group given

## mesh.ping(scoutId)
Ping another Scout in the mesh network.

## mesh.pinggroup(groupId)
Ping Scouts in an group. If available, at least one Scout will return a response.

## mesh.send(scoutId, message)
Send *message* to another Scout with ID of *scoutId*

## mesh.verbose(enabled)
Turn on/off verbose mesh radio debugging output

## mesh.report()
Prints a JSON response of the mesh radio status of the Scout.
```
{
 "type":"mesh",
 "scoutid":2,
 "troopid":1234,
 "routes":0,
 "channel":20,
 "rate":"250 kb/s",
 "power":"3.5 dBm"
}
```
## mesh.routing()
Prints a human-readable output of the current routing table for this Scout.

## mesh.announce(groupId, message)
Send *message* to an entire group at *groupId*

## mesh.signal()
Return the last received message’s signal strength (RSSI.)  Typical values returned will be somewhere between -30 and -95, with lower numbers being a lower signal.  Since they’re negative values, -95 is a weaker signal than -30.

## mesh.loss()
Return the last received message’s link quality indictator (LQI.)  Possible values are 0-255.  255 indicate no packet loss has occurred, and lower values indicate an increasing amount of packet loss is happening.

# miscellaneous

## temperature()
Print the current on-chip temperature in Celsius.

## randomnumber()
Print a true hardware-based random number

## uptime()
Print the number of milliseconds that have passed since the last Scout boot-up.

## report()
Print all of the Scout’s reports.

## verbose(enabled)
Enable all verbose output if *enabled* is set to 1

# led

## led.blink(red, green, blue, ms=500)
Blink the LED with the values of *red*, *green*, and *blue*.  Possible color values are between 0 and 255.  Pass the optional fourth argument to change the blink duration, with a default of half a second.

## led.off()
Turn off the LED.

## led.red(ms=0, continuous=0)
Make the LED red.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.green(ms=0, continuous=0)
Make the LED green.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.blue(ms=0, continuous=0)
Make the LED blue.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.cyan(ms=0, continuous=0)
Make the LED cyan.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.purple(ms=0, continuous=0)
Make the LED purple.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.magenta(ms=0, continuous=0)
Make the LED magenta.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.yellow(ms=0, continuous=0)
Make the LED yellow.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.orange(ms=0, continuous=0)
Make the LED orange.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.white(ms=0, continuous=0)
Make the LED white. Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.torch(ms=0, continuous=0)
Make the LED the Scout’s torch color.  Set *ms* to the number of milliseconds to be on if you want to blink the LED.  Set *continuous* to 1 to make the LED blink continuously until you call another led command or led.off.

## led.sethex(hexValue)
Set the LED to the *hexValue* given. Similar to HTML, “RRGGBB”, but no hash at the beginning.

## led.setrgb(red, green, blue)
Set the LED to values given by *red*, *green*, and *blue*.

## led.savetorch(red, green, blue)
Save the torch color to values given by *red*, *green*, and *blue*.

## led.report()
Print JSON of the current LED red, green, and blue values, and the torch color’s values.
```
{
 "type":"led",
 "led":[0,0,255],
 "torch":[0,255,0]
}
```
# pins
TODO
pin.on
pin.off
pin.makeinput
pin.makeinputup
pin.makeoutput
pin.setmode
pin.read
pin.write
pin.report.digital
pin.report.analog

# backpack

## backpack.report
Print JSON of the current backpacks attached to the Scout.
```
{
 "_":"bps",
 "a":[1,0,1,16,0,0,74,192]
}
```

## backpack.list
Lists all backpacks attached:
```
 01000116000074192
```

# scout

## scout.report
Print JSON of the Scout information, including information like family, hardware version, serial number, and build.
```
 {
 "type":"scout",
 "lead":false,
 "version":1,
 "hardware":1,
 "family":1000,
 "serial":2000052,
 "build":20140130
}
```
## scout.isleadscout
Returns true if the Scout is a Lead Scout, and has a Wi-Fi backpack attached.

## scout.sethqtoken(token)
Write the HQ token for this Scout given by *token*.

## scout.gethqtoken()
Print the HQ token currently assigned to this Scout.

## scout.delay(ms)
Delay the Scout for *ms* milliseconds.  Radio, shell, and other systems will continue to run, so it’s non-blocking.

## scout.daisy()
This will wipe a Scout and make it factory-fresh.  Running it once will ask you to run it again to confirm.  When run a second time, it will reset all settings in a Wi-Fi backpack, if attached, and will reset the mesh radio settings, HQ token, and mesh key.

## scout.boot
Restart the Scout.

# HQ

## hq.settoken(<token>)
Set the HQ token for this Scout.

## hq.gettoken()
Print the HQ token for this Scout.

## hq.verbose(enabled)
Enable verbose HQ output when *enabled* = 1.

# events

## events.start()
Start the event handler to trigger reports, callbacks, and eventing internals.

## events.stop()
Stop all event handlers on a Scout.

## events.setfreqs(digitalMs=50, analogMs=60000, peripheralMs=60000)
Set the frequency of the various event handlers. *digitalMs* will set how often the digital pin event handlers are called, and default to twenty times per second.  *analogMs* will set how often the analog pin event handlers are called, and default to once every 60 seconds.  *peripheralMs* will set how often the peripheral event handlers are called, and default to once every 60 seconds.  The peripherals include the battery percentage, voltage, charging flag, battery alarm, and temperature.

## events.verbose(enabled)
Turn on verbose output for event handlers when *enabled* = 1.

# key/value
TODO
## key

## key.print

## key.number

## key.save

# lead scout

## wifi.report()
Print the current report of the Wi-Fi connection:

```
{
 "type":"wifi",
 "version":0,
 "connected":true,
 "hq":true
}
```

## wifi.status()
Print human-readable information about the Wi-Fi module:

```
Wi-Fi Versions: S2W APP VERSION=2.5.1
S2W GEPS VERSION=2.5.1
S2W WLAN VERSION=2.5.1
MAC=20:f8:5e:a1:4a:57
WSTATE=CONNECTED     MODE=AP
BSSID=00:1b:63:2c:18:b3   SSID="Pinoccio" CHANNEL=2   SECURITY=WPA2-PERSONAL
RSSI=-50
IP addr=10.0.1.128   SubNet=255.255.255.0  Gateway=10.0.1.1
DNS1=10.0.1.1       DNS2=0.0.0.0
Rx Count=94     Tx Count=10232
CID  TYPE  MODE  LOCAL PORT  REMOTE PORT  REMOTE IP
1  TCP-SSL CLIENT  48838    22757    173.255.220.185
```

## wifi.list()
Print out a list of Wi-Fi access points (APs) nearby

```
       BSSID        SSID   Channel Type   RSSI  Security
 44:94:fc:62:b2:72, Louise  , 01, INFRA , -85 , WPA2-PERSONAL
 00:1b:63:2c:18:b3, Pinoccio, 02, INFRA , -52 , WPA2-PERSONAL
No.Of AP Found:2
```


## wifi.config(wifiAPName, wifiAPPassword)
Set the Wi-Fi access point name and password in order to connect.

## wifi.dhcp(hostname=NULL)
Enable DHCP for the Wi-Fi backpack.  An optional *hostname* can be passed if desired, but not required.

## wifi.static(ip, netmask, gateway, dns)
Set static IP settings for the Wi-Fi backpack.  *ip* is the static IP you wish to assign to the backpack.  *netmask* is the netmask.  *gateway* is usually the IP address of your router, and *dns* is the DNS server you’d like to use.

## wifi.reassociate()
Calling this makes the Wi-Fi backpack reassociate with its AP as well as attempt to auto-connect to HQ.

## wifi.command(command)
Send a raw command to the Wi-Fi module.  Please see the Gainspan GS1011MIPS datasheet for the raw commands.

## wifi.ping(hostname)
TODO: Ping a remote server.  Hostname can be a fully-qualified domain name, or can be an IP address.

## wifi.dnslookup(hostname)
TODO: Look up the IP address of the given *hostname*.

## wifi.gettime()
Return the current time, as fetched from the Wi-Fi backpack via network time protocol (NTP.)  The time returned is in UTC.

```
9/2/2014,21:48:43,1391982523746
```

## wifi.sleep()
TODO: Put the Wi-Fi backpack to sleep.  This does not put the entire Scout to sleep, just the Wi-Fi module itself.

## wifi.wakeup()
TODO: Wakes up the Wi-Fi module from a previous sleep state.

## wifi.verbose(enabled)
Turn on verbose Wi-Fi backpack output if *enabled* is set to 1.
