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

If we go back to PwerShell and type in "nslookup mainframe" we will see it cant find it. So lets work on this. Go to our dc-1 DNS server.
![image](https://github.com/user-attachments/assets/250cd226-19bd-41dd-bf6d-4c447389903b)

In dc-1 in the bottom left search bar type in DNS and bring up the DNS Manager.
![image](https://github.com/user-attachments/assets/2b50b429-272c-4563-8991-cb4d58647425)

Expand dc-1> Forward Lookup Zones> mydomain.com. In here we can see the A-records for dc-1, client-1 ect.
![image](https://github.com/user-attachments/assets/f578a3ed-6d2a-44eb-a9e7-b8c7a1503b63)

Now we need to open PowerShell in dc-1 and type in "ipconfig" and get the private IP for dc-1.
![image](https://github.com/user-attachments/assets/79c6d52f-ef61-414b-a86a-9c73d7149855)

Now that we have the IP we can go back into DNS Manager right clcik and selsect New Host (A or AAAA) name it mainframe with the IP address we got from PowerShell and Add Host. Now we can see we have made a, A record for mainframe.
![image](https://github.com/user-attachments/assets/b756540e-42e6-4b65-9643-f1f64c182e29)
![image](https://github.com/user-attachments/assets/65ac532d-162b-40c1-ab0d-677ef3b5af1d)

Back in client-1 if we ping mainframe we will now get a response. So here in the 3 steps there was no cache, nothing in the host file, but we added this to the DNS server so when it reached out to the DNS it got the response for mainframe being pinged.
![image](https://github.com/user-attachments/assets/667b50d6-eddf-4ac6-be3f-e09316970b1b)

Now lets go back to dc-1 and change mainframes A record IP address to 8.8.8.8 and see what happens.
![image](https://github.com/user-attachments/assets/96a3d473-8b60-4dd6-8139-430de6ca4f63)

Back in client-1 when we ping mainframe now it still pulls up the 10.0.0.4 IP and not 8.8.8.8. This is because therre was a cache with the 10.0.0.4 IP. Even though we have changed it. We can see the chache if we do "ipconfig /displaydns"
![image](https://github.com/user-attachments/assets/29d5f543-dbc0-4f5d-b358-76d7775a7b47)
![image](https://github.com/user-attachments/assets/64c7c979-d72b-4602-a5d5-f512bb5bd5f3)

Now lets flush the cache and see what we get. Type in "ipconfig /flushdns" (note you might have to run Powershell as admin if it wont work). Our cache is now empty.
![image](https://github.com/user-attachments/assets/424d9739-4124-436d-ae25-6b38c6005860)
![image](https://github.com/user-attachments/assets/779f24af-f0a6-4ed5-ac8a-9b762e5c2d55)

So now we can ping mainframe and see what we get for the IP. So as we can see we got the new 8.8.8.8 IP that we gave mainframe.
![image](https://github.com/user-attachments/assets/bdf26ac9-5107-455b-a9d6-a223f508d94c)

Lets head back to dc-1 and try a CNAME change. Right clcik and select New Alias (CNAME). We will name it "search" and point it to "www.google.com" (this is just for practice).
![image](https://github.com/user-attachments/assets/4ff6136c-70e3-4980-baab-6d7c7bd3f063)
![image](https://github.com/user-attachments/assets/72d24de3-c6c9-468f-afaa-2ad981ceabe6)

In client-1 lets ping search and see what we get. We will see that it pinged google. Now if we do "nslookup search" it will give us the IP addess for google and our Alias we set to our domain.
![image](https://github.com/user-attachments/assets/15b40618-b07f-4a71-9e10-1a6c0d7a9ac4)
![image](https://github.com/user-attachments/assets/c65b5ef4-434c-4668-8868-caab69a25201)

This concludes our understanding of DNS. We seen how we can give a physical name to a computer or website and how our computer takes that name and converts it to a IP addess to find it and communicate with it.














