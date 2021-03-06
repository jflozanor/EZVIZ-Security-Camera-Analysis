# Progress Report (3 / 25 / 2019)
## Overview
**Passive Sniffing of Camera Traffic on an Isolated Network:** Christian and I found interesting messages when we used the EZVIZ Android mobile app to discover local EZVIZ devices. We were able to see what looked like SSDP packets since the camera replied with a lot of information including the serial number. We didn't think much of it at first, but I went back to try and recreate those messages and found out that Hikvision, EZVIZ's parent company, has a history of using SSDP insecurely. _jgherndz_ 
  
**Android App Pentest (Emulator):** Using VMWare Workstation I attempted to get a variety of Android emulators up and running with the EZVIZ application. The goal was to be able to pentest the app and maybe even manipulate the system to attack the camera or gather data. Some of the emulators attempted include: Cm-x86_64-14.1-r2.iso, Cm-x86-14.1-r2.iso, and Android-x86_64-8.1-r1.iso. When those failed to function correctly, I moved on to Android Studio. Here I made two virtual devices (one running Android 8.1 and one with Android 9.0). _twlayne_
  
**Android App Pentest (ADB):** I used the Android Debug Bridge (ADB) tool from Android Studio to see what information I could gather from the app while I had it running on my phone. I aimed to generate a variety of log files using commands found on https://developer.android.com/studio/command-line/adb. I used dumpheap, dumpstate, dumpsys, force-network-logs, and logcat. I also looked into performing a tcpdump, but to do so requires a rooted phone. Jose agreed he would look into it for Milestone 3. _twlayne_
  
**WiFi Pumpkin:** Jose got the WiFi Pumpkin up and running on his laptop with a wireless adapter card. We then worked together to connect my phone and camera to the rogue access point. All we were able to pull was POST requests, but we may give the tool another shot to see if we can figure out how to get more data with it. _twlayne_ _jgherndz_
  
**MITM Android App:** I rooted an old Android phone that I no longer use in order to install a portswigger certificate as an Android system certificate. I did this because the EZVIZ application wouldn't let traffic through if the certificate was installed as a user certificate. The app did block some traffic but not all of it when the certificate was under a system certificate so we were able to see some traffic go through. _jgherndz_ _ccescobar_
  
**Serial Communications:** Once we took apart the camera, I noticed a label “UART”, which can be used to communicate with the OS via serial communication. Jose and I measured the currents with a volt meter and figured out which pin can be used to obtain serial. <add picture of the serial port in the camera> 
The first step was to create a cable that would allow us to get serial communication with the device. I decided to solder the cables to prevent miscommunication or other errors. 

Connecting the cables to a USB serial adapter was the easy part. What we needed was to figure out the baud rate, bits and parity the camera would use to communicate. Countless hours were spent testing the most common and even some obscure baud rates, but I only got invalid data. <insert picture of the invalid data.> I decided to reach out to Dr. Mahoney, cybersecurity professor at UNO, and ask for some help to get information about the device’s communications. 
  
*Case:*
The communication port would not require authentication or would use default settings. 
  
*Misuse:*
Unfortunately, there was not a reliable point of entry, thus I was unable to find a possible misuse case. _ccescobar_
  
**Direct Chip Programming:**
Attempted to read the content of the chip(W25Q64JV), using a chip programming board(CH341a Black Edition) and free to use software (CH341a Programmer). This method would allow us to read, write, and wipe the memory/BIOS of the camera. 
  
*Case:*
Ability to read and modify the contents of the chip. Flash new software directly to the board. 
   
*Misuse:*
We could modify or flash the contents of the chip with malicious code to allows to open a back door and obtain direct access to the device. This failed due to the time constrains and limited understanding of ARM architecture. _ccescobar_
  
**Open ports in the device:**
There are several open ports on my device 8000,9010,8200,544. [Nmap](Network/nmap_scan.PNG) provided a guess of the possible services running in that port, so I decided to conduct research to see if said service would be vulnerable or had other backdoors. _**to be continued in the next few points...**_
  
*Case:*
Open ports with possible backdoors or vulnerable services open to the internet. 
  
