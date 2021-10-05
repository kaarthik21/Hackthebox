## BountyHunter - (IP - 10.10.11.100)

![image](https://user-images.githubusercontent.com/52716626/136045891-11cdad57-9ed1-49a7-ae2c-a411ec2ca0bb.png)


#### For this machine, we are provided the IP address. When we do the NMAP scan, the standard 22(ssh) and 80(http) portsare found open and stored onto file nmap.txt

![image](https://user-images.githubusercontent.com/52716626/136048219-2db26748-a552-44c5-a8e0-4a902b74e76a.png)


#### As 80 port is open, we can view the website. The Menu consists of About, Contact and Portal. The Portal tab is suspicious as it redirects to a different page
#### In the log_submit.php page, we can give inputs to the machine. Using burp we see the requests we send
#### Lest us input 'test' in all four columns, then burp will output us a data variable with random string



#### When I tried decoding this string with MD5, Base64, etc Base64 seemed successful as we get an XXE script. When again decoding it with URL decoder we get a readable XXE which is same as the input we give
#### Now we are going to try the XXE Injection to get the forbidden files and subdomains of the machine
#### *xxe-attact-db.php.txt* file contains the script to be injected to view db.php 



#### Now we need to encode this into Base64 and URL encode format, then sending the resulting string into data column in the repeater of Burp will give us a encoded string again
#### Again decoding this string will give us the db.php file (which is stored into db_output_by_changing_data.php file)
#### Using the above same format, we try to access the /etc/passwd file of the machine to get the possible usernames (admin username mentioned in db.php file dosent work - mysql username)



#### Encoding xxe-etc-passwd.txt and sending through data var will give us the etc-passwd_output.txt
#### So /bin/bash has usernames root, development



#### So we connect to ssh (22 port) with the command: **ssh development@10.10.11.100** and entering the correct password found from db.php file will make successful login



#### The user.txt file in /home/development seems suspicious ;)



#### The next aim for us is to attain root privileges
#### The command **sudo -l** gives us the files and programs using root privileges without sudo



#### So /opt/skytrain_inc/ticketValidator.py is the script we should be using to get to root user 
#### Going through the python script, the input should be as mentioned to get the ticket otherwise the output will be "invalid ticket"
#### So a file "somefile.md" is written according to the script's expectations. **import('os').system('/bin/bash')** is the ingredient int he file to get us root



#### Now we are rooted, check /root/root.txt :)
