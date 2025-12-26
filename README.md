# Azure-Network-Protocols

<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Actions and Observations</h2>

------------------------------------------------------------------------------------------------

1st get logged into both vm’s, we are going to test what happens when a clients computer can’t ping the website due to IP address issues. 


![1st get logged into both vm’s](https://github.com/user-attachments/assets/1e729200-bdf0-480a-9abe-6393cb1e395b)

￼
2nd on Client-1 PC right Clint start > run > type “power shell “ > and enter on the command line. ( ping main frame ). 
The computer will try to ping the mainframe, but it will time out. Because we don’t have a “main frame “ in our cache, or host files or DNS we will give it an IP ADDRESS to ping when searching “ mainframe.
![1st get logged into both vm’s](https://github.com/user-attachments/assets/0b179da9-21ee-4bfa-9a62-66e9003c1130)

￼
3rd, also do “nslookup mainframee, “ and you will see that it does not find the mainframe either
￼![1st get logged into both vm’s](https://github.com/user-attachments/assets/ddb3e091-9aaf-4f9a-aa30-5b5f97d072bd)

4th ok so we will create a “ mainframe “ address for client-1 to ping. 
On DC-1 > start menu > administrative tools > DNS manager. Then in the DNS manager, select DC-1 > forward lookup zones > my domain.com 
￼![1st get logged into both vm’s](https://github.com/user-attachments/assets/b93b83d1-5072-4491-a8c9-2b06b7d983de)

5th ok, now we will make an address under the name “ main frame “ for client 1 to ping. Right click anywhere on the right side of the DNS manager > create new host A or AAAA  > NAME: mainframe and IP address 10.0.0.4 > enter 
![1st get logged into both vm’s](https://github.com/user-attachments/assets/31630db9-4639-4b4b-b74b-49ee4ea2055b)

￼
6th you can see the “ mainframe “ is now in the DNS records with the IP address 10.0.0.4 
￼![1st get logged into both vm’s](https://github.com/user-attachments/assets/f29d5046-ee96-4c8c-aebb-ddc7355a4af3)

7th now back on client-1 in PowerShell, ping mainframe again. And it will ping back. Since the mainframe wasn’t in the cache or local host file, it searched the DNS server, found the mainframe, pinged it, and gave  it back to us
￼![1st get logged into both vm’s](https://github.com/user-attachments/assets/58e81475-51d3-4ab5-a530-52a33112f3bf)

8th, now we are going to change the IP address and observe client-1 finding the new IP address. Double click mainframe > change IP address to 8.8.8.8
![1st get logged into both vm’s](https://github.com/user-attachments/assets/7286d1a7-88ad-40c3-87dd-1d3752a27e74)

￼
9th now back on client-1 we are going to ping the mainframe again and observe what happens. When you ping it you will notice that 10.0.0.4 IP shows up not 8.8.8.8; that is because client-1 is using the cache memory with the old IP. This would be a problem cause now client-1 can’t load the mainframe because the IP address is changed. So we need to update the IP address by flushing the DNS cache 
![1st get logged into both vm’s](https://github.com/user-attachments/assets/a5bcac12-7ca1-48b8-a050-fc877e7a09fc)

￼
10th ok so in powershell type ifconfig /flushDNS this will wipe the cache of the old DNS IP Then try > ping mainframe after flushing the DNS cache, client-1 will then search through local host files, then to the DNS server, where it will see that “mainframe “ got a new IP ADDRESS and reconnect to it using 8.8.8.8
![1st get logged into both vm’s](https://github.com/user-attachments/assets/22ff0292-c5b5-4b3c-b26a-a422a44b4b7f)

￼
11th ok now we are going to create a CNAME record, which is  basically mapping a human readable name to another. Back in DC-1 right click on your DNS server, go to “ create new CNAME “ > we will name the alias “ search, “ and  the fully qualified domain name or ( FQDN ) we will link it to is www.google.com, so whenever we type search in the command line, it will link us to google.com 
![1st get logged into both vm’s](https://github.com/user-attachments/assets/8d557e07-938f-40a6-afe0-665d08579a8c)

￼
12th now back in client-1, we will ping “ search “ and it will give us the IP address to google.com so if it were possible on client -1 we could type search in the browser bar and google.com would pop up but because “ search “ doesnt actually match the real certificate name of google it won’t load because that would be a compromised site possible.  
￼![1st get logged into both vm’s](https://github.com/user-attachments/assets/f37ed45a-e62d-4164-bab3-57a80ff1cd18)







