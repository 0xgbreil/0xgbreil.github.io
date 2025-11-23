---
layout: post
title: "Practical Packet Analysis – Chapter 4: Working with Captured Packets"
date: 2025-11-23
categories: [Practical Packet Analysis]
tags: [Networking, Wireshark, Packet Analysis, Pcap]
---

# Chapter 4: Working with Captured Packets

![intro image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/1.png)

In this chapter, you start actively using Wireshark to capture and analyze packets. 
The chapter explains how to handle capture files, examine packet details, and adjust 
time-display formats. It also introduces advanced capture options and explores filtering.

## Working with Capture Files
Most real-world packet analysis happens *after* the capture is complete. Analysts usually 
perform multiple captures at different times, save them, and review them later. 
Wireshark supports this workflow by allowing you to:
- Save capture files for later analysis  
- Merge multiple capture files into a single file for easier investigation

## Saving and Exporting Capture Files 

Wireshark lets you save your packet captures using **File → Save As**, where you choose 
the save location and file format. If you don’t specify a format, Wireshark uses the 
default **.pcapng** format.

![save image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/2.png)

A powerful feature in the Save As dialog is the ability to save **specific packet ranges**. 
This is useful for reducing large capture files by saving only:
- A certain packet number range
- Marked packets
- Packets currently visible after applying a display filter

Wireshark also supports exporting capture data into different formats for use in other 
applications or tools. Supported export formats include:
- Plaintext
- PostScript
- CSV (comma-separated values)
- XML

To export, select **File → Export Packet Dissections** and then choose the desired format. The Save As 
dialog will show options specific to the chosen export format.

![image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/17.png)

## Merging Capture Files 

Some types of analysis require combining multiple capture files. This is commonly 
done when you need to compare two data streams or combine traffic streams that 
were captured separately.

To merge capture files, open one of the files and choose **File → Merge**. This opens 
the *Merge with Capture File* dialog. From there:
- Select the capture file you want to merge into the one already open  
- Choose how the merge should happen:

### Merge Options:
- **Prepend**: Add the selected file *before* the currently open file  
- **Append**: Add the selected file *after* the currently open file  
- **Chronologically Merge**: Combine both files and reorder all packets based on 
  their timestamps
![Merge image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/3.png)

## Working with Packets 

When dealing with a large number of packets, navigating them efficiently becomes essential. Wireshark provides tools to **find, mark, and print packets** to make analysis easier.

### Finding Packets
To locate packets that match specific criteria:
- Open the **Find Packet** dialog by pressing **CTRL-F**

![find image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/4.png)

### Options in Find Packet Dialog:
1. **Display Filter**: Enter a filter expression to find packets that satisfy it  
2. **Hex Value**: Search for packets containing a specific hexadecimal value  
3. **String**: Search for packets containing a specific text string  
![find 2 image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/5.png)

Additional options allow you to:
- Select the window to search in  
- Choose the character set  
- Set the search direction  
- Make string searches case sensitive  

### Navigation:
- After entering search criteria, click **Find** to locate the first matching packet  
- Press **CTRL-N** for the next matching packet  
- Press **CTRL-B** for the previous matching packet

## Marking Packets

After finding packets that match your criteria, Wireshark lets you **mark** the ones 
you want to focus on. Marked packets are useful when you want to save them separately 
or quickly locate them later. They appear with a **black background and white text**.

### How to Mark Packets:
- Right-click the packet in the Packet List and select **Mark Packet**
- Or press **CTRL-M** to mark the selected packet

### Unmarking:
- Press **CTRL-M** again to unmark the packet  
- You can mark as many packets as you want

### Navigation Between Marked Packets:
- **SHIFT-CTRL-N** → Jump to the next marked packet  
- **SHIFT-CTRL-B** → Jump to the previous marked packet

![Mark Packet  image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/9.png)

## Printing Packets 

While most packet analysis is done on the screen, Wireshark also lets you print 
captured packets. Printing can be useful for quick reference, documentation, or when 
generating reports. You can even print packets to a PDF file.

To print packets, open the **Print** dialog by selecting **File → Print**.

![image 20 ](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/18.png)

### Printing Options:
You can choose to print:
- As **plaintext**
- As **PostScript**
- Or print to an **output file**

Similar to the Save dialog, you can print:
- A **specific packet range**
- **Marked packets only**
- **Packets currently visible** after a display filter

You can also choose which of Wireshark’s three main panes to include when printing 
each packet.

After selecting your options, click **Print** to generate the output.

![Print Packet  image](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/10.png)


## Setting Time Display Formats and References 

Time is critical in packet analysis, as network activity is highly time-sensitive. 
Wireshark provides several options to control how time is displayed during analysis.

