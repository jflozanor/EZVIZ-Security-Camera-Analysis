# Progress Report (5 / 2 / 2019)
## Overview
**Vulnerability Scan Using Armitage:**  
I thought it would be a good idea to run Armitage and see what kind of exploits it would recommend to use against the camera. Armitage was a quick and easy way to see if there would be any easy exploits to leverage the camera. _jgherndz_

**Attempt at Changing the Camera Default Password:**  
A question was asked during the last milestone on whether you can change the default password used to connect to the camera over port 554. I did some testing with the phone application and changed the default password and even removed it to see if it would affect the connection over port 554. _jgherndz_

**Utilizing Metasploit for Common Vulnerabilities:**  
One of the most common tools we used are metasploit. This tool tests all the known vulnerabilities in a system and attempts to crack the system. In our case, the tools detected possible vulnerable points of entry. However, when it ran the scripts and attacked the camera, it failed in all counts. _cescobar_

**Sniffing the Network Using Wireshark:**  
Another pass with Wireshark so see if there was anything that would stand out from previous reviews. Unfortunately, this did not yield any significant results. The only data I was able to recognize were typical uPnP packets to communicate to the servers. _cescobar_

**Wi-Fi Vulnerability Found in Marvell:88w8xxxx Line:**  
CVE-2019-6496 reports a vulnerability with ThreadX-based firmwares using Marvell Avast chips. The device I tested has a variant of the 88w8801 wireless chip, and it is likely to be vulnerable to the attacks mentioned in the CVE. _cescobar_

**Using TCP Connection to Connect to Camera via Port 8000:**  
Since we were unable to get much out of trying to send GET requests to the camera through port 8000 we thought the connection might be TCP. I used python in order to try to connect to the camera. I sent packets to mimic the ones seen in Wireshark but was still unable to get any response from the camera. _jghernndz_

**Hikvision IP Camera Access Bypass:**  
Many Hikvision IP cameras suffer from accessing bypass vulnerabilities which allow an unauthenticated person to access to users’ configuration accounts. After we have done some research, we could figure out superuser account named “admin” can be easily impersonated. Therefore, we used three different URLs to check if we could get any information through this vulnerability. Here is our list of the three URLs and what they are used for:
*  http://<camera.ip>/Security/users?auth=YWRtaW46MTEK - retrieve a list of all users and their roles.
*  http://<camera.ip>/onvif-http/snapshot?auth=YWRtaW46MTEK - get snapshot from the camera without authentication.
*  http://<camera.ip>/System/configurationFile?auth=YWRtaW46MTEK - download the camera configuration. _kalsalehi_ & _msalharthi_

**Investigating the Open Port 554:**  
As we found in nmap scan that the port 554 is open, so we tried to investigate this port to see if we can get any useful information. We ran the following command in terminal: Nmap --script rtsp-url-brute -p 554 192.168.1.21 (we attached the result of this command that we got.) _kalsalehi_ & _msalharthi_

**Connecting to Camera via Port 8000:**  
We opened iVMS and allowed Wireshark to start capturing the packets from iVMS. Once we stopped the scan, we found that there are only GET requests of HTTP protocol which were sent from iVMS to the camera via port 8000 (we uploaded a screenshot). Also, it shows some login session information as username=admin&random=10015011. As we already know that port 8000 is open by nmap scan, we also tried to do netcat session to double check that the port 8000 is open (screenshot is provided). _kalsalehi_ & _msalharthi_

**Open Ports, but No Vulnerabilities Found:**  
As nmap scan shows that there are some open ports, in this section we are focusing on ports 8000, 8200, and 9010. We tried to search for ways to exploit these three ports to get access to the camera. _kalsalehi_ & _msalharthi_

**Deauthenticating the Camera from the Access Point Using Aireplay-ng:**
The goal is to find the necessary information using Kismet and a wireless adapter. The information I need for this to work is the MAC address and the channel for the camera and access point. _smrooney_  

