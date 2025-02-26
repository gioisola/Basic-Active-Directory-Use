<h1>Basic Active Directory Use</h1>

<h2>Description</h2>
</b>
The goal of this lab was to start from scratch and create a basic Active Directory and practice adding users to it. This was my first introduction to AD so I did not dive too deep into it. 

<br/>
</br>

<i>Note: This lab includes some troubleshooting so it is a bit longer than my other projects.</i>

<br />
<br />

<p align="center">
<img src="https://i.imgur.com/ZdrkzwY.png" height="85%" width="85%" alt="RDP event fail logs to iP Geographic information"/>
</p>

<br/>
<br/>

<h2>Utilities Used</h2>

- Microsoft Server 2019
- Microsoft 10 PRO
- VirtualBox

<br/>

<h2>Program Walk Through</h2>
Two virtual machines were created. One to run Microsoft Server and another to simulate a basic Windows 10 client side machine. The client machine adapter settings were set up to connect to the internal network.  

<p align="center">
<img src="https://i.imgur.com/pqNbmzF.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

The first thing I did inside the server VM was label and set up the internal and external networks. I gave the "INTERNAL" network an abitrary IP address of 172.16.0.1 (I don't have photos but I believe this started out as an APIPA address). This is the address that would be used to connect the client to the DC. I labeled the external network as "INTERNET". This is what allowed the DC to connect to the public internet using my host computer existing internet connection. 

<p align="center">
<img src="https://i.imgur.com/JMBGJqw.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/3IWt8qf.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

The next step was so install Active Directory and create our domain. I used mydomain.com.

<p align="center">
<img src="https://i.imgur.com/T7z6xtT.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/UcHKpa6.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

I created an OU named ADMIN, added myself, and gave myself admin permissions. 

<p align="center">
<img src="https://i.imgur.com/eIKZOOC.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

I set up NAT to get the client connected to the internet through the DC.

<p align="center">
<img src="https://i.imgur.com/n90JE6g.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/eHCgDJT.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

Then a DHCP server had to be set up so the client could get an IP address to browse the internet.  

<p align="center">
<img src="https://i.imgur.com/qCIu74m.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/sp5CHIt.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

I hopped into the client machine and tested my internet connection. This is where I found my first problem. I could not ping to any sites through the command line. I did ipconfig /all to see where the issue might be. I did some digging and found that this might be a DNS issue. Seems right as my DNS was bringing up 10.0.2.15. This was the IP address of the DC external network rather than the internal network of 176.16.0.1.  

<p align="center">
<img src="https://i.imgur.com/8smaFLU.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/QD2YKM4.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

I did some research on how I could fix this issue. I found that within the DHCP scope options there was a section for DNS servers that showed the incorrect address (10.0.2.15). I removed that and added the correct one.

<p align="center">
<img src="https://i.imgur.com/E1OAMRq.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/itecCig.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

To test if this worked, I went back to the client machine and tried a few commands. I tested /flushdns but it did not work. I then tried /release and /renew and luckily this fixed the problem. 

<p align="center">
<img src="https://i.imgur.com/WmkkYCH.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/HRpJLcw.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/yjVL9wn.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/5gZaK20.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

After that was fixed I went back to the server machine and added all my users to AD. I used a script found online to mass create 1000 users. I also manually created a few just to see the process of how it was done.  

<p align="center">
<img src="https://i.imgur.com/ZoPvD0G.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
<p align="center">
<img src="https://i.imgur.com/PjWFCGx.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>

<br/>
<br/>

To finish this project off, I joined the new client machine to the domain and logged into it using one of the users I manually created. 

<p align="center">
<img src="https://i.imgur.com/IHnsOIE.png" height="85%" width="85%" alt="Image Analysis Dataflow"/>
</p>
