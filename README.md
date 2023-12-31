# Homelab for Detection and Monitoring


<b> In this project, I will be constructing an environment where I can implement and test various security concepts in a practical and safe space. I will walk through the process of configuring, optimizing, and securing an I.T. infrastructure at a small scare. </b>


<b> The goal will be to gain real-world knowledge and practice. </b>


## Content

* Breakdown
* Installing pfSense firewall for Network Segmentation & Security
* Configuring Security Onion
* Configuring Kali & pfSense Web Interface
* Configuring a Windows Server as a Domain Controller
* Configuring Windows desktops
* Configuring Splunk

## Breakown

For the context of this project, I will be using VMware to create the virtual machines that will host the operating systems and services involved.

I will be using a `Kali` machine as an attack machine. It will be used to send attacks against our `victim network`. 

To monitor these attacks I will be using `Security Onion`. It will serve as an all-in-one IDS, Security Monitoring, and Log Management solution. I will use a SPAN port to monitor traffic using Security Onion.

I will be configuring `pfSense` to serve as a firewall, router, and DHCP server, and network segmentator. It will ensure interconnectivity between the different networks.

The `victim network` will mainly consist of a domain controller and one or two client devices.

Although Security Onion will work as a SIEM, I will also be configuring `splunk` for additional practice using it.

![Topology](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/0b241074-ebd4-4226-8dc9-41c66a6ea9ed)


## Installing pfSense

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


## Configuring Security Onion

* In this section we will once again be creating a new VM, this time mounting the Security Onion ISO. For the Operating system version, I chose `CentOS 7 64-bit`.

![SO 1](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/a012ed2d-43ad-43e7-86b2-dee72393ff10)

![SO 2](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/cb8081ae-101a-4b7d-b51e-e924f1893233)

* I added two additional network adapters. One of them connects to pfSense via virtual network 4 (VMnet4). The other one will be used to monitor traffic via the SPAN port using VMnet5.

![SO 3](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/90b72196-92ac-42fb-bb1b-d1e922057f88)

* Then I went through the standard Security Onion setup.

![SO 4](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/f9bed30c-93f3-4f21-9707-81f182b99a98)

![SO 5](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/a2a29d3d-26cc-4c79-bf6f-a830eb57fb76)

* I made sure to note the `Management IP` since I will be using it to access the Web Configurator. The IP in this case was `192.168.9.129`.

![SO 6](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/db922864-580a-40b1-9de8-2ae1b780fcf3)

![SO 7](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/3d86f904-e34e-4ba5-96e7-e56b727431c4)

* Once Security Onion was set up I created a new VM running Ubuntu. This machine will have access to Security Onion and it will simulate a SOC/Security Analyst accessing a SIEM or any security tool from their device.

![UB 1](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/3dc8a161-bf66-462c-8873-334dc2960d50)

![UB 2](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/bca06209-8ba8-4063-a209-e3cd40b43ebb)

![UB 3](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/5ed532f0-c4a3-42e0-8fba-f2a86e2c12db)

* After the Ubuntu setup finished, I opened the command line and installed `net-tools`. I then ran `ifconfig` to determine what the IP of this Ubuntu machine was. In this case, it was `192.168.9.130`. I will be whitelisting this IP Address within the Security Onion interface to allow this Ubuntu machine to manage Security Onion remotely using the web interface.

![UB 4](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/f1c33cb1-b3ea-49c5-a777-faf84f36a235)

![UB 5](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/89c4a8a3-56e1-462a-8c71-253d85fc07c5)
  
* I forgot to get a screenshot of the process of adding a new firewall rule to Security Onion, but the steps are as follows:
* 1) I navigated back to the Security Onion VM and logged in with my credentials.
  2) I ran `sudo so-allow`.
  3) When prompted to choose a role for the IP or Range I chose `a` for `Analyst`.
  4) Finally, I added the Unbuntu instance IP address to the rule list.
 
* With this new firewall rule created I was able to access the SO web interface from the Unbuntu machine.

![UB 7](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/19342f0d-37a9-4a3c-b321-33702854c1f0)

![UB 8](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/9e625f98-fd86-4cea-b245-f5913af11ba0)

![UB 9](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/a56e9166-cac8-4b60-b952-4d888dbcf9d4)

## Configuring Kali & pfSense Web Interface

* Installing Kali was straightforward since I used a VMware image. The only modification I made was adding an additional network adapter.

