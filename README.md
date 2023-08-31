<div id="top"></div>

<!-- PROJECT LOGO -->
<br>
<div align="center">
  <a href="https://github.com/Reda96R/Sieve-of-Eratosthenes">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

<h1 align="center">::: NetPractice_42 :::</h1>
</div>

# Table of Contents
1. [Project Overview](#project-overview)
2. [Background Theory](#background-theory)
   1. [What is TCP/IP?](#what-is-tcpip)
   2. [TCP/IP](#tcpip)
   3. [OSI](#osi)
3. [Subnetting](#subnetting)
   1. [IP Address](#ip-address)
   2. [Subnet Mask](#subnet-mask)
   3. [Making a Subnet](#making-a-subnet)
4. [Acknowledgement](#acknowledgement)
5. [Resources](#resources)

`This project is a general practical exercise to let you discover networking.`
![[unplugged.png]](https://github.com/Reda96R/NetPractice/blob/main/images/unplugged.png)
# Project overview:
In this project we'll have to configure some small scale networks, so It would be wise to first understand TCP/IP addressing, and it wouldn't hurt to also have a look at OSI, but what the heck is TCP/IP or even this OSI thing? first let's back up a little and understand how networking works and how computers work.
# Background theory:
let's say we want to connect two computers so they can share data, the easiest way is to  plug those suckers with an ethernet cable and voila we have a newborn called **Networking**, but let's say another computer wants to join the party, now we'll need something called a switch (or a hub because data leaks are true communal experience) , now all the computer plugged to that switch form one **Network**, let's call it network0. now let's say in another universe network1 is running and one of the computers in that network wants to contact another in network0, how can that happen?, well that's where routers come in, all we have to do is to connect our two switches to the routers and by that we're connecting both networks, scale up this example and multiply these networks many time and you'll have this thing called the internet. this is a very general look of how networking works, so keep in mind that we're just touching the tip of the iceberg if you want to have a look on how devices talk across the network and networking in general I highly recommend NetworkChuck's free CCNA [course](https://www.youtube.com/playlist?list=PLIhvC56v63IJVXv0GJcl9vO5Z6znCVb1P).
## What is TCP/IP?
So here's a little history lesson, back in the days connecting two devices and sending data across was a thing from the future (literally), so **ARPANET** ( Advanced Research Projects Agency Network) came to the rescue, it was designed primarily as an experimental network to test the concept of packet switching, a revolutionary communication method where data is broken into packets and sent separately across a network, then reassembled at the destination, but during the 60's and early 70's. computers were expensive, bulky, and had limited processing power and memory and creating a large-scale network of such computers would have been impractical due to the resource constraints, I mean take a look at ARPANET's logical map,

![[arpanet.png]](https://github.com/Reda96R/NetPractice/blob/main/images/arpanet.png)

so many universities and institutions were significant players in ARPANET's creation, now that we have our base system many companies like IBM made there own networks and that would make problem of compatibility, each company's network can't connect to other's networks, and to deal with that **models** were made, they came to define how devices and networks should connect, and those models are **TCP/IP** and **OSI**.
## TCP/IP:
 Transmission Control Protocol/Internet Protocol or TCP/IP, it came before the OSI model and it has five layers, wait what's a layer?! a **layer** simply refers to a distinct level of functionality and abstraction within the networking stack, still no idea what I'm talking about? okay let's make it more simple, Imagine the TCP/IP as a cake with different layers, each layer has its own flavors and functions, and we have five of those which are represented in the following illustration,
 
 ![[TCP_layes.png]](https://github.com/Reda96R/NetPractice/blob/main/images/TCP_layes.png)
### Physical layer:
This is the bottom of the cake, it all starts here where we can find the actual physical connection between devices, like plugging in cables and sending data in packages.
>Sometimes the Physical layer can combine both the Physical and the Data link Layer 
### Data link layer:
Some call it **Network interface layer**, it is where the packet of data gets formatted and prepared to be transported and routed through the network. this is where MAC addresses live, aka switcher friends.
### Network layer:
This is the step where the data gets a header and a trailer, and these tell the data where to go, kind of like giving directions to a delivery person. this is the home of IP addresses, aka router friends.
### Transport layer:
In this layer the data gets encoded so it can be transported through the internet using either the [User Datagram Protocol (UDP)](https://www.fortinet.com/resources/cyberglossary/user-datagram-protocol-udp) for faster, connectionless communication or TCP for reliable, connection-oriented communication, Imagine it like a postal service ensuring that your packages arrive intact and in the correct sequence.
### Application layer:
This is the top layer, like the icing on the cake, it's where different apps and programs communicate with each other over the network. like you using apps for chatting, browsing, or sending emails.
## OSI:
for the Open Systems Interconnection (OSI) model the first four bottom layers all stay the same as the TCP/IP model, but before the Application layer we have two extra layers,
### Session layer:
Think of this layer as the conversation you have with someone while writing a letter. It manages the back-and-forth of information between your computer and the other computer, making sure they both understand each other.
### Presentation layer:
Imagine you're sending a gift along with your letter. This layer wraps your gift and prepares it in a way that the recipient can easily open and understand. It might involve translating the message into a language the other person understands or compressing files to save space.
then comes the Application layer that we've seen in the TCP/IP model.

The TCP/IP model is the most widely used networking model in practice today. While the OSI model provides a theoretical framework for understanding networking concepts, the TCP/IP model is more practical and closely aligned with the way the internet and modern networks actually operate.
Now that we have a good understanding of how networking works, let's get the meat of the whole project, **subnetting**.

## Subnetting:
Subnetting is a technique used in networking to divide a larger network into smaller, more manageable segments called subnets. This helps in optimizing network performance, managing IP addresses efficiently, and improving security. Subnetting is primarily associated with [IPv4](https://en.wikipedia.org/wiki/Internet_Protocol_version_4) addresses, as [IPv6](https://www.internetsociety.org/deploy360/ipv6/) has a much larger address space that often negates the need for subnetting. but why we need subnetting? Imagine you have a big office building with many departments. It's easier to manage and control access if you divide the building into smaller sections. Similarly, in networking, when you have a large network with many devices, dividing it into smaller subnets makes it more organized and manageable.
### IP address:
For us we'll just focus on IPv4 IP addresses. An IPv4 address is a 32-bit numerical value, typically represented as four sets of numbers separated by periods (e.g., 192.168.1.1). Each set of numbers is called an "octet," and each octet represents 8 bits.
it is divided into two parts: the network part and the host part. Subnetting modifies how these parts are used.
#### network part :
The network part of an IP address is used to identify the network itself. It's like the street address of a neighborhood. All devices within the same network share the same network part of their IP addresses. This allows routers to determine if a device is on the same local network or if it needs to be reached through routing to a different network.
#### host part:
The host part of an IP address is used to identify individual devices within the same network. It's like the apartment number in a building. Devices with different host parts can still be part of the same network, but they are distinguished from each other by their unique host part.
but how on earth are we supposed to distinguish between those parts? this is where something like **Subnet Mask** becomes handy.
#### subnet mask:
A subnet mask is often written using a format similar to an IP address, but with "1" bits representing the network part and "0" bits representing the host part.
let's have an example to have a better understanding, 
``` 
IP address   --->   192.168.1.1
subnet mask  --->   255.255.255.0
```
here we have our IP address and subnet mask, let's convert our subnet into binary, if you dared to ask why, then you might be a bit off the mark on that one, my friend.
```
IP address   --->   192.168.1.1
subnet mask  --->   11111111.11111111.11111111.00000000
```
notice here the part where we have all zero's that's our **host part**, the rest is **network part**, conventionally **192.168.1** is for the network, and the last **1** is the host part.
### Making a subnet:
Before we can make a subnet, we need to find the 7 attributes of our subnet,
- **Network ID**           --> the first IP address in the subnet;
- **First Host**             --> IP address after the network ID;
- **Broadcast IP**        --> the last IP address in the subnet;
- **Last Host**             --> the last IP address in the subnet;
- **Next Network**      --> IP address after the broadcast IP;
- **IP address numb** --> number of the IP addresses in the subnet;
- **CIDR/Mask**           --> converting between CIDR notation and subnet mask.
In order to find all seven attributes we're going to utilize a cheat sheet that you can find in this [link](https://nsrc.org/workshops/2009/summer/presentations/day3/subnetting.pdf), or you can construct a simple version of it by following the steps explained  in this [video](https://www.youtube.com/watch?v=ljS07YTEJ2I&t=5s), I''ll just work with the simple version, 

![[cheat_sheet.png]](https://github.com/Reda96R/NetPractice/blob/main/images/cheat_sheet.png)

let's say our network looks something like this: **10.1.1.55/28**
**1** - we need to find the CIDR, in our case it is /28;
**2** - now we should start with **.0** and increase it by the group size until we pass the target IP, that means we're going to increase .0 by **16** until we pass **.55** it should look like this: 
.0  ---> .16 
.16 ---> .32 
.32 ---> .48
.48 ---> .64
**3** - now it is time to fill our attributes,
- **Network ID**           --> 10.1.1.48/28, the IP before our target IP (.55);
- **First Host**             --> 10.1.1.49/28, IP after the network ID;
- **Broadcast IP**        --> 10.1.1.63/28, the last IP in the subnet (.64 is the next networks's ID);
- **Last Host**             --> 10.1.1.62/28,the last IP in the subnet;
- **Next Network**      --> 10.1.1.64/28,  IP after the broadcast IP;
- **IP address numb** --> 16 (14 usable), the group size;
- **CIDR/Mask**           --> /28 / 255.255.255.240;
If you didn't get it I highly recommend watching this [video](https://www.youtube.com/watch?v=5-wlfAdcmFQ&t=130s) as it goes through all this steps in details.

# Acknowledgement:
If you came up this far, well my friend congratulations that's all you need to start tackling the first levels of the project, but that doesn't mean that we've seen it all, in this short explanation I just brought to your attention some of the general concepts of subnetting and it is meant to give you a little kick start to get you going on the project, so you might need to do your own research to gain a more comprehensive understanding of subnetting and its intricacies.
Happy subnetting = ) .

<p align="right">(<a href="#top">back to top</a>)</p>

# Resources:
- https://www.practicalnetworking.net/stand-alone/subnetting-mastery/
- https://www.calculator.net/ip-subnet-calculator.html
- https://www.youtube.com/playlist?list=PLIhvC56v63IJVXv0GJcl9vO5Z6znCVb1P