*Misuse:*
 If there was indeed a backdoor, we could obtain information that we are not authorized to have, or obtain some level of privilege to sniff traffic.  _ccescobar_
 
 **Risky Default Configurations:**
    *Case:*
    Default configurations aid the user and the application to connect to the camera once for setup. If not properly configured or sanitized, default configurations can be guessed allowing bad actors to have access to the device's configuration. Since EZVIZ, uses the default administrative account of "admin" and a password like "abcxyz", it can be relatively easy to guess/crack. 

  *Misuse:*
  A bad actor can access the camera's internal console or administrative settings and take ownership of the device or allow unauthorized  access to other parties. _ccescobar_
 
 **Port 544:** 
 This port allows for authenticated users to view live feed across the internet or local network. 
  
  *Case:*
  This port uses the default authentication from the camera's config file and can be used in third party applications. 
  
  *Misuse:*
  With stolen credentials, a bad actor can view or share live feed of the camera without the owner's knowledge. _ccescobar_
 
**Web App Pentest (Password Policy):**
Mohammed and I created an account on the camera web app because it allows us to manage the camera remotely. We set up the account’s password (abc123) to measure the minimum required password on the web app. Moreover, we created another account with username (userabc123) and the password (abc123) is derivative of the username to test the level of the password complexity requirement. After we created the two accounts, we tried to log in on the web app several times with wrong password to see how many attempts are allowed to enter the passwords. Furthermore, once we clicked on forget password, we could reset the password using the same old password (abc123).  _Kalsalehi_ _msalharthi_
  
**iOS App Pentest:** 
Khalid and I were responsible for pentesting the EZVIZ camera through iOS application. We used an actual iPhone which was iPhone SE and it has 11.4.1 iOS version. We found a jailbreak called unc0ver that is suitable for our iOS version. We could download the jailbreak on the iPhone. Moreover, we tried to install Xcode, a development environment for macOS, on our PCs. _Kalsalehi_ _Msalharthi_
  

## Outcomes
**Passive Sniffing of Camera Traffic on an Isolated Network:** Through the discovery of those SSDP packets I was able to find out that Hikvision software works with the EZVIZ products giving us more access to the camera. Through the Hikvision software we were able to retrieve the cameras configuration using the default username and password. Christian was then able to find my WiFi SSID as well as WiFi password.  
* Discovered the ability to use Hikvision software
* Retrieved WiFi SSID and WiFi password from [config file](Network/config-info.png)
* [pcap](Network/Camera_alone.pcap) of camera alone
* [pcap](Network/phone_searching_lan.pcapng) of phone searching for cameras on LAN
  
**Android App Pentest (Emulator):** The emulators always ended in either failing to download the app, or failing to open the app after installation.
* I learned that the EZVIZ app is thoroughly secured to not run on virtual devices
  
**Android App Pentest (ADB):** I was able to successfully generate some dump/log files with data relating to EZVIZ activity. However, none of it seemed very critical. Note1: the *_ezviz files are the parts of the original files that reference "ezviz". Note2: the dumpsys file was too large to upload.
* dumpstate: [dumpstate](ADB/dumpstate.txt) | [dumpstate_ezviz](ADB/dumpstate_ezviz.txt)
* dumpsys: [dumpsys_ezviz](ADB/dumpsys_ezviz.txt)
* logcat: [logcat](ADB/logcat.txt) | [logcat_ezviz](ADB/logcat_ezviz.txt)
  
**WiFi Pumpkin:** After using the WiFi Pumpkin, the best we could get from it were POST requests for each transaction sent from my phone/camera
* POST requests

**MITM Android App:** We were able to find an API that the application uses when the user shares their camera with another EZVIZ user.
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
* Discovered possible misuse cases with the API  
  
**Serial Communication:** I was able to get a stream of data but decided to reach out to Dr. Mahoney, cybersecurity professor at UNO, and ask for some guidance in order to get more information about the device’s communications.

**Web App Pentest (Password Policy):** The web app has implemented a weak password policy that allows users to create:
* Easily guessable passwords that do not contain specific characters requirements; for example: mix of capital/small letters, symbols and numbers. 
* Passwords that are either same as usernames or a derivative of usernames.  
 Also, we found that the web app has:
