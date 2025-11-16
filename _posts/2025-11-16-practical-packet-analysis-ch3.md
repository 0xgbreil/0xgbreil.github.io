---
layout: post
title: "Practical Packet Analysis – Chapter 3: Introduction to Wireshark"
date: 2025-11-16
categories: [Practical Packet Analysis]
tags: [Networking, Wireshark, Packet Analysis, Pcap]
---

# Chapter 3: Introduction to Wireshark

![intro image](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/1.png)

This chapter introduces Wireshark, the packet-sniffing tool used throughout the book. While many applications exist for network analysis, Wireshark is the most popular and powerful choice.

## A Brief History of Wireshark
Wireshark has a long and interesting history. It was originally developed by Gerald Combs, a Computer Science graduate from the University of Missouri–Kansas City. The first version, called Ethereal, was released in 1998 under the GNU Public License (GPL).

After eight years, Combs left his job, but his employer owned the Ethereal trademark. Since he couldn’t obtain rights to the name, he and the development team rebranded the project as Wireshark in 2006.

Since then, Wireshark has grown significantly, with more than 500 contributors involved in development. The Ethereal project is no longer maintained, and all active development now continues under the Wireshark name.

## The Benefits of Wireshark

Wireshark provides several advantages that make it suitable for both beginners and experienced packet analysts. Here are its main benefits based on the criteria from Chapter 1:

#### Supported Protocols
Wireshark supports more than 850 protocols, from common ones like IP and DHCP to advanced proprietary protocols such as AppleTalk and BitTorrent. Since it is open source, new protocols are added regularly. If a protocol isn’t supported, users can even write the support themselves and submit it to the developers.

#### User-Friendliness
Wireshark has one of the most intuitive interfaces among packet-sniffing tools. It uses a GUI, offers clear menus, and includes features like protocol color-coding and graphical representations of raw data. Compared to command-line tools like tcpdump, Wireshark is much more approachable for beginners.

#### Cost
Wireshark is completely free under the GPL license and can be used for personal or commercial purposes. Some people still mistakenly pay for it online—sometimes even for fake “enterprise licenses.”

#### Program Support
Despite being free, Wireshark has one of the most active support communities in open source. Its website provides documentation, a wiki, FAQs, and mailing lists monitored by top developers. Paid support is also available through CACE Technologies’ SharkNet program.

#### Operating System Support
Wireshark runs on all major operating systems, including Windows, macOS, and Linux. A full list of supported systems is available on its official website.

## Installing **Wireshark**

The installation process for **Wireshark** is very simple, but your system must meet the following requirements:
- **400 MHz processor** or faster  
- **128 MB RAM**  
- **75 MB** of free disk space  
- A **NIC** that supports **promiscuous mode**  
- The **WinPcap** capture driver

**WinPcap** is the Windows implementation of the **pcap API**, which allows the system to capture raw packets, apply filters, and enable promiscuous mode.  
Although you can download WinPcap separately, it’s recommended to install the version **bundled with Wireshark**, since it’s tested for compatibility.

### Installing on Microsoft Windows

To install Wireshark on Windows, download the latest installer from the official website (**wireshark.org**) under the **Downloads** section.  
After downloading, follow these steps:

1. **Double-click** the `.exe` file and click **Next**.  
2. Read the **license agreement**, then click **I Agree**.  
3. Select the **components** to install. For most users, the **default options** are recommended.


![install 1](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/2.png)

4. Click **Next** in the **Additional Tasks** window.  
5. Choose the **installation location** for Wireshark, then click **Next**.  
6. When prompted to install **WinPcap**, make sure the **Install WinPcap** box is checked, then click **Install**.  

![install 2](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/3.png)


- Around halfway through, the **WinPcap** installer will start.  
  - Click **Next**, read the **license agreement**, and then click **I Agree**.  
- **WinPcap** should install successfully. Click **Finish** when done.  
- **Wireshark** installation will complete. Click **Next**, then **Finish** in the confirmation window.
### **Installing on Linux Systems**

#### Overview
- To install Wireshark on a Linux system, the first step is downloading the correct installation package for your distribution.
- Not all Linux distributions are officially supported, so it’s possible that your distro won’t have a dedicated installation package.
- System-wide installations usually require **root access**, but compiling from source often doesn’t.

#### RPM-based Systems
- For RPM distributions like Red Hat Linux, download the correct package from the Wireshark website.
- Install it using:
 ```bash
  rpm -ivh wireshark-0.99.3.i386.rpm
  ```
- If any dependencies are missing, install them and repeat the Wireshark installation.

#### DEB-based Systems
- On DEB-based systems such as Debian or Ubuntu, Wireshark can be installed directly from the system repositories.
- Use the following command:
```bash
  apt-get install wireshark
 ```

#### Compiling Wireshark from Source

- If your Linux distribution doesn’t support automated package management, the best way to install Wireshark is by compiling it from source.

##### Steps to Compile
1. Download the source package from the Wireshark website.
2. Extract the archive:
   ```bash
   tar -jxvf wireshark-1.2.2.tar.bz2
   ```
3. Change into the directory created after extraction.
4. As a root user, configure the source to build for your Linux distribution:
```bash
   ./configure
   ```
   - You can modify default installation options here if needed.
   - Missing dependencies will trigger errors.
   - Successful configuration will display a success message.
5. Build the source into a binary using:
```bash
   make
   ```
6. Complete the installation with:
```bash
   make install
   ```

![install 3](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/4.png)

### Installing Wireshark on Mac OS X

- Installing Wireshark on Mac OS X Snow Leopard has a few extra steps, but it is straightforward.

