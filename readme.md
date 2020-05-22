This is a basic tutorial to reflash Xiaomi Router 4a Gigabit Edition and install OpenWRT firmware using OpenWRTInvation Exploit tool.

Before going any further in this tutorial, I will say this again 
- "BACKUP YOUR FIRMWARE and YOUR CONFIGURATION"
- I am not responsible for any damage caused by this so do this at your own risk.

What will I actually do here:

- Show how to debrick your device/Reflash Stock firmware (Though I don't have the Global version 2.28.132, I only have the chineese version 2.28.62, if you backed up the global version you can help me to get that file as I've also didn't backup firsthand)

- Use OpenWRTInvasion Exploit tool to telnet into Your Xiaomi 4a router and reflash the OS to OpenWRT (OpenWRTInvation tool github 'http://github.com/acecilia/OpenWRTInvasion')

-------------------X-----------------X---------------
Flashing the stock firmware
---------------------------
[Be sure to back up your stock firmware beforehand]

- Set your local LAN ip to '192.168.0.100' & Mask to ' 255.255.255.0' & Leave Default gateway blank
- Connect the router with your pc to through LAN port
- Make a folder 'tftp' in your home directory
mkdir ~/tftp
- Copy firmware file to this directory and rename it to 'test.bin'
- Power off your router
- Press and hold the reset button and turn it on. Hold until the orange LED start to blink repeatedly.
This blinking mode opens another Mac Address with DHCP/BOOTP Type
- run this (change enp3s0 to your eth0)
sudo dnsmasq -i enp3s0 --dhcp-range=192.168.0.100,192.168.0.254 --dhcp-boot=test.bin --enable-tftp --tftp-root=./tftp/ -d -u nm -p0 -K --log-dhcp --bootp-dynamic 
(This will flash the ~/tftp/test.bin file to your router)
After successfull uploading the log will say something like:

dnsmasq-tftp: sent /home/nm/tftp/test.bin to 192.168.0.206

- Wait around ~5 minutes to let it flash and reboot the router.
- Then change your Local Ethernet ip to 192.168.31.10 & 255.255.255.0 & Default Gateway to 192.168.31.1
- Reboot your router
- Of course, close the terminal session.
- Go to 192.168.31.1 to access your miwifi

----------------------------------------------
Flashing OpenWRT using OpenWRTInvation tool.
----------------------------------------------

- git clone as instructed from acecilia github page (linked above)
- install python3 pip and other requirments as instructed
- cd to your OpenWRTInvation folder then

sudo python3 remote_command_execution_vulnerability.py

(enter router ip and stok)

After successfull exploiting it will tell you to telnet to your router as root with password <blank>.
For some reason I couldn't telnet into the router. It will refuse every request. then I changed my miwifi router as a repeater and connected to internet through another access point. Then I can telnet to by routers ip (192.168.31.7)

telnet 192.168.31.7
cd /tmp

( I have tried the default firmware but it does not have luci included, and cant install using opkg because of mismatch dependencies. User:zorro have created another custom firmware, that is linked here.)

curl http://download1815.mediafire.com/63tq9zb6nlug/0qetz7rm8n9hr04/openwrt-ramips-mt7621-xiaomi_mir3g-v2-squashfs-sysupgrade.bin --output firmware.bin

(in case mediafire link expires, I've uploaded these files in the "Files" Folder)
- Flash this firmware using this command. This will Erase OS1 and flash this custom firmware to mi-wifi

mtd -e OS1 -r write firmware.bin OS1

- After successfull flashing it will reboot
- Now set your lan ip to 192.168.1.2
- Go to 192.168.1.1 to access your OpenWRT
