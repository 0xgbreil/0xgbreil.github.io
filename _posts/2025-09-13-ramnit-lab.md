---
title: "Ramnit Lab - CyberDefenders"
date: 2025-09-13 21:00:00 +0200
categories: [CyberDefenders]
tags: [Endpoint Forensics, Execution, Defense Evasion, Command and Control, Volatility3, VirusTotal, Ramnit, Malware]
---



# Ramnit Lab - CyberDefenders

This is my writeup for the **Ramnit Lab** challenge from [CyberDefenders](https://cyberdefenders.org/blueteam-ctf-challenges/ramnit/).

## Scenario
Our intrusion detection system has alerted us to suspicious behavior on a workstation, pointing to a likely malware intrusion. A memory dump of this system has been taken for analysis. Your task is to analyze this dump, trace the malware’s actions, and report key findings.

## Q1: What is the name of the process responsible for the suspicious activity?
First, I used **Volatility 3** to solve this lab.  
You can download it from the official repository:  
[Volatility 3 on GitHub](https://github.com/volatilityfoundation/volatility)

Then, I used the following command to list the processes:

```bash
python3 vol.py -f memory.dmp windows.pslist
```
I searched extensively for any unusual processes but couldn’t find any.  
However, I noticed a process named **ChromeSetup.exe**, and I believe this is what we are looking for.

![pslist image](/images/Ramnit%20Lab/1.png)

Answer: **ChromeSetup.exe**

## Q2: What is the exact path of the executable for the malicious process?

I used the following command to check the path of the malicious process:

```bash
python3 vol.py -f memory.dmp windows.cmdline
```

![cmdline image](/images/Ramnit%20Lab/2.png)

Answer: **C:\Users\alex\Downloads\ChromeSetup.exe**

## Q3: dentifying network connections is crucial for understanding the malware's communication strategy. What IP address did the malware attempt to connect to?

There are two ways to solve this question:

1. Use the **windows.netstat** plugin.  
2. Dump the process on your machine and analyze it using  **[VirusTota](https://www.virustotal.com/gui/home/search)**

I used the first method with the following command:

```bash
python3 vol.py -f memory.dmp windows.netstat
```
From the output, I found the IP address as shown in the screenshot below:

![netstat image](/images/Ramnit%20Lab/3.png)

Answer: **58.64.204.181**

## Q4: To determine the specific geographical origin of the attack, Which city is associated with the IP address the malware communicated with?

I searched for the IP address **58.64.204.181** on [ipinfo](https://ipinfo.io/).

![ipinfo image](/images/Ramnit%20Lab/4.png)

Answer: **Hong Kong**

## Q5: Hashes serve as unique identifiers for files, assisting in the detection of similar threats across different machines. What is the SHA1 hash of the malware executable?

We will dump the malicious process and extract its hash.  
But before starting, we need to identify the **PID** of the process.

We need to use the following command:

```bash
python3 vol.py -f memory.dmp windows.pslist
```
![ PID image](/images/Ramnit%20Lab/5.png)

We can see that the **PID** of the malicious process is **4628**.  
Now, let’s dump the process using the following command:

```bash
python3 vol.py -f memory.dmp -o /home/kali/Desktop windows.dumpfiles --pid 4628
```
![ Dump file image](/images/Ramnit%20Lab/6.png)

Now, we use the following command to calculate the **SHA1 hash** of the dumped file:

```bash
sha1sum /home/kali/Desktop/file.0xca82b85325a0.0xca82b7e06c80.ImageSectionObject.ChromeSetup.exe.img
```
![ SAH1 image](/images/Ramnit%20Lab/7.png)

Answer: **280c9d36039f9432433893dee6126d72b9112ad2**

## Q6: Examining the malware's development timeline can provide insights into its deployment. What is the compilation timestamp for the malware?

I did a lookup for the hash on VirusTotal . 
Then, I navigated to the **Details** tab on VirusTotal to review the information about the file.


![  Time  image](/images/Ramnit%20Lab/8.png)

Answer: **2019-12-01 08:36**

## Q7: Identifying the domains associated with this malware is crucial for blocking future malicious communications and detecting any ongoing interactions with those domains within our network. Can you provide the domain connected to the malware?

I navigated to the **Relations** tab on VirusTotal to examine the file’s connections and related artifacts.

![  domian  image](/images/Ramnit%20Lab/9.png)

Answer: **dnsnb8.net**

## Conclusion


Feel free to connect with me on [LinkedIn](https://www.linkedin.com/in/0xgbreil/).