![KL 1](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/8e0eafff-663d-4ce8-8c45-50a1768a13cc)

![KL 2](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/9b962540-8fae-4fed-bf84-f11e452a91d1)

* Using the Kali machine I navigated to `192.168.1.1` in order to use the pfSense web interface.

![KL 3](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/071c807f-9ddc-4b5e-937d-37a556ba8c4e)

![KL 4](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/23b34597-733b-4c1e-9197-c003b5321146)

* I assigned the DNS.

![KL 5](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/b127d229-ccc0-4800-b182-b4c6750d0bf4)

* I also turned off the following security features in order to make the victim network more susceptible to attacks of different kinds.

![KL 6](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/29e83cc0-598f-4668-9921-89d00e550318)

![KL 7](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/2f732746-44d2-4aee-bf48-4c2adea43fd0)

* I then modified the names of the different interfaces to make them easier to recognize.

![KL 8](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/6c166f6c-8637-4cf4-a044-a2da22893a84)

![KL 9](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/d5c18508-3698-49ec-bbaf-72fae730a651)

* I created a bridge, added the victim network to it, and selected the Span Port.

![KL 10](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/a440a6a4-5f34-4c0b-8caf-9405c2978ac5)

![KL 11](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/b6328e31-09f5-43d7-acbe-04d214d1c9a4)

* Finally, I created a new firewall rule to allow traffic from `any` protocol. This should give me some interesting logs and alerts later on.

![KL 13](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/c618c314-dc72-4d80-9c19-40fa35f4b569)


## Configuring the Domain Controller

* For the next section I will be setting up the Domain Controller which will be running on a Windows Server 2019 VM.
* I started by creating the VM and assigning it to the VMnet3 network.

![DC-1](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/825da6ec-e4a3-4d3d-96c6-1265266876b1)

![DC-2](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/250d214e-329c-46a5-abb9-dbb902ff77f7)

* Then I proceeded with the standard Windows Server setup, making sure to select one of the Desktop Experience versions.

![DC-3](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/2f6db01d-b164-434f-b6ac-5c2a4360a2a2)

![DC-4](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/976b9dfe-d79c-414f-9f36-adfa5d0d413b)

![DC-5](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/046665b7-6a46-426d-b8f8-cf32ca41529f)

* Once the setup was done I changed the computer's name.

![DC-6](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/c99146a2-e7c8-47d5-b90b-1b335b08d9e6)

* I then proceeded to add the Roles and Features necessary. I began by installing the `Active Directory Domain Services` role.

![DC-7](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/37306072-7a51-4e0a-bfbb-2850753e34a1)

* Then I created a new domain (forest) called `homedomain.local`.

![DC-8](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/4bd30241-0399-4e22-9558-ecc599618f8d)

* I also needed to create a `Certificate Authority` so I added the `Active Directory Certificate Services` role.

![DC-9](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/6d5787c1-60e7-4916-839f-6174971cb4ab)

* Then I added the CA to homedomain.local.

![DC-10](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/438202e9-d747-499b-b79e-f693b2ff02c4)

![DC-11](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/d3bad1c4-f267-4282-9385-3b3d60114764)

![DC-12](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/b5063853-817b-41d7-80b7-7c6db853c109)

* Using AC DS I added a new user to the domain which will serve as our victim later on.

![DC-13](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/3c445b23-cd5d-4f2d-a4db-7cf331abd9bb)

* **For the purposes of this lab** I turned off the Windows Firewall. This is a specific use case for this project in order to view how different attacks are logged later on. 

![DC-14](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/6fc50344-cea0-4191-ba56-53dfc63408be)

* Lastly, I changed the IP address settings for the DC. I designed the pfSense firewall as the Default gateway as well as the DNS server.

![DC-15](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/fa052d19-d1ad-4081-ae71-955250fcbdab)

## Configuring the client device

* Like all the previous steps, I began by creating a new VM that will host Windows 10. I made sure to assign the the VM to the proper virtual network

![CL-1](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/09107ba7-0ce1-483d-bc62-815203a50ac0)

![CL-2](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/4aecc2d6-1150-4cc5-a393-31f33ab8bd54)

* This device will be used by the user we created in the previous section while setting up the DC

![CL-3](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/efa898de-ee52-4ca5-bd94-f4fcf3e7895b)

