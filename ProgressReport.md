# Progress Report (3 / 25 / 2019)
## Overview
**Passive Sniffing of Camera Traffic on an Isolated Network:** Christian and I discovered interesting messages when we used the EZVIZ android mobile app to discover local EZVIZ devices. We were able to see what looked like SSDP packets since the camera replied with a lot of information including serial number. We didn't think much of it at first but I went back to try and recreate those messages and found out that hikvision, EZVIZ parent company, has a history of using SSDP insecurely. _jgherndz_ 
  
**Android App Pentest:**
  
**WiFi Pumpkin:**
  
**Micro SD Card Attack:**
  
## Outcomes
**Passive Sniffing of Camera Traffic on an Isolated Network:** Through the discovery of those ssdp packets I was able to find out that hikvision software works with with the ezviz products giving us more access to the camera. Through the hikvision software we were able to retrieve the cameras configuration using the default username and password. Christian was then able to find my Wifi SSID as well as Wifi password. _jgherndz_ _cescobar_
* Discovered the ability to use hikvision software
* Retrieved Wifi SSID and Wifi password from config file
  
**Android App Pentest:** Summary of outcomes
* Outcome1
* Outcome2

## Hinderances
Not digging more into the SDDP packets right away led to not making the discovery that hikvision software can be used on the ezviz cameras.

## Ongoing Risks
(address your project risks identified from Milestone 1 and update them based on your current progress, this should be a table)
