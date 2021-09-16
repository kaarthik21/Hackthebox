# Explore (IP - 10.10.10.247)
#### We start our Reconnaisance by scanning the open ports with nmap
#### A full scan of this ip tells us thats 5 ports (2222, 5555, 42135, 46465, 59777), additionally it says that the device is a phone
#### (nmap -A -Pn -p- -sN 10.10.10.247)
#### A vuln scan on the machine (nmap --script vuln 10.10.10.247) dosent give us much valuable info
#### Going back to nmap report, port 42135 is running ES File Explorer
#### Now we are opening metasploit (type msfconsole)
#### Now searching for any vulnerabilities related to es file explorer gives us some results(command: search es_file)

![image](https://user-images.githubusercontent.com/52716626/133623934-1289994f-1ffc-4004-9680-0af5c0f6421d.png)

#### We got a vulnerability which can possibly be found in our attacking machine, so next we use it (command: use auxiliary/scanner/http/es_file_explorer_open_port or use id_no [id_no in this case is 0])
#### Now with typing *show options* we can give the input for this payload

![image](https://user-images.githubusercontent.com/52716626/133627248-5cbd0293-956c-4786-b335-a0ad9f00f279.png)

#### So, its asking for RHOSTS (attacker's ip, [LHOST is the host ip]) and RPORT is 59777 default 

![image](https://user-images.githubusercontent.com/52716626/133627470-6b5469b3-b476-4bb8-9067-837a3a991d4f.png)

#### Now by running this, we can know that its a VMWare Virtual Platform

![image](https://user-images.githubusercontent.com/52716626/133628071-b468c391-0680-4507-9a23-85ab5647a4f2.png)

#### By running _show actions_ we can give the specified inputs listed
#### By running (set action LISTAPPS and running it) we get the list of apps installed

![image](https://user-images.githubusercontent.com/52716626/133628845-e320f61f-9dcb-4a0d-8ff5-3ce26458fd3b.png)

#### We check for photos and videos with the same method

![image](https://user-images.githubusercontent.com/52716626/133629166-3ea0e6b2-97bb-4bae-b36d-352d9251c03f.png)

#### Now to view these photos, we ave to download it to our host system with the GETFILE command

![image](https://user-images.githubusercontent.com/52716626/133629764-d1bdbb9f-a765-4d16-b0d8-dc9cd0dd97ea.png)

#### Like the above sequence of commands, we download all the photos and cheching these we got something interesting on creds.jpg

![image](https://user-images.githubusercontent.com/52716626/133630681-1d6c4f88-6462-4398-b31e-397f4b9f1a71.png)

#### [PS: I thought this might be the one and tried inputting the second line as the flag :/ ]
#### Now we know kristi is a user, so we connect to ssh with *ssh -L 5555:localhost:5555 kristi@10.10.10.247 -p 2222* and the password as the second line  

![image](https://user-images.githubusercontent.com/52716626/133634627-bc63269f-3beb-48c0-a726-200c235a9d66.png)

#### Now we search for the flag in some file and boom we found the first flag

![image](https://user-images.githubusercontent.com/52716626/133633365-86f0bc40-5fe5-4e1a-b1d6-e75013d8c8e0.png)

#### Our next step is to get root privilages to access the unprivilaged directories
#### We initially found that this device is a android phone so we use adb(android debug bridge) to get root access
#### In our previous ssh command we made a local connecting shell at localhost:5555, we connect adb on localhost:5555 to get our shell
#### adb connect localhost:5555
#### adb shell, Or if "error: more than one device/emulator" arrives then adb -s localhost:5555 shell should do the job
#### After getting into the shell typing su will make u root user

![image](https://user-images.githubusercontent.com/52716626/133638175-8f9ecdcc-a3f0-48f5-9b6a-1f35aba12714.png)

#### We got root privilages now, check /data/root.txt found the 2nd flag.

![image](https://user-images.githubusercontent.com/52716626/133638307-064cf594-de4a-43ef-9c55-841b3918f94b.png)

## And Explore machine is pwned


