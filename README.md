# Executive Project Summary
In the modern age, Internet of Things (IoT) devices are becoming more and more common. One portion of this growing market includes security cameras. Through research and development, Wi-Fi enabled cameras have become quite affordable. They also have a growing list of features that are attractive to consumers. Some examples of the convenient tools available include remote two-way communications, remote access to live video, integration with Alexa and IFTTT, cloud storage, SD storage, and much more.  
  
Our main goal is to perform a penetration test on an EZVIZ CTQ2C 720p security camera. Our scope will include testing the vulnerabilities of the camera itself, along with utilizing other devices to attack it over the network. These can be the smartphone companion app, web app, and other IoT devices. We will not be directly pen testing the official servers that host the cloud service and allow remote camera access.  
  
A compromised security camera can lead to unwanted spying and the patterning of tenants' activities. If remote access is achieved by a malicious individual, the user could be spied on without ever knowing it. In the modern day of sacrificing security for convenience, it's important for consumers to remain aware of the dangers around them. To help people best defend themselves, we aim to present a report of our findings and an establishment of best practices for securing IoT cameras.

## Goals and objectives
* Build experience practicing skills learned in coursework
* Pentest camera with a variety of methodologies
    * Intercept video/remotely control device
    * Test if vulnerable to spoofed network
    * Create a botnet using multiple cameras  
* Examples of useful types of attack for this project
    * Trojan horse attack, this attack uses (RAT) and provides hidden access.
    * Clickjacking attack.
* Deliver best practice guide, highlighting vulnerabilities to minimize threat space

## Merit of the Project
* With IoT cameras gaining popularity, finding and sharing weaknesses so they can be patched is beneficial to everyone  
* It is important for IT companies and individuals to know that how to protect their assets during the current revolution in the IT industry when their cameras are connected to the interent. 
* If no vulnerabilities are found, we can add comfort to the idea of consumers trusting these devices

# Proposed Project Timeline
Are the 2 below the same or different? If so, how?

## Tasks and Expected Completion Time
Table format

## Gantt Chart
Gantt chart format

# Project-oriented Risk List
|Risk name (value)  | Impact     | Likelihood | Description |
|-------------------|------------|------------|-------------|
|Brick security camera (30) | 6 | 5 | It's possible that we may brick the security camera while trying to gain access to it via the hardware |
|Corrupt micro SD card (20) | 4 | 5 | It's possible that we may corrupt an SD card while attempting to gain access to the device using the SD card |
|Team member being unavailable/unwilling to help (32) | 8 | 4 | There may be a loss in productivity if one or more team members are unable to cooperate |
|Cannot attack via network (15) | 3 | 5 | There may be no network-based vulnerabilities |
|Cannot attack via micro SD card (15) | 3 | 5 | There may be no way to attack through the SD card |
|Exploit unathorized servers (40) | 10 | 4 | We could possilbly obtain a cease-and-desit order and stop the porject |

# Project Methodology
## Literature Review
|Resource  | Author(s) | Importance | Signifcance to the group |
|-------------------|---------|---------------------------|-------------|
| https://github.com/OWASP/owasp-mstg#android-testing-guide | OWASP | There are in-depth guides on testing mobile device applications based on the operating system  | This is useful to the group because the device we are testing has an application on both android and iOS|

## Technical Plan
Misuse cases (attack vector/space)

One threat space that we will attempt to use to gain access to the EZVIZ security camera are the smartphone applications, this includes the android or iOS applications, that can be used to setup the device as well as watching the live feed from the camera. In order to do this we will be using OWASP's penetration testing standard. This standard has instructions how to setup a testing enviornment for each operating system and suggestions how to test the security of android applications. If we are able to find a vulnerability within the aplication we will try to leverage it to gain access to the security camera. 


# Resources/Technology needed
|Resource  | Dr. Hale needed? | Investigating Team member | Description |
|-------------------|---------|---------------------------|-------------|
|EZVIZ CTQ2C| no | Everyone | this allows everyone to do independent research |
| SD card | no | Everyone | Needed for the camera to be able to store recordings, does not come included with device |
| Iphone | no | Mohammed & Khalid | Needed to investigate the iOS app |
| Android Device | no | Jose & Thomas | Needed to investigate the android app |
| burp suite | no | Christian | Needed to test the webapp | 

# First Sprint Plan
Kanban: located in the projects tab. 
