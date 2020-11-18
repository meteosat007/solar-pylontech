# solar-pylontech
How to connect to a Sofar HYD3600 Inveter and 2 Pylontech US3000 batteries delivering the measures into an Influxdb, visualising them with Grafana all through the Node-red platform.






I have spent many hours looking at all of the content on the WEB to get connected vis Modbus to the Sofar HYD3600 Inveter and via RS485 to 2 Pylontech batteries in parallel. It would seem that all of the currrent WEB resources for the batteries use different messages which finally was resolved after trial and error.

After trying Python and Javascript options I have decided to go with Node-Red due to its easy block appproach to getting the results quickly with the least amount of complexity. Over the years I have learnt that if its simple to set-up, its more likely to just keep on running.

Here I will share my Node-red flows and photos of the physical connections, the USB-RS485 adpters used on the raspberry Pi loacted close to the inverter and running Node-red.

Once you have your pi OS installed, set-up Wifi and updated the OS using the normal methods shown on the WEB you can install Node-red automatically to the latest version using the command below which is used at the command prompt vis ssh or in a terminal window on the raspberry pi.

bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)

The command above will install the latest versions of Node, Npm and Node-red automatically and if no errors occur it takes about 5 minutes to complete.

