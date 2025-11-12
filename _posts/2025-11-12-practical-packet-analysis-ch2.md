---
layout: post
title: "Practical Packet Analysis – Chapter 2: Tapping into the Wire"
date: 2025-11-12
categories: [Practical Packet Analysis]
tags: [Networking, Wireshark, Packet Analysis, Pcap]
---

# Chapter 2: Tapping into the Wire

![intro image](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/1.png)

this chapter discusses how to correctly position a **packet sniffer** to capture network traffic effectively. It introduces the concept of *“tapping into the wire”*, explains why sniffer placement can be challenging, and highlights the importance of understanding network hardware like **hubs**, **switches**, and **routers** before capturing packets.

![sinfffer Place](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/2.png)
##  Living Promiscuously

Before capturing packets, a **Network Interface Card (NIC)** must support *promiscuous mode*. This mode allows the NIC to see **all packets** passing through the network, not just those addressed to it.

Normally, each device only processes packets specifically meant for it, discarding the rest to save processing power. However, this behavior limits packet analysis, since analysts often need to view **every packet** on the wire.

By enabling **promiscuous mode**, the NIC forwards all packets to the CPU, regardless of their destination, allowing packet-sniffing tools (like **Wireshark**) to capture and analyze them.

Most modern NICs support this feature, and Wireshark uses **libpcap/WinPcap drivers** to activate it directly.  
You must have both a compatible NIC and an operating system that allow promiscuous mode, and usually, **administrator privileges** are required to use it.

##  Sniffing Around Hubs

Sniffing traffic on a **hub-based network** is ideal for packet analysis.  
In a hub, every packet sent by one device is copied to **all other ports**, so a packet sniffer connected to any port can see **all network communication** passing through the hub.

This gives the analyst a *limitless visibility window*—you can view all traffic between every device connected to the same hub.

However, hub-based networks are now **rare**. The main problem is **packet collisions**: only one device can send data at a time.  
If multiple devices send packets simultaneously, the packets collide, leading to data loss and retransmissions, which slow down the network.  
Because of this inefficiency, modern networks mostly use **switches** instead of hubs.

![Sniffing Around Hubs](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/3.png)

##  Sniffing in a Switched Environment

Switches are the most common devices in modern networks because they efficiently handle **broadcast**, **unicast**, and **multicast** traffic. They also support **full-duplex communication**, allowing devices to send and receive data at the same time.

However, switches make packet analysis more difficult. When a sniffer is connected to a switch port, it can only see **broadcast traffic** and the **traffic sent or received by its own machine** — not the traffic between other devices.

This limits the analyst’s visibility window compared to a hub network.  
To capture traffic from a target device on a switched network, there are **four main techniques**:
1. **Port mirroring**
2. **Hubbing out**
3. **Using a network tap**
4. **ARP cache poisoning**

![Sniffing in a Switched Environment](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/4.png)

### Port Mirroring

**Port mirroring** (aka port spanning) is one of the easiest ways to capture traffic from a target on a switched network.  
Requirements: you must have access to the switch’s management interface (CLI or web GUI), the switch must support mirroring, and you need an empty port to plug your sniffer into.

How it works:
- You configure the switch to **copy all traffic** from a source port (the target’s port) to a destination port (the sniffer’s port).
- Example: mirror port **3** to port **4** — plug your analyzer into port 4 to see everything sent/received by the device on port 3.

![Port Mirroring ](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/5.png)

Notes:
- Configuration commands vary by switch vendor (many switches use CLI; some have web GUIs). Check the switch documentation for exact commands.
- Watch port throughput. Mirroring many full-duplex ports into a single port can overwhelm the destination port.  
  Example: mirroring 23 full-duplex 100 Mbps ports to one port could produce up to **4,600 Mbps**, exceeding a single port’s capacity and causing packet loss, pauses, or dropped traffic.

![ Notes1](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/6.png)

### Hubbing Out

Another way to capture the traffic through a target device on a switched network is by hubbing out. This technique puts both your analyzer and the target device on the same network segment by connecting them to a hub.
It’s especially useful when port mirroring isn’t available but you still have physical access to the switch.

To hub out, you need a hub and some network cables:

Unplug the target device from the switch.

Plug the target’s cable into the hub.

Connect your analyzer to the hub.

Connect the hub back to the switch using another cable.

Now, your analyzer and target are on the same broadcast domain, allowing your analyzer to capture all packets from the target device.

However, note that hubbing out reduces the duplex mode from full to half, which might slow communication.
Also, hubs need power, so you’ll need a power outlet nearby.

