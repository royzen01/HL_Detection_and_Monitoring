# Homelab for Detection and Monitoring


<b> In this project, I will be constructing an environment where I can implement and test various security concepts in a practical and safe space. I will walk through the process of configuring, optimizing, and securing an I.T. infrastructure at a small scare. </b>


<b> The goal will be to gain real-world knowledge and practice. <b/>

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

