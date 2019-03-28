# Progress Report (3 / 25 / 2019)
## Overview
**Passive Sniffing of Camera Traffic on an Isolated Network:** Christian and I discovered interesting messages when we used the EZVIZ android mobile app to discover local EZVIZ devices. We were able to see what looked like SSDP packets since the camera replied with a lot of information including serial number. We didn't think much of it at first but I went back to try and recreate those messages and found out that hikvision, EZVIZ parent company, has a history of using SSDP insecurely. _jgherndz_ 
  
**Android App Pentest (Emulator):** Using VMWare Workstation I attempted to get a variety of Android emulators up and running with the EZVIZ application. The goal was to be able to pentest the app and maybe even manipulate the system to attack the camera or gather data. Some of the emulators attempted include: Cm-x86_64-14.1-r2.iso, Cm-x86-14.1-r2.iso, and android-x86_64-8.1-r1.iso. When those failed to function correctly, I moved on to Android Studio. Here I made two virtual devices (one running Android 8.1 and one with Android 9.0). _twlayne_
  
**Android App Pentest (ADB):** I used the Android Debug Bridge (ADB) tool from Android Studio to see what information I could gather from the app while I had it running on my phone. I aimed to generate a variety of log files using commands found on https://developer.android.com/studio/command-line/adb. I used dumpheap, dumpstate, dumpsys, force-network-logs, and logcat. I also looked into performing a tcpdump, but to do so requires a rooted phone. Jose agreed he would look into it for Milestone 3. _twlayne_
  
**WiFi Pumpkin:** Jose got the WiFi Pumpkin up and running on his laptop with a wireless adapter card. We then worked together to connect my phone and camera to the rogue access point. All we were able to pull was POST requests, but we may give the tool another shot to see if we can figure out how to get more data with it. _twlayne_ _jgherndz_
  
**MITM Android App:** I rooted an old android phone that I no longer use in order to install a portswigger certificate as an android system certificate. I did this because the EZVIZ application wouldn't let traffic through if the certificate was installed as a user certificate. The app did block some traffic but not all of it when the certificate was under a system certificate so we were able to see some traffic go through. _jgherndz_ _ccescobar_
  
  
**Serial Communications:** Once we took apart the camera, I noticed a label “UART”, which can be used to communicate with the OS via serial communication. Jose and I found measured the currents with a volt meter and figured out which pin can be used to obtain serial. <add picture of the serial port in the camera> 
The first step was to create a cable that would allow us to get serial communication with the device. I decided to solder the cables to prevent miscommunication or other errors. 

Connecting the cables to a USB serial adapter was the easy part. What we needed was to figure out the baud rate, bits and parity the camera would use to communicate. Countless hours were spent testing the most common and even some obscure baud rates, but I only got invalid data. <insert picture of the invalid data.> I decided to reach out to Dr. Mahoney, cyber sec prof at UNO, and ask for some help to get information about the device’s communications. 
  
   *Case:*
  The communication port would not require authentication or would use default settings. 
  
   *Misuse:*
  Unfortunatly, there was not a reliable point of entry, thus I was unable to find a possible misuse case.
  
  **Direct Chip Programming:**
  Attempted to read the content of the chip(W25Q64JV), using a chip programming board(CH341a Black Edition) and free to use software (CH341a Programmer). This method would allow us to read, write and wipe the memory/BIOS of the camera. 
  
   *Case:*
  Avility to read and modify the contents of the chip. Flash new software directly to the board. 
   
   *Misuse:*
    We could modify or flash the contents of the chip with mallicious code to allows to open a back door and obtain direct access to the device. This failed due to the time constrains and limited understanding of ARM architecture. 
  
  **Open ports in the device:**
  There are several open pots on my device 8000,9010,8200,544. Nmap provided a guess of the possible services running in that port, so I decided to conduct reserach to see if said service would be vulnerable or had other backdoors. And _"oh boy! let me tell you!!"_  _**to be continued in the next few points...**_
  
  *Case:*
  Open ports with possible backdoors or vulnerable services open to the internet. 
  
  *Misuse:*
  If there was indeed a backdoor, we could obtain information that we are not authorized to have, or obtain some level of proviledge to sniff traffic.  
  
  **Weak Default Configurations:**
  *Case:*
  Default configurations aid the user and the application to connect to the camera once for setup. If not properly configured or sanitized, default configurations can be guessed allowing bad actors to have access to the device's configuration. 
  
   *misuse:*
   _Oh boy, Where do I start. _ WE HACKED IT. 
  