* Once the OS was done installing I manually assigned the IPv4 properties.
* This device is on the VMnet3 so the default gateway is `192.168.2.1` and the DNS server is `192.168.2.10` which is the DC (I will most likely attempt to automate this process in the future to seamlessly add new clients.)

![CL-4](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/1887475f-1b9e-496a-a93f-ec9c4098230f)

* I then proceeded to change the name of the device and add it to the domain.

![CL-6](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/ae610608-e712-4390-b139-a74b84c99531)

![CL-5](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/09ba021c-191f-4183-9966-47c6e71d9563)

![CL-7](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/c163e633-1c83-4d07-b215-0afe57fc6b60)


## Installing Splunk

* I began the next section by creating a VM that hosted an instance of Ubuntu Server. I added an additional Network Adapter and assigned it to `VMnet6`.

![SP-1](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/7bdb9a92-1e15-4702-aa54-d62337b9e764)

![SP-2](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/f6fa5cfc-5264-416f-9256-7b720aea6cc2)

* I then proceeded with the standard Ubuntu Server installation.

![SP-3](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/796b1168-f1a8-4619-ad40-863440a241fb)

![SP-4](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/d25098aa-4711-4ff6-b0ab-050e9ce0b48a)

![SP-5](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/a615c113-7359-43e4-a456-661589cf0a64)

![SP-6](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/d7ece367-9ce4-49d7-8f99-8af0687eb501)

* To simplify the process I decided to install a GUI for Ubuntu Server. In order to do this I began by installing `tasksel` using the CLI.

![SP-7](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/4d79b1d9-91a3-4583-ae40-1d62fcb57410)

* Then I used tasksel to install Ubuntu Desktop

![SP-8](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/fcbc8a62-c7e9-4241-a405-eb1c57e8918f)

![SP-9](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/a5a601bb-9e7e-492e-a629-798047b1c13a)

* Next, I downloaded the Splunk installer from the official Splunk website and unpacked it.

![SP-10](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/73ba2a2a-932b-4941-bfe0-827b2c32f5da)

* Once unpacked, I navigated to the `bin` folder and started the Splunk service.

![SP-11](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/bdf35c6f-3769-494c-b06a-9fd2c8025f33)

![SP-12](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/58447877-699d-4965-9e10-ae6bf9f1f0c8)

* Finally, I navigated to the Splunk web interface address, which in this case was `http://splunk:8000`, and confirmed the service was up and running.

![SP-13](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/ac0b94fa-0ce6-449d-bb01-f0259681cb2b)

![SP-14](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/92b4edad-a7e8-4e8e-91c2-600adcdf6e37)

## Configuring Splunk

* After installing Splunk I began the configuration to forward and receive logs. I first started by enabling the port to listen on which in this case is `9997`.

![SP-15](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/928591ec-bf31-447c-9573-14337fc9383e)

* Then I created an `index` that would aggregate data coming from the Domain Controller. I named the index `wineventlog`.

![SP-16](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/a4b1b89d-fb20-4b6e-a54a-6e43037f6084)

* I returned to the DC and began the installation of `Splunk universal forwarder`.

![SP-17](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/fae00a87-9adc-4e9b-8d75-80c0adcd7341)

![SP-18](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/ef94a2c4-4aeb-49f4-a731-00369243f194)

* Afterwards I returned to the Splunk web interface to add the universal forwarder as a data source.

![SP-19](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/c4ed70b2-9ad4-4c99-a65a-f0a0e9b3a29a)

![SP-20](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/e0e0bfef-94fa-4e46-b964-65acf6f7af6a)

![SP-21](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/6ce6a856-41d8-4f00-aaae-63ddec653a9d)

* And finally, I began searching to confirm that Splunk was properly receiving the events.

![SP-22](https://github.com/royzen01/HL_Detection_and_Monitoring/assets/13005742/ff260d10-1675-43df-b5c4-edfd70e2d58f)

# Conclusion

I undertook this project because I wanted to get some hands-on experience building a topology from a security perspective. My goal behind this was to create something that was unique but also functional. Of course, the scale of this project doesn't compare to any real-world scenario, but it did give me great insight into how a company might structure its private network. I still have much to learn but I find that this is a great start to having functional cybersecurity experience.

# Going forward

As I continue my journey as a cybersecurity professional I will be utilizing a variety of tools and services. Through the use of this home lab, I can practice and hone my skills in a safe but realistic environment. I will be diving into concepts such as Intrusion detection, and vulnerability scanning and I might even try out some Red Hat tactics.


