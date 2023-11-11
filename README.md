# Homelab for Detection and Monitoring


<b> In this project, I will be constructing an environment where I can implement and test various security concepts in a practical and safe space. I will walk through the process of configuring, optimizing, and securing an I.T. infrastructure at a small scare. </b>


<b> The goal will be to gain real-world knowledge and practice. </b>

## Content

* Configuring pfSense firewall for Network Segmentation & Security
* Configuring Security Onion
* Configuring Kali Linux as an attack machine
* Configuring a Windows Server as a Domain Controller
* Configuring Windows desktops
* Configuring Splunk
* Ubuntu/CentOS/Metasploitable/DVWA/Vulnhub machines: All these are potential Linux machines that can be added to the network for exploitation, detection, or monitoring purposes.

## Breakown

For the context of this project, I will be using VMware to create the virtual machines that will host the operating systems and services involved.

I will be using a `Kali` machine as an attack machine. It will be used to send attacks against our `victim network`. 

To monitor these attacks I will be using `Security Onion`. It will serve as an all-in-one IDS, Security Monitoring, and Log Management solution. I will use a SPAN port to monitor traffic using Security Onion.

I will be configuring `pfSense` to serve as a firewall, router, and DHCP server, and network segmentator. It will ensure interconnectivity between the different networks.

The `victim network` will mainly consist of a domain controller and one or two client devices.

Although Security Onion will work as a SIEM, I will also be configuring `splunk` for additional practice using it.

![Topology](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/0b241074-ebd4-4226-8dc9-41c66a6ea9ed)


## Configuring pfSense

* We begin by opening up VMware, creating a new virtual machine, and mounting the pfSense ISO.

![CW 1](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/b327b480-84c5-4df3-a709-1fc0a8db3b87)

![CW 2](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/c1c8eeba-f12f-445d-8874-4e3a10f4d212)

![CW 3](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/65315a54-8afc-4b47-8c49-848046a0afc5)

* I added 5 additional network adapters and assigned each of them to a virtual network.

![CW 4](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/e79040c8-d24f-4209-b65a-804914c32dbf)

* I then proceeded with the standard pfSense installation.

![CW 5](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/39d70efd-a555-49fa-9cd4-77f5cf55ad35)

![CW 6](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/0f426fbf-d755-49da-8139-a6edafdffc46)

* Once pfSense finished installing I configured the network interfaces I created during setup to the interfaces within pfSense. These interfaces will be further configured using the pfSense WebConfigurator.

![CW 7](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/aaaf9543-c500-4035-8ba3-4c6a36fa9c01)

![CW 8](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/af50c5e8-8f75-42fc-93c5-8871a198dc4d)

![CW 9](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/e27200b5-b2f0-4e92-a980-b26b67925097)

* Then I assigned an IP address to the LAN interface, turned on DHCP on it, and gave it an IP scope.

![CW 10](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/9be75d75-873f-49d2-8b25-4b4793ab1c22)

* I finalized the pfSense setup by assigning each interface an IP address. Note: em4 did get an IP address since this will be used as a SPAN port later on.

![CW 11](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/762464df-ea03-457b-9278-09908b02911c)







