# Progress Report (3 / 25 / 2019)
## Overview
**Passive Sniffing of Camera Traffic on an Isolated Network:** Christian and I discovered interesting messages when we used the EZVIZ android mobile app to discover local EZVIZ devices. We were able to see what looked like SSDP packets since the camera replied with a lot of information including serial number. We didn't think much of it at first but I went back to try and recreate those messages and found out that hikvision, EZVIZ parent company, has a history of using SSDP insecurely. _jgherndz_ 
  
**Android App Pentest:**
  
**WiFi Pumpkin:**
  
**Micro SD Card Attack:**

**MITM Android App:** I rooted an old android phone that I no longer use in order to install a portswigger certificate as an android system certificate. I did this because the EZVIZ application wouldn't let traffic through if the certificate was installed as a user certificate. The app did block some traffic but not all of it when the certificate was under a system certificate so we were able to see some traffic go through. _jgherndz_ _ccescobar_
  
## Outcomes
**Passive Sniffing of Camera Traffic on an Isolated Network:** Through the discovery of those ssdp packets I was able to find out that hikvision software works with with the ezviz products giving us more access to the camera. Through the hikvision software we were able to retrieve the cameras configuration using the default username and password. Christian was then able to find my Wifi SSID as well as Wifi password. _jgherndz_ _ccescobar_
* Discovered the ability to use hikvision software
* Retrieved Wifi SSID and Wifi password from config file

  
**Android App Pentest:** Summary of outcomes
* Outcome1
* Outcome2

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

{"remark":"Test","email":"ccgrassy@gmail.com","phone":"ccgrassy@gmail.com","account":"","shareDeviceInfos":[{"shareCameras":[{"channelNo":1,"permission":127}],"permission":3,"subSerial":"155002344"}]}
```
* Discovered possible missuse cases with the api

## Hinderances
**Passive Sniffing of Camera Traffic on an Isolated Network:** Not digging more into the SDDP packets right away led to not making the discovery that hikvision software can be used on the ezviz cameras.

**MITM Android App:** learning adb commands as well learning the process of rooting an android phone. Finding useful information since most of the traffic seemed to be blocked due to using burp to be a man in the middle.

## Ongoing Risks
(address your project risks identified from Milestone 1 and update them based on your current progress, this should be a table)
