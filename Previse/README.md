# Previse - (IP - 10.10.11.104)

![image](https://user-images.githubusercontent.com/52716626/136501611-ad8183e0-f7ef-485d-8410-7495aa4ed8d6.png)

#### Given the machine's IP, we do the nmap and gobuster scans and store it in nmap.txt and gobuster.txt
#### From the report, the ports 22(ssh) and 80(http) are open
#### Visiting the website at port 80, it is a login page
#### Trying SQL injection dosent work, so we first need to register in order to login
#### A lot of subdomains (.php) were found using gobuster (gobuster dir -u http://10.10.11.104 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt)

![image](https://user-images.githubusercontent.com/52716626/136501779-cff99b3b-6e3d-4058-9115-e7820b12871d.png)

#### Visiting all the subdomains, nav.php subdomain is found and have status code: 200, so this site is loading and shows the menu of options

![image](https://user-images.githubusercontent.com/52716626/136501910-ab9758f9-222c-4bf5-a8f9-cfec3a1ff1e8.png)

#### In 'Accounts', there is a 'Create account' option which cannot be accessed
#### Next we are using burp to intercept the request to visit the unaccessable site
#### After getting to nav.php, we we intercept nav.php and Right click->Do Intercept->Response to this request->Forward
#### After this, we can see that the request has 302 status code which is redirecting us. By changing the status code to 200 (to view the site). and forwarding it we will be able to visit the page 'accounts.php' where we can create an account and login

![image](https://user-images.githubusercontent.com/52716626/136502036-1160b491-57ff-4799-8e71-25d6d9eb80e6.png)

#### Now we should be redirected to accounts.php, where we can create our username and password

![image](https://user-images.githubusercontent.com/52716626/136502148-8614a7e2-bdc7-494f-ad33-61b80f46738e.png)

#### Now we are logging in with our previously created user and pass

![image](https://user-images.githubusercontent.com/52716626/136502232-5f01bf98-03fa-40dc-b7a3-f3d775fa974a.png) 

#### In 'files' tab, we can download a list of files. SITEBACKUP.zip may contain some valuable info so we download it and unzip it and store the files into siteBackup directory
#### Now files or commands can be input or injected to favour us
#### We can upload our files in 'files' section. But the system dosent run these files neither are stored in the same directory
#### Another way of running commands can be through os injection which can be done in 'MANAGEMENT MENU->LOG DATA' column where we can run cmds in place of the delimitor
#### Using Burp, we view the request by sending delimitor as comma

![image](https://user-images.githubusercontent.com/52716626/136502926-58f3574d-a584-4c2f-9661-ea1d5946798f.png)

#### We now can modify the delim variable 
#### In the logs.php file, we can see that Python is used to run the script '/opt/scripts/log_process.py' with delim as the input
#### The delim variable in the above picture(within burp) is replaced with 'payload_unicode_encoded' file 

![image](https://user-images.githubusercontent.com/52716626/136504344-737a2781-6de9-4140-a63e-5e7ea7c2b4a7.png)

#### payload_unicode_encoded is encoded form of payload_raw. payload_raw file contains script in python which when executed runs the OS commands
#### After modifying the delim variable, we need to run 'nc -vlp 9999' in our Host machine's command line to get a reverse shell
#### (The python code injected will return us a reverse shell only when we are listening to that port)