**Using Wireshark on the USB Connection:**  
While brainstorming for more attack vectors, I thought about the possibility of data being transferrable through the micro-USB port. To test this, I connected the camera to my laptop with a USB cable and ran the USB plugin for Wireshark against the connection. To help ensure proper test procedures, I tried a combination of two different cameras and two different cables. _twlayne_  
  
**Revisiting ADB via Android Studio:**  
After realizing there could be more to discover in ADB's logging feature via the Android Studio application, I set the log to record while performing the following activities: login to EZVIZ app with fingerprint, setup camera on WiFi/phone, changed camera name in the app, changed encryption password, logged out of EZVIZ app, and logged back in with my regular password. To analyze the information collected, I opened the [ADBLogOutput](ADB/ADBLogOutput.txt) in Notepad++ and used the Find tool to search for any data that may be sensitive or aid with another attack. _twlayne_  
  
**Tcpdump:**  
Another Android investigation tool available is Tcpdump. With a rooted phone and the command line interface for ADB, Tcpdump serves as a way to capture network packets directly from the device. After obtaining a copy of the binary for the tool, I followed the steps listed on the https://wladimir-tm4pda.github.io/porting/tcpdump.html web page. This resulted in me collecting a pcap file ([Tcpdump Capture](ADB/capture_tcpdump.pcap)) that I could view in Wireshark. _twlayne_

## Outcomes
**Vulnerability Scan Using Armitage:**  
Armitage was able to recommend a few attacks after it completed its initial scan and found the camera. There were 3 categories that Armitage found. Unfortunately, none of the exploits worked. _jgherndz_

**Attempt at Changing the Camera Default Password:**  
After changing and removing the password, I found that none of these changed the default password used for port 554. The password change is only for the video encryption used for the application. If the video is shared with another user using the phone app then that user would use the password that was changed during testing. _jgherndz_

**Using TCP Connection to Connect to Camera via Port 8000:**  
I was unable to get any type of information from the tcp connection. _jgherndz_
  
**Hikvision IP Camera Access Bypass:**  
We were not able to exploit the vulnerability using the three URLs. All that we can tell from this point is that this vulnerability has been mitigated. _kalsalehi_ & _msalharthi_

**Investigating the Open Port 554:**  
In the nmap result, we found some information such as rtsp://192.168.1.21/user=admin_password=tlJwpbo6_channel=1_stream=0.sdp?real_stream As we see there is user=admin, password=tlJwpbo6. _kalsalehi_ & _msalharthi_

**Connecting to Camera via Port 8000:**  
In Wireshark result, we clicked on Hypertext Transfer Protocol to get the full request URL. We thought that would be the parameters as we know that GET sends parameters in the URL; which we need to add to the camera IP address to login to the camera onboard server via browsers. But we got a message “can’t reach this page” (two screenshots are provided). Also, we tried to filter my packages by this filter “http && ip.src == <the camera ip address>”, we could not find anything. That means there is no response coming from the camera. We did not get any POST requests on Wireshark, we are not sure if this is because the high level of protection that iVMS has! Or the camera does not respond. At the end we found that there is no way to access the camera’s onboard server via browsers, so we could not run ZAP tool as we were planning to do. Also, we figured out that random, which is in login session information, represents the session ID since it changes each time we login into iVMS. _kalsalehi_ & _msalharthi_
  
**Open ports, but No Vulnerabilities Found:**  
As a result of our search, we could find exposed vulnerability in port 8000 that may work for Splunk software company not for Hikvision. Also, we could not find anything that may help us to exploit port 8200. Lastly, we moved to port 9010, we found a vulnerability called UPnP NAT Traversal that may work to exploit the port 9010. _kalsalehi_ & _msalharthi_ 

**Deauthenticating the Camera from the Access Point Using Aireplay-ng:**  
I was able to block the communication between the camera and the access point (router). This command persists until you stop it with CTRL-C. This attack is not limited to just WiFi connected cameras but most IoT devices. Ways to protect against this attack include reducing the range of your access point, using cables instead of going wireless, or making your network hidden.  The last one is not advised because it could make someone try harder to get into the network if the network is hidden. _smrooney_    

