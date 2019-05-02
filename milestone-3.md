# Progress Report (5 / 2 / 2019)
## Overview
**Vulnerability Scan using Armitage**  
I thought it would be a good idea to run armitage and see what kind of exploits it would recommend to use against the camera. Armitage was a quick and easy way to see if there would be any easy exploits to leverage the camera. _jgherndz_


**Attempt at changing the Camera default password**
A question was asked during the last milestone on whether you can change the default password used to connect to the camera over port 554. I did some testing with the phone application and changed the default password and even removed it to see if it would affect the connection over port 554. _jgherndz_

**Connecting to Camera via Port 8000**

**Utilizing metasploit for common vulnerabilities**
One of the most common tools we used are metasploit.This tool tests all the known vulnerabilities in a system and attempts to crack the system. In our case, the tools detected possible vulnerable points of entry; however, when it ran the scripts and attack the camera, it failed in all counts. _cescobar_

**Sniffing the network using wireshark** Another pass with wireshark so see if there was anything that would stand out from previous reviews. Unfortunaly, this did not yield any significant results. The only data I was able to recognize were typical uPnP packets to communicate to the servers. _cescobar_

**Wi-Fi vulnerability found in marvell:88w8xxxx line** CVE-2019-6496 reports a vulnerabilty with ThreadX-based firmwares using Marvell Avast chips. The device I tested has a variant of the 88w8801 wireless chip, and it is likely to be vulnerable to the attacks mentioned in the CVE. _cescobar_


**Using TCP Connection to connect to Camera via Port 8000**
Since we were unable to get much out of trying to send GET requests to the camera through port 8000 we thought the connection might be TCP. I used python in order to try to connect to the camera but was unable to get any response from the camera. _jghernndz_

**Hikvision IP Camera Access Bypass**
Many Hikvision IP cameras suffer from accessing bypass vulnerabilities which allowing unauthenticated person to access to users’ configuration accounts. After we have done some research, we could figure out superuser account named “admin” can be easily impersonated. Therefore, we used three different URLs to check if we could get any information through this vulnerability. Here is our list of the three URLs and what they are used for:
*  http://<camera.ip>/Security/users?auth=YWRtaW46MTEK - retrieve a list of all users and their roles.
*  http://<camera.ip>/onvif-http/snapshot?auth=YWRtaW46MTEK - get snapchat from the camera without authentication.
*  http://<camera.ip>/System/configurationFile?auth=YWRtaW46MTEK - download the camera configuration. _kalsalehi_ & _msalharthi_

## Outcomes
**Vulnerability Scan using Armitage**
Armitage was able to recommend a few attacks after it completed its initial scan and found the camera. There were 3 categories that armitage found unfortunately none of the exploits worked. _jgherndz_

**Attempt at changing the Camera default password**
After changing and removing the password I found that none of these change the default password used for port 554. The password change is only for the video encryption used for the application. If the video is shared with another user using the phone app then that user would use the password that was changed during testing. _jgherndz_

**Connecting to Camera via Port 8000**



**Using TCP Connection to connect to Camera via Port 8000**
I was unable to get any type of information. _jgherndz_

**Hikvision IP Camera Access Bypass**
We were not able to exploit the vulnerability using the three URLs. All what we can tell from this point is that this vulnerability has been mitigated. _kalsalehi_ & _msalharthi_


## Hinderances
**Vulnerability Scan using Armitage**
The hinderance would be that the attacks that Armitage found were not succcesful. _jgherndz_
  
**Attempt at changing the Camera default password**
There were no hinderances, I was still able to connect using VLC player using the default username and password. _jgherndz_

**Connecting to Camera via Port 8000**
A lack of knowledge on how to write a TCP connection in python could have limited my results. _jgherndez_

**Hikvision IP Camera Access Bypass**
This vulnerability cannot be exploited successfully because Hikvision released a new update for the camera that has fixed the vulnerability.   _kalsalehi_ & _msalharthi_
