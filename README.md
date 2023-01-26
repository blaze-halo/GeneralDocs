# hardware
Where all schematics, PCBs, and electronics live


https://github.com/cli/cli/blob/trunk/docs/install_linux.md

run gh auth login and following text and GUI instructions:
1) Select "GitHub.com" and then hit enter key    - not "GitHub Enterprise SErver" 
2) Select HTTPS and then hit enter key
3) Type "Y"
4) Select "Login with a web browser" and then hit enter key. Browser will open. Copy the 8 character long code in termal into the browser




Order for camera plug in is: Front Left, Front, Front Right, Rear Left, Rear, Rear Right



   ## To ONLY Debug Hardware (using SSH access to OBCs) 

   #### Log off SSH access as soon as you are done with your session.

1. **Turn on wifi using software team's help:**
   ```
   sudo nmcli radio wifi on
   ```
   

2. **You can now ssh using the OBC's credentials.** 
   ```
   ssh halo@OBC-X
   ```
   Enter password

3. **Set of commands to debug hardware.** 
   ### DO NOT go outside this umbrella of tested commands, used purely for debug info only.
   - **GPS connection:** 
     Command to check if GPS deevice is connected to OBC.
     GPS connects over a serial port. 
     Can be checked by:
     ```
     ls -l /dev/ttyACM*
     ``` 
     If you see a device named `ttyACM0/ttyACM1`, GPS is connected.

    - **GPS data:** 
      Check if NMEA sentences are received. Run:
      ```
      sudo cat /dev/ttyACM0|grep GNGGA
      ```
      If you see lat, long data, GPS is functional. 

     - **Modem connection:**
       Command to check how many modems connected to any OBC. Run:
       ```
       nmcli d
       ```
       To see device names of connected modems.
       If you see 3 devices starting with 'Device' names enp, enx, usb having 'Type' Ethernet, all modems are connected

     - **Modem statistics:**
       Command to check bandwidth usage, latency, etc. for each connected modem. Run:
       ```
       nload -m
       ```

     - **Cameras connected:**
       Command to check how many cameras connected to the OBC. Run: 
       ```
       lsusb | grep Microdia
       ```
       This will return USB cameras and the USB Bus they are on. 

     - **After every instance that you unplug and plug the cameras, you have to make sure they are plugged back in the right order, and then run**
       ```
       sudo systemctl restart teleop
       ```
       After this you are supposed to run a script to arrange the cameras in the right order for the RP console. Contact the Software team for this.

     - **To keep checking what cameras are connected, and their behavior in terms of dropping off and coming back, Run:**
       ```
       watch -n 0.1 'lsusb | grep Microdia'
       ```
       where `-n,--interval` is the interval between output updates and 0.1s is the fastest this command will execute.
       Followed by the command inserted in quotes, especially if cascaded commands. 

     - **Some more commands to get status info:**
  
       To check all running nodes and their status:
       ```
       sudo systemctl status
       ``` 

       To check an individual node's status 

       ```
       sudo systemctl status <node_name>
       ```

       After every unplug and replug of cameras

       ```
       sudo systemctl restart teleop
       ```

       If the network services are not working with the modems being healthy. 

       ```
       sudo systemctl restart doppelganger@-*
       ```
     
     
  
      
     