#### Steps to Install
1. Download the DMG package from the Wireshark website.
2. Copy `Wireshark.app` to the Applications folder.
3. Open the Utilities folder inside `Wireshark.app`.
4. In Finder, click **Go → Go To Folder** and enter `/usr/local/bin/` to open the directory.
5. Copy the contents of the **Command Line** folder into `/usr/local/bin/`. You will need to enter your password.
6. In the Utilities folder, copy the **ChmodBPF** folder into the **StartupItems** folder. Enter your password again to complete the installation.



## Wireshark Fundamentals

- After installing Wireshark, you can start familiarizing yourself with it.  
- When you first open the program, the window will appear empty because no data has been captured yet.  
- To make things interesting, you need to capture some packet data.

### Your First Packet Capture
- To get packet data into Wireshark, perform your first capture.  
- It’s okay if nothing is wrong on the network; packet analysis isn’t just for troubleshooting problems but also to understand normal network traffic.  
- Knowing normal network traffic is essential to detect anomalies later.

**Steps to Capture Packets:**
1. Open Wireshark.  
2. From the main menu, select **Capture → Interfaces**.  
   - You’ll see a list of interfaces with their IP addresses.  
3. Select the interface you want to use and click **Start**, or click the interface directly from the welcome page.  
4. Wait about a minute, then click **Stop** from the **Capture** menu to end the capture and view your data.

- After completing these steps, the main window will fill with data.  
- It may seem overwhelming at first, but it will make sense once you explore each part of the Wireshark interface step by step.

![capture image 1](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/5.png)



##  Wireshark’s Main Window

- Most of your time in Wireshark will be spent in the main window.  
- This window displays all captured packets and organizes them in a more understandable format.  
- The main window uses a **three-pane design**: Packet List, Packet Details, and Packet Bytes.

### Pane Details
1. **Packet List (Top Pane)**  
   - Shows a table of all packets in the current capture.  
   - Columns include packet number, capture time, source and destination, protocol, and general info.  
   - Traffic here refers to all packets; e.g., DNS traffic shows only DNS packets.

2. **Packet Details (Middle Pane)**  
   - Shows hierarchical information for a single packet.  
   - Can collapse or expand sections to view details about specific parts of the packet.

3. **Packet Bytes (Lower Pane)**  
   - Shows the raw, unprocessed bytes of the packet as it travels over the network.  
   - This pane can be confusing at first because it shows data in raw format without interpretation.

**Interaction Between Panes**
- To see details of a packet, select it in the Packet List pane.  
- Then, clicking parts of the Packet Details pane highlights the corresponding bytes in the Packet Bytes pane.

![capture image 2](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/6.jpg)


## Wireshark Preferences


- Wireshark has several preferences that can be customized to meet your needs.  
- To access Preferences, select **Edit → Preferences** from the main menu.  
- The Preferences dialog contains several customizable options.

### Main Sections
1. **User Interface**  
   - Determines how Wireshark presents data.  
   - You can customize saving window positions, layout of the three main panes, scroll bar placement, Packet List columns, fonts, and background/foreground colors.

2. **Capture**  
   - Options related to how packets are captured.  
   - Includes default capture interface, promiscuous mode, and real-time Packet List updates.

3. **Printing**  
   - Options for how Wireshark prints captured data.

4. **Name Resolution**  
   - Allows Wireshark to resolve addresses into recognizable names (MAC, network, transport).  
   - You can also set the maximum number of concurrent name resolution requests.

5. **Statistics**  
   - Configurable options for Wireshark’s statistical features.

6. **Protocols**  
   - Options for capturing and displaying packets for various protocols Wireshark can decode.  
   - Not all protocols are configurable; some have multiple options.  
   - Best left at defaults unless a specific reason exists.

![wireshark 1](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/7.png)


## Packet Color Coding

Wireshark uses packet color coding to make protocol identification faster and easier.  
Each color represents a specific protocol — for example, DNS packets appear in blue, and HTTP packets appear in green.

This visual coding helps analysts move quickly through large capture files without reading the protocol field for every packet.

### Coloring Rules
You can view and edit all color rules through the **Coloring Rules** window:
- Go to **View → Coloring Rules** to see all protocol-color associations.
- Wireshark allows modifying existing rules or creating new ones.

### Example: Changing the HTTP Color
To change the background color of HTTP packets:
1. Open Wireshark → **View → Coloring Rules**.
2. Select the **HTTP** rule.
3. Click **Edit** to open the Edit Color Filter dialog.
4. Choose a new **Background Color**.
5. Apply the changes.

### Why Color Coding Helps
As you work with captures, some protocols appear more often than others.  
Custom colors help highlight important traffic — for example, coloring DHCP traffic in bright yellow makes spotting rogue DHCP activity easier.

Color rules can also be based on custom filters for even more precise analysis.

With color coding configured, you're ready to move into deeper packet analysis in the next chapter.

![wireshark 2](/images/Practical%20Packet%20Analysis/Chapter%203%20Introduction%20to%20Wireshark/8.png)

## Conclusion

In this chapter, we explored the essential foundations of Wireshark — from installing it on different operating systems to understanding its interface, preferences, and powerful packet color-coding features. By now, you should have a solid grasp of how Wireshark displays, organizes, and highlights network traffic to make analysis faster and clearer.

In the **next chapter**, we will dive into **Working With Captured Packets**, where you’ll learn how to navigate, filter, inspect, and analyze packets in real-world scenarios.

If you have any feedback, suggestions, or questions, feel free to reach out to me on LinkedIn:  [Mohamed Gbreil](https://www.linkedin.com/in/0xgbreil/)