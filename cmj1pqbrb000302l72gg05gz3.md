---
title: "what are packets"
seoTitle: "what is packet in networking"
seoDescription: "what is packet in networking"
datePublished: Thu Dec 11 2025 17:29:19 GMT+0000 (Coordinated Universal Time)
cuid: cmj1pqbrb000302l72gg05gz3
slug: what-are-packets
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765474057017/79229a86-681b-4377-b301-b9b05bc17313.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1765474122436/ddfc0b35-6b8f-4afc-b642-1f5a52a9987a.png
tags: computer-networks, cn, packet-tracer, packet-switching

---

In computer networks, this is one of the most asked and fundamental topics. Let me break it down with a simple example.

Suppose you are sending a video on WhatsApp to your friend. The network you are using is the same network that many other people are using at the same time. There is no fixed path that guarantees your video will travel directly and reliably to the right person. If a disturbance happens in the middle while sending one big chunk of data, the entire transfer can fail and you would have to send the whole video again

It’s similar to talking on an old wired telephone with a private, end-to-end wire connection. If that wire gets disturbed, the whole communication breaks.  
But in reality, networks are shared—many users, many routes, lots of traffic.

This is where **packets** come in.

Instead of sending the entire video as one big block, the network **breaks the video into many small chunks**, called **packets**.  
Each packet contains:

1. A small part of the video
    
2. Metadata that describes how and where it should be delivered
    

Think of it like sending a parcel to your friend. You attach important details like the **address**, **sender**, **receiver**, and sometimes **weight**. Without metadata, the parcel wouldn’t know where to go. The same idea applies to packets.

## Metadata inside a packet

Every packet includes three major components:

**1\. Header**

This contains control information such as:

* Source address
    
* Destination address
    
* Sequence number
    
* Protocol
    
* TTL
    
* Checksum
    

The header tells the network **where the packet came from**, **where it needs to go**, and **how it should be handled**.

**2\. Payload**

This is the **actual data**, the small part of your video being transmitted.

**3\. Trailer**

This usually contains error-checking information, such as CRC, to verify whether the packet was delivered correctly.

By breaking the video into packets, the network can send each small chunk independently through different routes. If one packet gets lost, only that packet needs to be resent—**not the entire video**. This makes communication fast, reliable, and scalable, even when millions of people share the same network.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765473355204/9b9af716-46ef-4e73-8a08-cd59e0762b93.png align="center")

## IPV4 Packet Headers

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765473679430/5c902e7d-48a2-41ee-9fbd-484eed1f713f.png align="center")

## Interview Question

1\. What is a packet?

Explain the definition and why networks use packets instead of sending data as one large block.

2\. What are the components of a packet?

Expected parts: Header, Payload, Trailer.

3\. What information is stored inside a packet header?

Source IP, Destination IP, TTL, Protocol, Checksum, Sequence Number, etc.

4\. What is the payload in a packet?

Explain that it's the actual user data (video chunk, message, file part).

5\. What is the role of the trailer in a packet?

Error detection, usually CRC or checksums.

6\. Do packets always take the same path to reach the destination? Why or why not?

Explain dynamic routing + path independence.

7\. What happens when a packet gets corrupted or lost during transmission?

TCP → retransmits  
UDP → packet lost permanently

8\. What is TTL (Time-To-Live) in a packet and why is it important?

Prevents infinite loops.

9\. What is packet fragmentation? Why does it occur?

Triggered when packet size &gt; MTU; packet is split into smaller fragments.

10\. Can packets arrive out of order? How is this handled?

Yes. TCP reorders using sequence numbers.