Tip: Always inform the device’s user before unplugging it — especially if it’s the CEO’s device!

#### Finding “True” Hubs

Be careful: many devices are sold as hubs but are actually low-level switches.
If you use a fake hub (a switch), you’ll only capture your own traffic, not the target’s.

To check if it’s a true hub, connect two computers to it:

If one computer can sniff the other’s traffic, it’s a real hub.

If not, it’s a switch in disguise.

True hubs are rare and outdated, so you might find them:

At surplus auctions (like school district sales).

On eBay, but be cautious — many listings are mislabeled.

![Hubbing Out ](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/7.png)

###  Using a Tap

A **network tap** is a hardware device placed inline between two points on a cable to capture packets flowing between them. It's like hubbing out but uses purpose-built hardware for reliable, non-intrusive capture.

**Tap types**
- **Non-aggregated tap**: typically has **four ports** (two for the network link, two for monitoring both directions separately).
- **Aggregated tap**: simpler, with **three ports** (network in, network out, and a single monitor port that provides bidirectional traffic).

**Power**
- Most taps require power; some include batteries for short, portable captures.

**Using an aggregated tap (capture all traffic to/from one machine)**
1. Unplug the target computer from the switch.  
2. Plug the computer into the tap’s **“in”** port.  
3. Connect the tap’s **“out”** port to the network switch.  
4. Connect the tap’s **monitor** port to your sniffer machine.

After this, the sniffer on the monitor port will see all traffic entering and leaving the target host.

**Why use a tap**
- Taps are designed for analysis and tend to be more reliable and less disruptive than hubbing out.
- They provide a clean way to capture traffic even when switches don’t support mirroring.

![ Using a Tap ](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/8.png)

###  Aggregated vs. Nonaggregated Taps

**Aggregated taps**
- Simple: **3 ports** — `in`, `out`, and one `monitor` port that shows bidirectional traffic.
- Setup to capture a single host:
  1. Unplug the target from the switch.
  2. Plug target → tap **in**.
  3. Plug tap **out** → switch.
  4. Plug tap **monitor** → sniffer.
- Pros: fewer cables, only one NIC needed on the sniffer.
- Cons: can be overwhelmed if traffic volume is very high.

![ Aggregated taps ](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/9.png)

**Nonaggregated taps**
- More flexible: **4 ports** — `in`, `out`, `monitor A` (one direction), `monitor B` (other direction).
- Setup to capture a single host:
  1. Unplug target from switch.
  2. Target → tap **in**.
  3. Tap **out** → switch.
  4. Tap **monitor A** → sniffer NIC1.
  5. Tap **monitor B** → sniffer NIC2.
- Pros: better for high-volume captures or when directional separation is needed.
- Cons: requires two NICs and more cabling.

![ Nonaggregated taps ](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/10.png)

**Choosing a tap**
- Aggregated taps are usually preferred for simplicity and lower hardware needs.
- Nonaggregated taps are useful for heavy traffic or one-direction-focused captures.
- Price range: from inexpensive Ethernet taps (~US$150) to expensive enterprise fiber taps (five-figure prices).

---

##  ARP Cache Poisoning (a method for tapping a switched network)

**ARP basics (quick recap)**
- Layer 2 uses MAC addresses; Layer 3 uses IP addresses.
- When a host needs a MAC for a known IP, it checks its ARP cache. If missing, it broadcasts an ARP request to `FF:FF:FF:FF:FF:FF`.
- The owner replies with an ARP reply containing its MAC; the requester stores it in the ARP cache.

**How ARP cache poisoning (ARP spoofing) works**
- An attacker (or analyst) sends fake ARP messages to clients or switches, advertising forged MAC-to-IP mappings.
- By poisoning ARP caches, the attacker can cause traffic destined for the target to be forwarded to the attacker’s machine (interception) or cause disruption (DoS).
- ARP poisoning is an advanced, software-based way to tap into a switched network when hardware taps or mirroring aren’t available.

###  How ARP Cache Poisoning Works (ARP Spoofing)

**What it is**  
ARP cache poisoning (aka ARP spoofing) sends fake ARP messages that advertise forged MAC↔IP mappings, causing other hosts or switches to send traffic intended for a target through the attacker/analyst machine. It's an advanced software method to intercept traffic on switched networks.

**Uses**  
- Attackers: intercept traffic, perform man-in-the-middle, or cause DoS.  
- Analysts: legitimate way to capture a target's packets when hardware taps or mirroring aren't available.

![ ARP cache poisoning ](/images/Practical%20Packet%20Analysis/Chapter%202%20Tapping%20into%20the%20Wire/11.png)