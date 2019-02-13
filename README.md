# Executive Project Summary
In the modern age, Internet of Things (IoT) devices are becoming more and more common. One portion of this growing market includes security cameras. Through research and development, Wi-Fi enabled cameras have become quite affordable. They also have a growing list of features that are attractive to consumers. Some examples of the convenient tools available include remote two-way communications, remote access to live video, integration with Alexa and IFTTT, cloud storage, SD storage, and much more.  
  
Our main goal is to perform a penetration test on an EZVIZ CTQ2C 720p security camera. Our scope will include testing the vulnerabilities of the camera itself, along with utilizing other devices to attack it over the network. These can be the smartphone companion app, web app, and other IoT devices. We will not be directly pen testing the official servers that host the cloud service and allow remote camera access.  
  
A compromised security camera can lead to unwanted spying and the patterning of tenants' activities. If remote access is achieved by a malicious individual, the user could be spied on without ever knowing it. In the modern day of sacrificing security for convenience, it's important for consumers to remain aware of the dangers around them. To help people best defend themselves, we aim to present a report of our findings and an establishment of best practices for securing IoT cameras.

Illegal camera surveillance has adverse effects on society, from the perspective that people feel their privacy is ever invaded via camera systems. Generally, the fact that people are becoming more computer literate cases of hacking cameras are rapidly being common. Industries like the hotels and surveillance units suffer greatly. An instance of the hotel sector, once their customers’ privacy is invaded through camera hackings, the hotels credibility is lost, thus they end up losing financially. 

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
* Through understanding the vulnerability trail, users are given the power to recover their lost data

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
|Cannot attack via IOS/Android Apps (15) | 3 | 5 | There may be no vulnerabilities alllow us to get access to the camera via the phone apps |

# Project Methodology
## Literature Review
|Resource  | Author(s) | Importance | Signifcance to the group |
|-------------------|---------|---------------------------|-------------|
| https://github.com/OWASP/owasp-mstg#android-testing-guide | OWASP | There are in-depth guides on testing mobile device applications based on the operating system  | This is useful to the group because the device we are testing has an application on both android and iOS|
| https://arxiv.org/ftp/arxiv/papers/1709/1709.05742.pdf | Guri, Mordechai, Dima | Touches on how attackers use infrared waves to read signals from cameras remotely. The main mean of attacks discussed is exfiltration and infiltration.  | It’s useful to the group since the EZVIZ CTQ2C 720p security camera can be penetrated both internally and externally. |
| http://s3.eurecom.fr/docs/trusted16_costin.pdf | Costin,Andrei | There is detailed explanation on various attacks styles like steganography on surveillance systems and how to mitigate them  | The information is beneficial to the study in the sense that it gives an insight on how to overcome some of the vulnerabilities that might arise from the stud of the EZVIZ CTQ2C 720p security camera |
| https://patents.google.com/patent/US7188513B2/en | Wilson,Marshall | Shades light on the new age issue whereby people live oblivious of the security threats they are exposed via systems like surveillance cameras.  | Provides essential material that helps the group in educating people on the possible threats they endure by neglecting proper security measures when they use systems, with an example of the EZVIZ CTQ2C 720p security camera |
| https://bit.ly/2Gm8IX9 | Friedewald | Covers on how the society is affected by the surveillance and privacy issues. | The material is important since it helps in highlighting how the society is affected by the security breaches on the surveillance systems they install for their private use. |
| https://bit.ly/2E6c3qW | Rajpoot, Qasim Mahmood, Christian | Covers on how the legal sector is coming in to counter attack the growing vice of breaches through camera systems.  | Material is important as it helps explain how demographic intervention promotes change on technological lawlessness. |
| https://arxiv.org/ftp/arxiv/papers/1802/1802.03110.pdf | Wei Zhou, Yuqing Zhang, and Peng Liu | In length discussions on IoT, and especially its efficiency and possible breaches. | Since the EZVIZ CTQ2C 720p security camera is an IoT technology most of the possible vulnerability routes affecting it are mentioned. |
| https://bit.ly/2DvLZUA | Berglez, Regina, reinhard | Explains on how to enhance security when using surveillance systems. | It has some possible schemes that can come in handy to explain how security cameras can be used without fear of being hacked. |

## Technical Plan
Misuse cases (attack vector/space)

One threat space that we will attempt to use to gain access to the EZVIZ security camera are the smartphone applications, this includes the android or iOS applications, that can be used to setup the device as well as watching the live feed from the camera. In order to do this we will be using OWASP's penetration testing standard. This standard has instructions how to setup a testing enviornment for each operating system and suggestions how to test the security of android applications. If we are able to find a vulnerability within the aplication we will try to leverage it to gain access to the security camera.

In this study the group will attempt to access EZVIZ CTQ2C 720p security camera. To gain access to the camera, we will attempt getting its IP address though a web application called the angry IP scanner. Follow the steps as arraigned in Wei Zhou text. If any vulnerability is found, the test results will be recorded as further investigations are conducted.


# Resources/Technology needed
|Resource  | Dr. Hale needed? | Investigating Team member | Description |
|-------------------|---------|---------------------------|-------------|
|EZVIZ CTQ2C| no | Everyone | this allows everyone to do independent research |
| SD card | no | Everyone | Needed for the camera to be able to store recordings, does not come included with device |
| Iphone | no | Mohammed & Khalid | Needed to investigate the iOS app |
| Android Device | no | Jose & Thomas | Needed to investigate the android app |
| Burp suite | no | Christian | Needed to test the webapp |
| Server Space | no |Christian | Needed to test MITM attacks|
| Wireshark software | no | Mohammed & Khalid | Software used to monitor network traffic |
| Workstations and writing materials | no | Everyone | For recording test results during practical |

# First Sprint Plan
Kanban: located in the projects tab. 