### Time Display Formats
Every captured packet receives a timestamp from the operating system.  
Wireshark can display:
- The **absolute timestamp** (the exact capture moment)
- Time **relative to the previous packet**
- Time **relative to the start of the capture**
- Time **relative to the end of the capture**

You can configure these options from **View → Time Display Format**, where you can:
- Change the **presentation format**
- Adjust the **precision** (seconds, milliseconds, microseconds, etc.)
![image 1](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/11.png)
### Packet Time Referencing
Time referencing lets you pick a specific packet and make all following time 
calculations relative to that packet instead of the start of the capture.

To set a time reference:
- Select a packet → **Edit → Set Time Reference**

To remove the reference:
- Select the same packet → toggle **Set Time Reference** off

When a packet is marked as a reference, the Time column shows **"REF"**.

**Important:**  
Time referencing only works correctly when the time display format is set to show 
time **relative to the beginning of the capture**.  
Using any other time format will produce confusing and meaningless results.

![image 2](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/12.png)

## Setting Capture Options 

Wireshark provides extensive capture options to give flexibility and control over 
how packets are captured and saved. These options are accessible via **Capture → Interfaces → Options**.

### Capture Settings
- **Interface selection**: Choose the network interface to capture from (local or remote)  
- **Promiscuous mode**: Enable or disable (default is enabled)  
- **Capture format**: Choose between standard or experimental **pcap-ng**  
- **Packet size limit**: Limit capture size by bytes  
- **Wireless/remote settings**: Configure as needed  
- **Buffer size**: Adjust kernel buffer size (Windows only)  
- **Capture filter**: Apply a filter to capture only specific packets

![image 3](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/13.png)

### Capture File(s) Settings
- Automatically save captured packets to a file or a file set  
- Options include:
  - **Single file** or **Multiple files (file set)**  
  - **Ring buffer**: Overwrite oldest files when max number is reached  
- **Triggers**: Create a new file based on:
  - File size  
  - Time interval  
- **Stop Capture After**: Stop capture after a certain number of files

### Stop Capture Settings
- Stop capture based on:
  - File size  
  - Time interval  
  - Number of packets

### Display Options
- **Update List of Packets in Real Time**: Show captured packets instantly  
- **Automatic Scrolling**: Keep view scrolled to newest packets  
 Warning: Using both can be processor intensive; disable if not needed  
- **Hide Capture Info Dialog**: Suppress the small info window showing number/percentage of captured packets

### Name Resolution Settings
- Enable automatic resolution for:
  - MAC addresses (Layer 2)  
  - Network addresses (Layer 3)  
  - Transport addresses (Layer 4)  
- Full discussion of name resolution and its drawbacks is in Chapter 5



## Using Filters

Filters allow you to control exactly which packets are shown or captured during analysis. A filter is simply an expression that defines rules for including or excluding packets.  
- If there are packets you don’t want to see, you can write a filter to hide them.  
- If you want to focus only on specific packets, you can write a filter that shows only those.

Wireshark provides two main types of filters:

### 1. Capture Filters
These are applied *before* capturing packets.  
Only packets that match the filter expression will be captured and saved.

### 2. Display Filters
These are applied *after* capturing packets.  
They allow you to hide unwanted packets or show only the packets that match certain criteria.

The chapter begins with explaining capture filters first.

## Capture Filters

Capture filters are applied during the actual packet-capturing process.  
They are mainly useful for **performance**, because they prevent Wireshark from capturing packets you don’t need, saving CPU and storage.

Custom capture filters are especially helpful when working with large amounts of traffic.  
By capturing only the relevant packets, the analysis becomes much faster and easier.

### Example Scenario
If you're analyzing a server with multiple services running on different ports, and you only want to troubleshoot traffic related to port **262**, manually finding that traffic would be difficult.

Instead, you can create a capture filter to record **only** packets going to or from port 262.

### Steps to Create a Capture Filter
1. Go to **Capture → Interfaces**, then click **Options** next to the interface you want.
2. Select the interface to capture on.
3. In the **Capture Filter** field, enter an expression—here:  
   **`port 262`**  
   This filters all traffic to/from port 262.
4. Click **Start** to begin capturing.

After capturing enough packets, you will see **only port 262 traffic**, making the analysis faster and more efficient.

![image 4](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/14.png)

## Capture/BPF Syntax

Capture filters in Wireshark use **Berkeley Packet Filter (BPF)** syntax, which is also used by many packet-sniffing tools because they rely on libpcap/WinPcap libraries.  
Understanding BPF syntax is essential when working with packets at a deep level.

### Structure of a BPF Expression
A capture filter is built using **expressions**, and each expression is made of one or more **primitives**.  
A primitive contains:
- One or more **qualifiers**
- Followed by an **ID** (like an IP, network, or port)