## Outcomes
**Passive Sniffing of Camera Traffic on an Isolated Network:** Through the discovery of those ssdp packets I was able to find out that hikvision software works with with the ezviz products giving us more access to the camera. Through the hikvision software we were able to retrieve the cameras configuration using the default username and password. Christian was then able to find my Wifi SSID as well as Wifi password.  
* Discovered the ability to use hikvision software
* Retrieved Wifi SSID and Wifi password from [config file](config-info.png)
  
**Android App Pentest (Emulator):** 
* I learned that the EZVIZ app is thoroughly secured to not run on virtual devices
  
**Android App Pentest (ADB):** I was able to successfully generate some dump/log files with data relating to EZVIZ activity. However, none of it seemed very critical. Note1: the *_ezviz files are the parts of the original files that reference "ezviz". Note2: the dumpsys file was too large to upload.
* dumpstate: [dumpstate](ADB/dumpstate.txt) | [dumpstate_ezviz](ADB/dumpstate_ezviz.txt)
* dumpsys: [dumpsys_ezviz](ADB/dumpsys_ezviz.txt)
* logcat: [logcat](ADB/logcat.txt) | [logcat_ezviz](ADB/logcat_ezviz.txt)
  
**WiFi Pumpkin:** After using the WiFi Pumpkin, the best we could get from it were POST requests for each transaction sent from my phone/camera
* POST requests

**MITM Android App:** We were able to find an api that the application uses when the user shares their camera with another EZVIZ user.
```
POST /api/mobile/share/batch?sessionId=ac4bf3df727f414e8f021a58dc4f7e20 HTTP/1.1
Host: apiius.ezvizlife.com
Connection: close
Content-Length: 204
Origin: https://apiius.ezvizlife.com
User-Agent: Mozilla/5.0 (Linux; Android 8.1.0; ONEPLUS A3000 Build/OPM7.181205.001; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/71.0.3578.99 Mobile Safari/537.36 Ezviz/Android/
Content-Type: application/json
Accept: */*
Referer: https://apiius.ezvizlife.com/front_static/mobile_share/h5/views/share-manage.html
Accept-Encoding: gzip, deflate
Accept-Language: en-US
Cookie: clientType=3; clientVersion=3.8.2.0106; userId=; lang=en; C_SS=ac4bf3df727f414e8f021a58dc4f7e20
X-Requested-With: com.ezviz

{"remark":"Test","email":"gmailEmail@gmail.com","phone":"gmailEmail@gmail.com","account":"","shareDeviceInfos":[{"shareCameras":[{"channelNo":1,"permission":127}],"permission":3,"subSerial":"155000000"}]}
```
* Discovered possible missuse cases with the api  
  
**Serial Communication:**  I was able to get a stream of data but decided to reach out to Dr. Mahoney, cyber sec prof at UNO, and ask for some guidance in order to get more information about the device’s communications.

## Hinderances
**Passive Sniffing of Camera Traffic on an Isolated Network:** Not digging more into the SDDP packets right away led to not making the discovery that hikvision software can be used on the ezviz cameras.

**Android App Pentest (Emulator):** On most emulations, it cannot be downloaded onto the devices through the Google Play Store (see [Android Studio_Fail](PlayStoreFail.JPG)). Sometimes I could download it but then it would crash repeatedly. I even tried using APK files from https://www.apkmonk.com/app/com.ezviz/#previous; the install would always fail at the last second. 
  
**Android App Pentest (ADB):** Two of the ADB commands I ran failed to produce any positive results. [dumpheap](ADB/dumpheap_fail.JPG) failed due to an anti-debugging security exception. [force-network-logs](ADB/forceNetworkLogs_fail.JPG) failed due to my inability to get the command working. 
  
**WiFi Pumpkin:** The tool was unexpectedly not user friendly. Our troubles in getting more data could be linked to either user error, a bad tool, or the camera/phone are secured in a way to not allow the viewing of more in-depth network data. If we have the time, we can pursue the tool again.

**MITM Android App:** learning adb commands as well learning the process of rooting an android phone. Finding useful information since most of the traffic seemed to be blocked due to using burp to be a man in the middle.

**Serial Communication:** What we needed was to figure out the baud rate, bits and parity the camera would use to communicate. Countless hours were spent testing the most common and even some obscure baud rates, but I only got invalid data. <insert picture of the invalid data.> 
  
  
## Ongoing Risks
(address your project risks identified from Milestone 1 and update them based on your current progress, this should be a table)