**Using Wireshark on the USB Connection:**  
Data is not transferrable via the micro-USB port. Wireshark failed to detect any data utilizing the connection while the camera was in use. Device was not detected by my computer. _twlayne_  
  
**Revisiting ADB via Android Studio:**  
While digging into the output from ADB, I made a some small discoveries:  
* When logging into the application with a password, "password_value" is a visible piece of data in the log. However, it is not clear text; the value appears hashed.  
* Altering the camera name and encryption password did not result in log entries. With this, there were no clear text values with the input I provided during the changes.
* No network data (such as IP, MAC, and SSID) could be found.
* During initial app loading, userID 0 was used. This is known to be the superuser/root account for an Android device.
* In some log entries, my phone's device model (SM-G965U) could be read, along with the EZVIZ process being denied access vector cache (AVC) permission. _twlayne_  
  
**Tcpdump:**  
While the captured packets failed to reveal any super helpful data, there were several things I found that were interesting:  
* Throughout many of the packets, it became evident that EZVIZ relies on Amazon AWS for at least some of their server activities.
* Packets such as number 941 serve as GET requests for image files where the source IP is that of the phone. In the pcap file, I saw requests for 3.png, 4.png, and 5.png. I opened the gallery on the phone and found 1.png and 2.png. The images were nothing more than thumbnail-like pictures of the camera.
* While we are already well aware that the EZVIZ cameras are developed in a way that allows for compatibility with Hikvision tools, some of the packets from Tcpdump contain more proof of this fact. Like in packet 1398, Hikvision is directly referenced in a query being performed by the camera.
* Throughout the pcap data, it is evident that encryption is being used to hide any sensitive data. Encryption Alerts and Encrypted Handshakes are spread throughout the file. _twlayne_

## Hinderances
**Vulnerability Scan Using Armitage:**  
The hinderance would be that the attacks that Armitage found were not successful. _jgherndz_
  
**Attempt at Changing the Camera Default Password:**  
There were no hinderances, I was still able to connect using VLC player using the default username and password. _jgherndz_

**Using TCP Connection to Connect to Camera via Port 8000:**  
A lack of knowledge on how to write a TCP connection in python could have limited my results. _jgherndez_

**Hikvision IP Camera Access Bypass:**  
This vulnerability cannot be exploited successfully because Hikvision released a new update for the camera that has fixed the vulnerability.   _kalsalehi_ & _msalharthi_

**Investigating the Open Port 554:**  
This method of obtaining login credentials is seperate from our previous attempt at tinkering with port 554. The login information we found using the nmap scan wasn't able to login succesfully using VLC. _kalsalehi_ & _msalharthi_ & _jgherndz_

**Connecting to Camera via Port 8000:**  
Wireshark is not able to get any response from the camera, so we can investigate the traffic that could be captured. We think that happens because there is a high-level protection layer on iVMS preventing the camera’s response to get captured or there is manufacture errors which is mostly the correct reason. _kalsalehi_ & _msalharthi_

**Open Ports, but No Vulnerabilities Found:**  
we could not do anything with our finding for port 8000 since the finding is not going to work on Hikvision. We skipped the port 8200 since we have not found anything to play around with. Lastly, the vulnerability that we found for port 9010, it allows anyone to get control remotely across the public internet, so it is not useful in our case since we are on an isolated environment. _kalsalehi_ & _msalharthi_  
  
**Deauthenticating the Camera from the Access Point Using Aireplay-ng:**  
There were a few setbacks, like the initial guide being incomplete, using a bad computer, and not fully understanding the GUI for Kismet. _smrooney_
  
**Revisiting ADB via Android Studio:**
A small hinderance encountered while using ADB was the Android studio app would not filter my logs based on the process name/ID. I had to resort to a text string filter on "ezviz". _twlayne_
