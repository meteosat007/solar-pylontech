# solar-pylontech
How to connect to a Sofar HYD3600 Inveter and 2 Pylontech US3000 batteries delivering the measures into an Influxdb, visualising them with Grafana all through the Node-red platform.

![Grafana](https://user-images.githubusercontent.com/41062235/99589010-4eb91700-29e3-11eb-87da-2c0199f952e3.jpg)

I have spent many hours looking at all of the content on the WEB to get connected vis Modbus to the Sofar HYD3600 Inveter and via RS485 to 2 Pylontech batteries in parallel. It would seem that all of the currrent WEB resources for the batteries use different messages which finally was resolved after trial and error.

After trying Python and Javascript options I have decided to go with Node-Red due to its easy block appproach to getting the results quickly with the least amount of complexity. Over the years I have learnt that if its simple to set-up, its more likely to just keep on running.

Here I will share my Node-red flows and photos of the physical connections, the USB-RS485 adapters used on the raspberry Pi located close to the inverter and running Node-red.

Once you have your pi OS installed, set-up Wifi and updated the OS using the normal methods shown on the WEB you can install Node-red automatically to the latest version using the command below which is used at the command prompt vis ssh or in a terminal window on the raspberry pi.

bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)

The command above will install the latest versions of Node, Npm and Node-red automatically and if no errors occur it takes about 5 minutes to complete.

To access Node-red from your browser enter the URL below using your ip address for the pi.             
http://x.x.x.x:1880

You will need to ensure that you install the listed nodes below into Node-red Manage Palette

![Node-red Palette](https://user-images.githubusercontent.com/41062235/99590834-cd16b880-29e5-11eb-8084-f3150aaf1ce8.jpg)

The Sofar Battery Data nodes to extract battery related data via the inveter over Modbus. 

![Sofar Battery Data](https://user-images.githubusercontent.com/41062235/99590855-d30c9980-29e5-11eb-889f-7c0435c73496.jpg)

The Sofar Inverter Data nodes to extract inverter related data via the inveter over Modbus. 

![Sofar Inverter Data](https://user-images.githubusercontent.com/41062235/99590861-d56ef380-29e5-11eb-8deb-cae8469d25ac.jpg)

The Pylontech Battery Data nodes to extract data from the RS485 interface on the Master battery.

![Pylontech Master Battery](https://user-images.githubusercontent.com/41062235/99590845-cf791280-29e5-11eb-9d6f-6475cd8493e2.jpg)

The Pylontech Battery Data nodes to extract data from the RS485 interface on the Master Battery via the link cable.

![Pylontech Slave Battery](https://user-images.githubusercontent.com/41062235/99590852-d142d600-29e5-11eb-9bf9-5ce377ab07b3.jpg)

You will need to follow the many online examples of how to setup Grafana and Influxdb onto the machine which will host the database and grafana instance.
I used this link which worked well.  https://ksummersill.medium.com/raspberry-pi-4-with-influx-telegraf-and-grafana-to-monitor-sensor-data-6487efbef42b

To get the RS485 data into the USB on the pi used to connect to your Inverter and or Batteries I have used the item below from Amazon. Very cheap but works well. I have to find out how to lock each one to the required USB interface to avoid issues on reboot.

![RS485 USB Adapter](https://user-images.githubusercontent.com/41062235/99592383-0d773600-29e8-11eb-9223-0295719651f6.jpg)

For information here are the correct request strings in ASCII for the two batteries. Info on the WEB seems out of date for the slave battery.

Master Battery              Request
Get analog values           ~20024642E00202FD33\r
Get Serial Number           ~20024693E00202FD2D\r

Slave Battery
Get analog values           ~20034642E00203FD31\r
Get Serial Number           ~20034693E00203FD2B\r

Get analog Values

Request
7E3230303234363432453030323032464433330D        HEX
~20024642E00202FD33                             ASCII

Response
~20024600F07A11020F0CC50CC30CC50CC40CC40CC60CC50CC40CC70CC50CC60CC60CC50CC60CC6050B370B230B230B2D0B2DFFE0BF8DFFFF04FFFF001B00BBE4012110E1D6      ASCII

HEX Broken down by measure with 139 HEX digits                                                  Hex position
7e 32 30 30 32 34 36 30 30 46 30 37 41 31 31 30 32                                              0,16
30 46                   No of cells                                                             17,18
30 43 43 35             cell 1 =                                                                19,22
30 43 43 33                                                                                     23,26
30 43 43 35                                                                                     27,30
30 43 43 34                                                                                     31,34
30 43 43 34                                                                                     35.38
30 43 43 36                                                                                     39,42
30 43 43 35                                                                                     43,46
30 43 43 34                                                                                     47,50
30 43 43 37                                                                                     51,54
30 43 43 35                                                                                     55.58
30 43 43 36                                                                                     59,62
30 43 43 36                                                                                     63,66
30 43 43 35                                                                                     67,70
30 43 43 36                                                                                     71,74
30 43 43 36             Cell 15 =                                                               75,78
30 35                   Temp No  = 5                                                            79,80
30 42 33 37             Temp BMS Board                                                          81,84    
30 42 32 33             Temp Avg Cell 1-4                                                       85,88
30 42 32 33             Temp Avg Cell 5-8                                                       89,92
30 42 32 44             Temp Avg Cell 9-12                                                      93,96
30 42 32 44             Temp Avg Cell 13-15                                                     97,100
46 46 45 30             Current now first 46=Negative/discharge  0=positive/Charge  -4064mA     101,104
42 46 38 44             Pack Voltage =49037mv                                                   105,108                             
46 46 46 46             Remaining Capacity 1                                                    109,112
30 34                   User Defined Number 4                                                   113,114
46 46 46 46             Module Total Capacity 1                                                 115,118
30 30 31 42             Cycles =27                                                              119,122
30 30 42 42 45 34       Remaining Capacity 2 48100mAh                                           123,128
30 31 32 31 31 30       Module Total Capacity 2  74000mAh                                       129,134
45 31 44 36                                                                                     135,138