Example:  
`src 192.168.0.10`  
This primitive matches packets with the **source IP = 192.168.0.10**.

### Logical Operators
You can combine primitives using three logical operators:
- **AND (&&)** → both conditions must be true  
- **OR (||)** → either condition can be true  
- **NOT (!)** → excludes matching packets  

Example filter: **src 192.168.0.10 && port 80**

This captures packets **from 192.168.0.10** AND with **port 80** (either source or destination port).

### BPF Qualifiers (Types of qualifiers)
- **Type:** defines what the ID refers to  
  - Examples: `host`, `net`, `port`
- **Direction (Dir):** defines packet direction  
  - Examples: `src`, `dst`
- **Protocol (Proto):** restricts to a specific protocol  
  - Examples: `ether`, `ip`, `tcp`, `udp`, `http`, `ftp`

![image 5](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/15.png)
## Hostname and Addressing Filters 
This section explains how to filter packets in Wireshark using different types of addresses and protocols.  
You can filter traffic based on hostnames, IPv4, IPv6, or MAC addresses.  
You can also specify direction (source or destination) to narrow down traffic.

Examples:
- Filter by IPv4: `host 172.16.16.149`
- Filter by IPv6: `host 2001:db8:85a3::8a2e:370:7334`
- Filter by hostname: `host testserver2`
- Filter by MAC: `ether host 00-1a-a0-52-e2-a0`

Direction qualifiers:
- `src` → traffic coming **from** a host
- `dst` → traffic going **to** a host

When no type qualifier is used, Wireshark assumes `host` by default.

Port filtering:
- `port 8080` → only traffic on port 8080
- `!port 8080` → everything except port 8080
- `dst port 80` → traffic going to port 80

Protocol filters:
- `icmp`
- `!ip6` → everything except IPv6

![save image 2 ](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/1.jpeg)

## Protocol Filters

Protocol filters allow you to filter packets based on specific protocols.  
Examples:
- Show only ICMP: `icmp`
- Hide IPv6: `!ip6`

BPF syntax also lets you filter based on specific bytes inside protocol headers using offsets.

Examples:
- ICMP type field is at offset 0 → `icmp[0] == 3` (destination unreachable)
- Match echo request/reply → `icmp[0] == 8 || icmp[0] == 0`
- Read 2 bytes starting at offset 0 → `icmp[0:2] == 0x0301` (type 3, code 1)

TCP flags are at offset 13, and each flag is a bit inside that byte:
- RST flag → `tcp[13] & 4 == 4`
- PSH flag → `tcp[13] & 8 == 8`

Table of common capture filters includes filters for TCP flags, ICMP types, IPv4, IPv6, UDP, and MAC-based filtering.
![image 6](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/16.png)
Display filters allow hiding or showing packets without removing them from the capture.  
Example:
- Hide ARP traffic: `!arp`

The Filter Expression dialog helps beginners create filters by selecting protocol fields, conditions, and values without typing manually.  
Advanced users prefer writing expressions directly for speed and flexibility.

## Filter Expression Syntax Structure 

Wireshark filter expressions let you include or exclude packets using comparison and logical operators.

**Comparison operators** allow you to match specific values:
- Example: show all packets involving IP `192.168.0.1`  
  `ip.addr == 192.168.0.1`
- Example: show packets 128 bytes or less  
  `frame.len <= 128`

Available comparison operators: `==`, `!=`, `>`, `<`, `>=`, `<=`.

**Logical operators** let you combine multiple conditions:
- To show packets from either of two IPs:  
  `ip.addr==192.168.0.1 or ip.addr==192.168.0.2`

Logical operators include: `and`, `or`, `xor`, `not`.

Wireshark also includes many ready-to-use display filters, such as:
- `!tcp.port==3389` → hide RDP traffic  
- `tcp.flags.syn==1` → show SYN packets  
- `!arp` → hide ARP  
- `http` → show HTTP traffic  
- `smtp || pop || imap` → show cleartext email protocols

**Saving Filters:**  
You can save your custom capture and display filters so you don’t have to retype them.  
Steps include opening the filter dialogs, creating a new filter, naming it, entering the expression, and saving it.

Wireshark includes built-in filters that beginners can use as examples when building their own.

![save image 3 ](/images/Practical%20Packet%20Analysis/Chapter%204%20Working%20with%20Captured%20Packets/2.jpg)

## Conclusion

In this chapter, we explored how Wireshark filter expressions work, including comparison and logical operators, and how to build effective capture and display filters. We also covered how to save your custom filters to streamline your workflow during packet analysis.

In the next chapter, we will dive into **Advanced Wireshark Features** to take your network analysis skills to the next level.

If you have any feedback or would like to connect, feel free to reach out on LinkedIn:  [Mohamed Gbreil](https://www.linkedin.com/in/0xgbreil/)