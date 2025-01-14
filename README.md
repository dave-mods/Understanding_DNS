# Understanding DNS
![output](https://github.com/user-attachments/assets/35580a2f-c6c3-418d-8f0c-a17b4bc0a9f0)

# Description
In this pratice we will see how DNS can take a human readable website (www.google.com) and turn it into a IP address that computers use to identify each other. When you search a website in your browser DNS directs your request to the right server allowing access to websites.
# Enviorments and Utilities Used
 - Microsoft Azure
 - Virtual Machines
 - Remote Desktop
 - PowerShell
# Operating Systems Used
 - Windows 10
# Project Walk-through
This is a slight continuation of Active Directory. If You need help setting up the VM's start there. We will use the client-1 and dc-1 VM's created there. Log into both VM's using the mydomain.com\jane_admin, Cyberlab123!.
![image](https://github.com/user-attachments/assets/c0570a73-1a3e-4fa9-ac2e-4e4f9a7882d9)

In client-1 we will use the search bar in the bottome left and type in PowerShell and open it. In PowerShell type "ping mainframe" and hit enter. So the 3 steps we dont see but how it works is first it checks Local DNS cache (fastest). Second it checks Local Host File (faster). Third is the DNS server (slowest).
![image](https://github.com/user-attachments/assets/d4e7fa67-dcc0-4e16-a84a-19f90a800f38)

To see the local DNS cache we can type in "ipconfig /displaydns". If we look through all this info we will not find a cache of "mainframe" in the DNS.
![image](https://github.com/user-attachments/assets/0001d74c-31de-433d-b4cd-57eec8e56a03)

If we want to see the local host file we can open notepad and run it as administrator> file> open> bottom right change Text Doc to All files. Look for the drivers folder> ect> hosts.
![image](https://github.com/user-attachments/assets/ed40e643-8b8a-4682-b023-093e06088560)

Back in PowerShell if you try and "ping zebra" the ping will fail. So in the host file we will add the loop back IP address and put zebra next to it. Then save it and try to ping zebra again and it will work. This worked because when the computer looked in the cache zebra wasent there. Then it checked the host file and found zebra so it was able to respond to the ping and not fail.
![image](https://github.com/user-attachments/assets/76937206-c3ae-4c64-af92-3ceda6ad643e)







































