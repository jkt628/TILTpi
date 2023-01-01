# Tilt Pi Installation Instructions for Raspberry Pi OS "Bullseye"

## Update January 1, 2023

These instructions worked on December 2, 2020 version of Raspberry Pi OS Lite "Buster" and probably still do.

1. Using SSH from another computer or from a Raspberry Pi command line directly, enter the following commands. Update Raspbian packages and install python install utility, "python3-distutils".

   ```bash
   sudo apt-get update
   sudo apt-get install python3-distutils
   ```

1. Get the “aioblescan” bluetooth scanner module customized with Tilt plugin, unzip downloaded file, go to install folder, and install.

   ```bash
   wget https://github.com/jkt628/aioblescan/archive/Tilt.zip
   unzip Tilt.zip
   cd aioblescan-Tilt/
   sudo -H python3 setup.py install
   ```

1. As an optional step, you can run the following command to verify Tilt scanner is working. Make sure Tilt is nearby and floating if doing the test. Use "Ctrl-C" to stop scanning.

   ```bash
   sudo python3 -u -m aioblescan --tilt
   ```

1. Install node-red update script for Raspberry Pi using version that works with Google Sheets web apps. Answer "yes" to install Raspberry Pi specific nodes.

   ```bash
   bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
   ```

1. Install dashboard ui module for node-red and enable auto-start of node-red at boot.

   ```bash
   cd ~/.node-red
   sudo -H npm install node-red-dashboard
   sudo systemctl enable --now nodered.service
   ```

1. Download Tilt Pi “flow” that uses the new Aioblescan module

   ```bash
   wget https://raw.githubusercontent.com/jkt628/TILTpi/Bullseye/flow.json
   ```

1. Run downloaded Tilt Pi app/flow in node-red:

   ```bash
   curl -X POST http://localhost:1880/flows -H "Content-Type: application/json" -H "Node-RED-Deployment-Type: nodes" --data "@flow.json"
   ```

1. As an optional step, you can add a WiFi connection manager script that has a robust algorithm to maintain the WiFi connection if the connection is unstable.

   ```bash
   curl -L wifi.brewpiremix.com | sudo bash
   ```

1. In a web browser visit `http://raspberrypi.local:1880/ui` or `http://localhost:1880/ui` (if not using SSH to install). If this doesn't work, you may need to find the IP address of your Raspberry Pi and go to `http://X.X.X.X:1880/ui` where `X.X.X.X` is your Raspberry Pi's IP address.
