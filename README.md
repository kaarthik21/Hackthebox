# Cap (IP - 10.10.10.245)

![image](https://user-images.githubusercontent.com/52716626/133619684-54c392a5-a33f-4015-930a-15c1fbdd862d.png)

#### Given the IP address of the machine, we first scan for open and filtered ports with the using of nmap command as follows:
#### nmap -sN -Pn -p- -A 10.10.10.245 and add that into a file called nmap.txt
#### From the scan report we can say that ports 21(ftp), 22(ssh) and 80(http) are open and the operating system is found to be Unix, Linux; CPE: cpe:/o:linux:linux_kernel

![image](https://user-images.githubusercontent.com/52716626/133619960-e84c6e35-6bab-4926-9f1d-f35bc8066179.png)

#### For connecting through ssh on port 22, we need to find some user credentials through reconnaisance.
#### So we use *Gobuster* (tool to find all the sub-domains with a url) with the following command:
#### gobuster dir -u https://10.10.10.245 -w /usr/share/dirb/wordlists/big.txt
#### From this report, we can say that /capture, /data, /ip, /netstat subdomains seems to exist
#### By visiting through all these sites, /data subdomain seems interesting as it contains some downloadable files.

![image](https://user-images.githubusercontent.com/52716626/133620464-71c4887f-16c5-42ca-8584-97d39e56f563.png)

#### We download the pcal files with id=0 & id=1 with are 0.pcap and 1.pcap
#### Using the strings command on 0.pcap file (biggest file), we can find the username and password of a specific user

![image](https://user-images.githubusercontent.com/52716626/133620576-9b69546c-f7d1-4c7d-a5c2-99c57b7b944b.png)

#### (This can also be found out using a tool called Wireshark)
#### Give the username and password, we try to login into port 22(ssh) with the found credentials and we are successful **(ssh nathan@10.10.10.245 and enter the password)**
#### After logging in, /home/nathan dir has a suspicious file user.txt which contains our first flag
#### So now we have accessed nathan's machine and our next aim is to escalate to root privilages
#### There is also a script called linpeas.sh which is the pentesting script and gives us valuable info when run
#### So, by running ./linpeas.sh we found the sudo, python and some other versions are outdated and the binary has linux cap_setuid set
#### A google search of "Gtfobins python3.8 privilage escalation" will make us run the following command (https://gtfobins.github.io/gtfobins/python/#capabilities):
#### ./python -c 'import os; os.setuid(0); os.system("/bin/sh")'

![image](https://user-images.githubusercontent.com/52716626/133621057-06f8a7fb-d0ea-4123-a17f-112b81f8ac69.png)

#### We are now root, now search for all the dir which previously asked for root privilages like root/, lost+found/, etc
#### Fortunately /root conatains a file called root.txt which contains the flag
# We have pwned the Cap machine successfully
