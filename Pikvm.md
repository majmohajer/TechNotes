[HOME](home.md)  

## Troubleshooting
If device shows in your router under connected wifi device but local ip address
is not working, try incognito mode as chrome extension can cause problem  

### Update 
To update, run following commands under the root user:

[root@pikvm ~]# pikvm-update

### Setup 

To setup: Download the os and install RPi Imager tool (https://www.raspberrypi.com/software/)  and
 choose custom os to pick the downloaded os image on your hard drive.


### First Steps Important  

Check Device Manager in windows to verify it is not showing as pikvm

Change video format to MJPEG if you are seeing a black screen after login. WebRTC has some issues which might be port related.

To capture ctrl-alt-del run in cmd prompt:  
start chrome --app="https://192.168.1.76/"

 ### Terminal  

Defaults:  root root  for console and admin admin for web

-In Web Terminal to obtain superuser privilages run: su - root (prompt says password!)
- ifconfig      displays information about the current network status
- hostnamectl displays 

- Change root and admin passwords as below
<pre>
[root@pikvm ~]# rw
[root@pikvm ~]# passwd root
[root@pikvm ~]# kvmd-htpasswd set admin
[root@pikvm ~]# ro
</pre>

Change ssh password as well.   
ssh root@pikvm (use root as password)

reboot

### add user
kvmd-htpasswd set myuser

### How to Set or Change Hostname in Linux
use hostnamectl command to display information  
Edit /etc/kvmd/meta.yaml to alter the displayed server name in the web UI  
Execute: hostnamectl set-hostname yourhostname  
edit /etc/kvmd/meta.yaml to alter the displayed hostname in the web U

### Wifi 
To set up wifi (WPA3 is not supported), create a pikvm.txt file on sd card and add   
 WIFI_ESSID='ATTP7Vd3HB'  
 WIFI_PASSWD='9194910338'  
See https://docs.pikvm.org/on_boot_config/

Alternatively, you can add multiple wifi networks using  
 wpa_passphrase 'MyNetwork' 'P@assw0rd' > /etc/wpa_supplicant/  wpa_supplicant-wlan0.conf  

To add another wifi network, you just need to run the same command BUT don't overwrite the file, append to it. So it'll look like this:

wpa_passphrase 'MyNetwork2' 'P@assw0rd2' >> /etc/wpa_supplicant/wpa_supplicant-wlan0.conf

The double ">>" (instead of the single ">") means append and it'll add another "network" field to your /etc/wpa_supplicant/wpa_supplicant-wlan0.conf file.


### Yaml  

Use nano to edit files and do not repeat kvmd. Only 4 spaces should be used for indentation

### Enabling Mouse Jiggler 
To enable the Jiggler, it is required to allow some config lines to /etc/kvmd/override.yaml (after that, it will be available in the Web UI for activation)  
<pre>
kvmd:  
    hid:  
jiggler:  
            enabled: true  
            active: true  
</pre>

### Emulate various USB devices on the target machine  
By default this is what is set:
<pre>
otg:
    manufacturer: PiKVM
    product: Composite KVM Device
    vendor_id: 0x1D6B
    product_id: 0x0104
    serial: CAFEBABE
 </pre>
You can change how this is displayed with the following example for /etc/kvmd/override.yaml file:
<pre>
otg:
    manufacturer: Corsair
    product: Corsair Gaming RGB
    vendor_id: 0x6940
    product_id: 0x6973
    serial:
</pre>
Use the following USB database to get the desired devices: https://the-sz.com/products/usbid or https://devicehunt.com.

Turn off Mass storage to not show under DVD/Cd-rom as pikvm in Device Manager  
<pre>
kvmd:  
    msd:  
        type:  disabled

kvmd:
    atx:
        type: disabled
</pre>

### Advance stuff
https://docs.pikvm.org/edid/#edid-examples-for-pikvm-v2  
[root@pikvm ~]# kvmd-edidconf --set-mfc-id=LNX     
--set-monitor-name=PiKVM --set-audio=1  
[root@pikvm ~]# reboot  
