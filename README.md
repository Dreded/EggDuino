# EggDuino
Arduino Firmware for Spherebot / Eggbot integration in Inkscape
Forked from [papabricole/EggDuino](https://github.com/papabricole/EggDuino) very outdated original readme in README_ORIG.md

This works with latest software from... https://wiki.evilmadscientist.com/Installing_software follow their instructions to install inkscape and install the plugin.

To write this code to the arduino of your choice I highly recommend [VSCode](https://code.visualstudio.com/) with the Platform.IO Extension(install instructions below)

The aim of this fork is to allow hackers to easily add their own hardware/tweaks.

# Build the firmware

## Using Arduino IDE

    - Click the github 'Clone or Download' button, then 'Download ZIP'
    - Extract the ZIP
    - Rename the extracted folder into 'EggDuino'
    - Open 'EggDuino.ino' using the Arduino IDE
    - Install the 'VarSpeedServo' library
    - Build & upload firmware

## Using Platformio - Recommended

    - Click the github 'Clone or Download' button, then 'Download ZIP'
    - Extract the ZIP
    - Build: 'platformio run'
    - Upload firmware: 'platformio run --target upload' (bottom left of VS Code, Arrow beside checkmark)
    
# Disable Arduino Auto Restart
#### After writing firmware/sketch
##### Failing to do so you will get "Failed to connect to EggBot. :(" when trying to plot inside Inkscape
#### Arduino Uno:
- Install a 10uF capacitor between Reset and GND ( obviously negative side in GND )
- I did this by soldering it to the CNC Shield on the correct pins
    
# Install Platform.IO
### Optional but recommended
This is optional as the sketch/firmware can be uploaded to the board using the Arduino IDE  
If you enjoy Arduino do yourself a favour and install Platform.IO so you can use a real code editor([VSCode](https://code.visualstudio.com/)) with autocompletion.
### Careful to be patient and let it do its thing.
1. Install Microsoft's Free [VSCode](https://code.visualstudio.com/)
2. Open VSCode Extension Manager
3. Search for official PlatformIO IDE extension
4. Install PlatformIO IDE.

### Tested with InkSkape v1.1.2 (march 2022)
The Setup here is for a Arduino UNO Using a Protoneer CNC Shield V3 but can be easily changed for whatever combo you wish just edit the pins in **config.h**

After installing the Evil Mad Scientist InkScape Plugin you then need to go into the InkScape Extensions folder
- Linux: ~/.config/inkscape/extensions/
- Windows: %APPDATA%\inkscape\extensions\

then **edit** the file **axidraw_deps\plotink\ebb_serial.py** using any editor that isn't Notepad( I Like Notepad++ or VS Code )

change the VID:PID to the VID:PID of your board(instructions below)
```
if ebb_port is None:
    for port in com_ports_list:
        if port[2].startswith("USB VID:PID=04D8:FD92"):
            ebb_port = port[0]  # Success; EBB found by VID/PID match.
            break  # stop searching-- we are done.
```

# How to find Vid Pid
### Recommended Method
In VS Code after installing Platform.IO extension...
- Menu: View > Command Palette
- Type: PlatformIO Cli
- Select: PlatformIO: Open PlatformIO Core CLI
- in the terminal that pops up type: pio device list
- Copy the USB VID:PID of your device and paste it into the *inkscape\extensions\axidraw_deps\plotink\ebb_serial.py* file you opened in the above instructions.

### Alternate Method
#### Windows:
- Open Device Manager
- Under Ports(Com & LPT) Right click your device, select Properties
- Go to details Tab
- Change Property Dropdown to: Hardware IDs
- Right clip and Copy the correct line
- will look like USB\VID_1A86&PID_7523
- Convert to normal format: USB VID:PID=1A86:7523
- paste it into the file *inkscape\extensions\axidraw_deps\plotink\ebb_serial.py* you opened in the above instructions.
#### Linux:
- open a terminal
- type: lsusb
- find your device, copy the ID
- paste after VID:PID= in the file *inkscape\extensions\axidraw_deps\plotink\ebb_serial.py* you opened in the above instructions.
