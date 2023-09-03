# ACARS
Installation for ACARS 

A quick "How to" guide on installing ACARS on your DeFLi or DeSky GS to enable further rewards. 

The install provides: 

(1) Easy Installation - Just copy-paste one command to run bash script and it does everything.

(2) Automatic start - systemd service automatically starts it at boot/reboot.

(3) Easy monitoring & control - Provides commands systemctl status, systemctl stop, systemctl start, and systemctl restart.

(4) Easy configuration - Provides a separate config file in simple format, each config item on a separate line, starting with --

(5) Blacklisting of rtl-sdr - The installation script creates a blacklist file /etc/modprobe.d/blacklist-rtl-sdr.conf with required blacklist entries, preventing error: failed to connect/open rtl-sdr device. 

Please first follow the steps in "Installing RTL-SDR Drivers" and "Installing GQRX"

Copy-paste following command in SSH console and press Enter key. The script will install and configure ACARS. 

sudo bash -c "$(wget -O - https://raw.githubusercontent.com/abcd567a/ad2/master/install-ad2.sh)" 


If installing on 64-bit Raspberry Pi OS
Befor issueing above bash command, give following additional commands:  

sudo dpkg --add-architecture armhf   
sudo apt update 

sudo apt install libudev-dev:armhf   

After script completes installation, it displays following message 

INSTALLATION COMPLETED
=======================
PLEASE DO FOLLOWING:
=======================
(1) In your browser, go to web interface at
     http://ip-of-pi:8686

(2) To view/edit configuration, open config file by following command:
     sudo nano /usr/share/ad2/ad2.conf

    (a) Default value of gain is auto
        To use another value of gain, add following NEW LINE
        (replace xx by desired value of gain)
          --gain xx
    (b) Default value of frequency correction is 0
        To use a different value, add following NEW LINE
        (replace xx by desired frequency correction in PPM)
          --freq-correction xx

    Save (Ctrl+o) and Close (Ctrl+x) the file
    then restart ad2 by following command:
          sudo systemctl restart ad2

To see status sudo systemctl status ad2
To restart    sudo systemctl restart ad2
To stop       sudo systemctl stop ad2

**CONFIGURATION** 

The configuration file can be edited by following command:
sudo nano /usr/share/ad2/ad2.conf  

Default contents of config file
Default setting are are bare minimum.
You can add extra arguments, one per line starting with --


--freq 131550000
--freq 131725000
--http-port 8686

**Lines to add** 

    --gain xx (replace “xx” with your gain setting from GQRX)  

    --freq-correction 65 

    --outServer ad2:30008  

ou must then choose your frequencies and input up to 3 listen frequencies from the list provided here https://defli.network/adapt-defli-gs-acars, they need to be in this format 

    --freq 131550000  

Finally ensure that your HTTP port is set correctly, this should show as: 

    --http-port 8686  

Save and exit the editor. 

You can see all the parameters you can add to the file by using command: 

cd /usr/share/ad2

./acarsdeco2 --help

**SOCAT** 



Finally you must run the SOCAT command or port forward your data to us: You have set the out port to 30008 using the outserver command and so the SOCAT command is: 

socat tcp-l:30008,fork,reuseaddr tcp:162.255.85.52:30010 

To port forward please use the IP of your Pi with internal port as 30010 and external 30008

