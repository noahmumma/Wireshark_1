<p align="center">
<img src="https://i.imgur.com/NNFKCTY.png" alt="SIEM Logo"/>
</p>


<h1>Using Wireshark to Analyze Malicious Network Logs</h1>
This tutorial sought to answer four questions about a day's worth of packet data and to find malicious files.<br />

<h2>The Four Questions</h2>
1. What is the date of this network traffic?
</p> 
2. Which downloaded files were infected, and what are their file hashes?
</p>
3. What is the domain name that delivered the exploit kit and malware?
</p>
4. What is the IP address, MAC address, and host name of the infected machine?

<h2>What is Wireshark?</h2>
Wireshark is a tool that presents captured packet data in a digestible format, allowing a security team to analyze it effectively. It's known as a "network packet analyzer" (NPA).  
</p> 
Imagine an NPA as a tool to help examine exactly what's happening inside a network cable. Whenever a packet is sent through the network cable, Wireshark can present that data to an analyst, showing them everything about it. 
</p> 
But this is not limited to just analysts. Network administrators can use this to troubleshoot network problems, developers can use it to debug protocol implementations, and engineers can use it to examine security problems. 


<h2>Environments and Technologies Used</h2>

- Kali Linux
- U.S. Cyber Range
- Wireshark
- Virus Total

<h2>Operating Systems Used </h2>

- Linux 6.18

<h2>High-Level Deployment Steps</h2>

- Step 1: Change the format of the "Time Column" to reveal the date.
- Step 2: Filter packets to just "http.request", use "md5sum" to get the hash values from those that contained executables, run them through virustotal[.]com to find which are malicious.
- Step 3: Head back to the content type section of the executable files, and further inspect them to find the hostname. 
- Step 4: Clear all filters, then apply "Dynamic Host Configuration Protocol" as a filter,  select "User Datagram Protocol", and scroll down to find the hostname, IP and MAC address of the infected machine. 

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/sU1iZsW.png" height="80%" width="80%" alt="Step 1"/>
</p>
<p>
The first question, "What is the date of this network traffic?" is solved easily. All you need to do is. Select “View --> Time Display Format --> Date and Time of Day”. Selecting this brings up the date of the network traffic: November 16, 2014.  
<br />

<p>
<img src="https://i.imgur.com/sU1iZsW.png" height="80%" width="80%" alt="Step 1"/>
</p>
<p> 
For the second question, "Which downloaded files were infected and what are their file hashes?", I needed to narrow down the number of packets I was inspecting. I went to “Statistics --> Protocol Hierarchy --> HTTP --> Apply as Filter”.
<p>
This brought up exclusively the packets that dealt with HTTP. Then, at the top search bar, I entered “http.request” to only show the HTTP packets that were making requests. Finally, I only wanted to see the suspicious files, so I looked for those that contained executable files. By going to “Export --> Content Type”, I could bring the files to the top that contained executables.  
<p>
After, I exported one of each file types (java, msdownload, shockwave-flash) to the VM and used the “md5sum” command to get their hash values.  
<p></p>
Java File – 1e34fdebbf655cebea78b45e43520ddf 
<p></p>
Msdownload File – d276c86dcdbcdb6b74ee02496bc90d98 
<p></p>
SWF File -- 7b3baa7d6bb3720f369219789e38d6ab 
<p>
</p>
<img src="https://i.imgur.com/sU1iZsW.png" height="80%" width="80%" alt="Step 1"/>
The Java file was flagged as malicious
</p><p>
<img src="https://i.imgur.com/sU1iZsW.png" height="80%" width="80%" alt="Step 1"/>
</p>
The msdownload file was not flagged as malicious
</p><p>
<img src="https://i.imgur.com/sU1iZsW.png" height="80%" width="80%" alt="Step 1"/>
</p>
The SWF was flagged as malicious
</p>
Using virustotal.com, a website that allows you to check file hashes to see if they are malicious, I entered each file hash to reveal if it was infected. 2/3 were.
</p>

</p>
<img src="https://i.imgur.com/sU1iZsW.png" height="80%" width="80%" alt="Step 1"/>
</p>
The next question, "What is the domain name that delivered the exploit kit and malware?", was already discovered through a previous step. Revisiting the “Export --> Content Type” section, I found the domain name for the exploit kit and malware. All three of the executable files, they all shared the same host name, even the one msdownload file that was not malicious. The host name is “stand.trustandprobatereality.com”.
<br />