* No account lockout or CAPTCHA implemented on ‘login’ page.
* Password history is not maintained. 
* No two-factor authentication(2FA) options are provided.
An attacker can launch a brute force attack to crack users’ login credentials due to the weakness of the password requirements and there is no account lockout, CAPTCHA, or 2FA implemented on ‘Login’ page. 
  
**iOS App Pentest:** When we tried to install the jailbreak, the iPhone automatically restarted without completing to install the jailbreak. We looked up how to solve this issue, and we could find that we must enable and disable some certain settings from the jailbreak to install it successfully, but we could not get rid of the issue. After discussing with Dr. Hale, we decided to stop penetrating on iOS application due to our limitation and started to work with our teammates on the other aspects of the project.
  

## Hinderances
**Passive Sniffing of Camera Traffic on an Isolated Network:** Not digging more into the SSDP packets right away led to not making the discovery that Hikvision software can be used on the EZVIZ cameras.

**Android App Pentest (Emulator):** On most emulations, the app cannot be downloaded onto the devices through the Google Play Store (see [Android Studio_Fail](PlayStoreFail.JPG)). Sometimes I could download it but then it would crash repeatedly. I even tried using APK files from https://www.apkmonk.com/app/com.ezviz/#previous; the install would always fail at the last second. 
  
**Android App Pentest (ADB):** Two of the ADB commands I ran failed to produce any positive results. [dumpheap](ADB/dumpheap_fail.JPG) failed due to an anti-debugging security exception. [force-network-logs](ADB/forceNetworkLogs_fail.JPG) failed due to my inability to get the command working. 
  
**WiFi Pumpkin:** The tool was unexpectedly not user friendly. Our troubles in getting more data could be linked to either user error, a bad tool, or the camera/phone are secured in a way to not allow the viewing of more in-depth network data. If we have the time, we can pursue the tool again.

**MITM Android App:** Learning ADB commands as well as learning the process of rooting an Android phone. Finding useful information since most of the traffic seemed to be blocked due to using burp to be a man in the middle.

**Serial Communication:** What we needed was to figure out the baud rate, bits and parity the camera would use to communicate. Countless hours were spent testing the most common and even some obscure baud rates, but I only got invalid data. 
  
**Web App Pentest (Password Policy):**
No permission was given to us to verify manually that the web app is vulnerable to Brute Force attack.
  
**iOS App Pentest:**
* After we changed certain settings in the Jailbreak (unc0ver), the iPhone kept rebooting.
* We could not install Xcode, a development environment for macOS, on PCs because it violates Apple policy.

  
## Ongoing Risks
|Risk name (value)  | Impact     | Likelihood | Description | Still Relevant |Occurred|
|-------------------|------------|------------|-------------|----------------|-------|
|Brick security camera (30) | 6 | 5 | It's possible that we may brick the security camera while trying to gain access to it via the hardware | Yes| Camera appears to be bricked due to attempting to load a new config file using Hikvision software|
|Corrupt micro SD card (20) | 4 | 5 | It's possible that we may corrupt an SD card while attempting to gain access to the device using the SD card | No | N/a |
|Team member being unavailable/unwilling to help (32) | 8 | 4 | There may be a loss in productivity if one or more team members are unable to cooperate | Yes | N/a |
|Cannot attack via network (15) | 3 | 5 | There may be no network-based vulnerabilities | Yes | Still haven't given up we seem to have found at least one way in |
|Cannot attack via micro SD card (15) | 3 | 5 | There may be no way to attack through the SD card | No | Documentation that we had found is no longer available|
|Cannot attack via IOS/Android Apps (15) | 3 | 5 | There may be no vulnerabilities allow us to get access to the camera via the phone apps | Yes | iPhone app wasn't successful, there are still a couple more things to try on the Android side|
  
# Diagrams
[Use Case: Camera](Diagrams/Use_Case_Camera.PNG)  
[Use Case: API](Diagrams/Use_Case_Api.PNG)  
[Process: Emulator](Diagrams/Process_Emulator.JPG)
