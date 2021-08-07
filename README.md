# Super-Spam Official Walkthrough

Room link - https://tryhackme.com/jr/superspamr
 ---
 
 **(Note to tester: the machine needs more resources to speed up this time)**
 
This machine can take 5-10 minutes to setup. 

Good luck Have fun solving it :)

---

 - #### Let's scan the machine with rustscan for open ports
	`rustscan -a 10.10.103.218`

	![image](https://user-images.githubusercontent.com/71016915/114276846-18d60700-9a46-11eb-9b7c-86dcaf901a40.png)
	
	We can see 5 open ports 80, 4012, 4019, 5901 and 6001
	
	Lets use NMAP to perform version detection
	
	`nmap -sV 10.10.103.218 -p 80,4012,4019,5901,6001 -T4`
	
	![image](https://user-images.githubusercontent.com/71016915/114276947-939f2200-9a46-11eb-8074-7940d622ba13.png)


	
	**_Port 80 = HTTP_**
	
	**_Port 4012 = SSH_**
	
	**_Port 4019 = FTP_**
	
	**_Port 5901 = VNC_**
	
	
	---
	
	#### Q1. What CMS and version is being used? (format: wordpress x.x.x)
	
	`nmap -A -p 80 -T4 ip`
	
	![image](https://user-images.githubusercontent.com/71016915/114291631-a6494380-9aa6-11eb-9152-6a7f5bc71187.png)

	---
	
- ### FTP
	Lets login in FTP with **anonymous**
	
	`ftp  10.10.103.218 4012`
	
	![Pasted image 20210228103031](https://user-images.githubusercontent.com/71016915/109417999-08b11d80-79ec-11eb-8ce7-0295fc5d493c.png)


	We can see a `note.txt`
	
	![Pasted image 20210228103159](https://user-images.githubusercontent.com/71016915/109418003-0e0e6800-79ec-11eb-9c21-5e58637dc94e.png)

	 
	 We can see on line of ***13th January*** that adam has included a *Wireshark* file
	 
	 Lets do a `ls -a` to see if there are any hidden files
	 
	 ![Pasted image 20210228103539](https://user-images.githubusercontent.com/71016915/109418009-149cdf80-79ec-11eb-9a1b-617a9ada60f3.png)

	 
	 Yes!! We were right
	 
	 There is a *hidden directory* called **_`.cap`_**
	 
	 Lets navigate into that directory and check its contents
	 
	 ![image](https://user-images.githubusercontent.com/71016915/128585623-a4243dee-8055-4eec-b96f-3bc1b1f32d07.png)

	 
	 We can see a file called SamsNetwork.cap, we can download on our machine.
	 
	 `-rwxr--r--    1 ftp      ftp        370488 Feb 20 14:46 SamsNetwork.cap`
	 

	 `get SamsNetwork.cap` - To download it in our machine.
	 
	 ![Pasted image 20210228105101](https://user-images.githubusercontent.com/71016915/109419253-91cb5300-79f2-11eb-8642-79deb0b6f80b.png)

	 
	 It is now downloaded in our machine.
	 Lets use *[aircrack-ng](https://www.aircrack-ng.org/)* to crack it.
	 
	 `aircrack-ng SamsNetwork.cap -w /path/to/rockyou.txt`
	 
	 
	 After some time we can see that it **_successfully_** cracked the password.
	
	 ![Pasted image 20210228133528](https://user-images.githubusercontent.com/71016915/109419250-8d9f3580-79f2-11eb-8732-983ee4e10edb.png)

	 
	 Password : ***[REDACTED]***
	
	 Now we have a password!!
	 
	 
	 ---

  - ### Website


	Lets check out the *website*.
	
	![image](https://user-images.githubusercontent.com/71016915/114277108-7585f180-9a47-11eb-9103-7e41a765222f.png)
	
	Lets try finding username's in the website
	
	![image](https://user-images.githubusercontent.com/71016915/114277145-a0704580-9a47-11eb-890a-b21c083056c1.png)

	Click on **Blog**

	After viewing all the blogs, I found these usernames
	 -	***Adam_Admin***
	 -	***Benjamin_Blogger***
	 -	***Donald_dump***
	 -	***Lucy_Loser***
	
	We have 1 password and 4 users. Let's try logging in.
	
	Scroll all the way down
	![image](https://user-images.githubusercontent.com/71016915/114277360-7b300700-9a48-11eb-8b73-ab32665477ea.png)


	After trying the password for all user's. We can see the the password [REDACTED] works for the user [REDACTED].	
	
	![image](https://user-images.githubusercontent.com/71016915/114277411-b6323a80-9a48-11eb-8d3b-a9910606d244.png)
	

	After login we can see this page.
	![image](https://user-images.githubusercontent.com/71016915/114277449-dc57da80-9a48-11eb-98f7-bc122325af1d.png)
	
	`http://ip/concrete5/index.php/dashboard/welcome`
	
	Now just change the url to `ip`
	![image](https://user-images.githubusercontent.com/71016915/114277581-5f793080-9a49-11eb-8bc5-dddfab22972b.png)
	
	Scroll a bit down and click on any of them.
	![image](https://user-images.githubusercontent.com/71016915/114277604-6e5fe300-9a49-11eb-882a-9b0874adc965.png)
	
	You will see these button's on the top right
	
	![image](https://user-images.githubusercontent.com/71016915/114277687-dadae200-9a49-11eb-9e1e-b6f141dab9a7.png)
	
	Click on
	
	![image](https://user-images.githubusercontent.com/71016915/114277702-ee864880-9a49-11eb-9be0-ac3c2fc75d59.png)
	
	And you will see  this

	![image](https://user-images.githubusercontent.com/71016915/128586933-e88d38ab-4951-4dc2-81a1-3210db2ce360.png)
	
	Click the System and Settings
	
	![image](https://user-images.githubusercontent.com/71016915/114277739-22fa0480-9a4a-11eb-8014-e3ff85c94b28.png)
	
	Click on **File Manager** under **Files** section
	
	![image](https://user-images.githubusercontent.com/71016915/114277798-5d63a180-9a4a-11eb-985a-e2ab9becb682.png)

	You will see a site like this
	![image](https://user-images.githubusercontent.com/71016915/114277864-a156a680-9a4a-11eb-9709-11aebc9b2e6e.png)
	Click on `upload file`
	
		
	Upload a PHP revserse shell. You can find it [here](https://github.com/pentestmonkey/php-reverse-shell).
	
	Download this on your machine and change the ip address in it using a text edittor
	
	Now lets upload this in the website and set up a listener on *our*  machine :  `nc -lvnp 1234` (replace 1234 with the port you have changed in the php reverse shell script)
	
	![image](https://user-images.githubusercontent.com/71016915/114291777-a564e180-9aa7-11eb-9d54-7a2b7ff3c5a5.png)


	Looks like `.php` extension is an invalid extension
	
	Lets fix that.
	
	![image](https://user-images.githubusercontent.com/71016915/114278719-b9c8c000-9a4e-11eb-82ac-177523e7a080.png)
	
	
	![image](https://user-images.githubusercontent.com/71016915/114278749-d238da80-9a4e-11eb-8193-0471ed7a4b67.png)

	Add `, php` at the end 

	![image](https://user-images.githubusercontent.com/71016915/114278804-0dd3a480-9a4f-11eb-8497-bee8ff050ef9.png)
	
	Click on Save
	
	Now lets try to upload again
	
	![image](https://user-images.githubusercontent.com/71016915/114278834-3bb8e900-9a4f-11eb-817f-be17d5e30028.png)

	
	Click on `Upload Files` on the top right
	
	![image](https://user-images.githubusercontent.com/71016915/114291733-57e87480-9aa7-11eb-8eb2-d2e2c1ed078f.png)


	Boom!! Its successfull!
	
	Now lets click `close`.
	
	Now set up a listener `nc -lvnp 1234`(replace 1234 with the port you have changed in the php reverse shell script)
	
	

	![image](https://user-images.githubusercontent.com/71016915/114295659-3fd31e00-9ac4-11eb-9a68-92ea6968820f.png)
	
	
	After you click on that link. You will get a reverse shell.
	
	Lets upgrade our shell by using `python3 -c 'import pty;pty.spawn("/bin/bash")'`
	
	![image](https://user-images.githubusercontent.com/71016915/114296530-7ceddf00-9ac9-11eb-81a7-bcf72081004d.png)

	
	Navigate to `/home/personal`.
	
	There are a lot of files
	
	![image](https://user-images.githubusercontent.com/71016915/114296562-ac9ce700-9ac9-11eb-8f4c-b97c929e3e98.png)

	
	To find the user flag we need to `grep` through all the files - `cat * | grep flag`
	
	![image](https://user-images.githubusercontent.com/71016915/114296577-cc340f80-9ac9-11eb-92c4-fcabef9a0d1c.png)

	
	Now go to `/home/lucy_loser` and list the files
	
	![image](https://user-images.githubusercontent.com/71016915/114296680-3d73c280-9aca-11eb-82dd-103b6874a84a.png)


	
	We see an **hidden** directory `.MessagesBackupToGalactic`. Let's navigate to that folder and view files
	
	![image](https://user-images.githubusercontent.com/71016915/114296651-174e2280-9aca-11eb-9616-e3b7f9e46aab.png)
	
	
	We can see a lot of `png`, a python file and a `note.txt`
	
	
	#### Q3. What type of encryption did super-spam use to send his encrypted messages?
	

	![image](https://user-images.githubusercontent.com/71016915/114379829-edb6f900-9ba6-11eb-83c4-ff340b318112.png)

	
	
	Lets download all the `png`s to our machine using a python3 http server - `python3 -m http.server 2222`. 
	Remember to run it in while you are in the `.MessagesBackupToGalactic` folder.
	
	![image](https://user-images.githubusercontent.com/71016915/114296808-eae6d600-9aca-11eb-8700-0685fcf0ed43.png)

	
	
	To get it on your machine follow these steps
	
	
	```
	wget ip:2222/c1.png
	wget ip:2222/c2.png
	wget ip:2222/c3.png
	wget ip:2222/c4.png
	wget ip:2222/c5.png
	wget ip:2222/c6.png
	wget ip:2222/c7.png
	wget ip:2222/c8.png
	wget ip:2222/c9.png
	wget ip:2222/xored.py
	```
	
	Now Lets run the `xored.py` script.
	For it to work you would need PILLOW installed
	
	`pip3 install pillow`
	
	Lets run the script.
	
	![image](https://user-images.githubusercontent.com/71016915/114296245-dd7c1c80-9ac7-11eb-846f-6132e87c625b.png)


	It requires 2 png files. After trying a lot i found out `c2.png` and `c8.png` worked
	
	![image](https://user-images.githubusercontent.com/71016915/114296281-13b99c00-9ac8-11eb-84fe-6f83079ec6bd.png)
	
	Lets view our newly made file `c28.png`
	
	#### Q4. What key information was embedded in one of super-spam's encrypted messages?
		[redacted]
		
	![image](https://user-images.githubusercontent.com/71016915/114296317-57140a80-9ac8-11eb-84ae-c0951ebea51d.png)
	
	We can find a password
	
	After trying each user i found out the we could use the user `donalddump` to login using that password.
	
	![image](https://user-images.githubusercontent.com/71016915/114297507-a78e6680-9ace-11eb-861f-d7601dd647ba.png)
	
	Let's SSH into donalddump
	
	`ssh donalddump@ip -p 4012`  and password which you found in the image
	
	![image](https://user-images.githubusercontent.com/71016915/114297605-153a9280-9acf-11eb-8a6a-8dde4f9529b8.png)
	
	Lets browse in our home directory
	
	![image](https://user-images.githubusercontent.com/71016915/114297618-297e8f80-9acf-11eb-9aad-3c599cdadb55.png)

	Permission denied - Lets try to change the permissions of the `/home/donalddump` - `chmod 777 /home/donalddump`
	
	![image](https://user-images.githubusercontent.com/71016915/114297647-6ea2c180-9acf-11eb-9101-d2671636af9f.png)
	
	---
	
	- ### Getting Root
	
	---
	
	Now we remember we had VNCviewer running as we found in our nmap scan.
	
	Lets try to gain root using that
	
	Grab VNC passwd file from donaldump's directory 
	
	![image](https://user-images.githubusercontent.com/71016915/114300323-63ef2900-9add-11eb-8435-5fb7eafc1010.png)
	
	Transfer this using `python3 -m http.server 1234`
	
	On your machine - `wget ip:1234/passwd`
	
	![image](https://user-images.githubusercontent.com/71016915/114300433-f1327d80-9add-11eb-9af5-17feab92e80d.png)


	Do port forwarding with ssh as you know **donald_dump**'s password  , `ssh -L 5901:localhost:5901 donalddump@ip -p 4012`
	
	![image](https://user-images.githubusercontent.com/71016915/114300582-abc28000-9ade-11eb-97e3-7bf8b8ac4c43.png)

	---
	
	#### Note : Make sure you have VNCviewer installed, if not please install it using this [link](https://www.realvnc.com/en/connect/download/viewer/linux/).
	Or install it using: `sudo apt install tigervnc-viewer`
	
	
	Run - `vncviewer -passwd passwd_file ip::5901`  on your machine.
	
	- passwd_file was the one we transfered from Superspam to our machine which was in the donalddump home directory.
	
	![image](https://user-images.githubusercontent.com/71016915/114301063-a9f9bc00-9ae0-11eb-9f18-dc8e7d2e2cde.png)

	Let's change the root password and then log in as root from `ssh`
	
	`passwd`
	
	![image](https://user-images.githubusercontent.com/71016915/114301129-f80ebf80-9ae0-11eb-9c9e-f799ca64bb26.png)

	I changed the password to `1234`
	
	Lets do `su root` in the donalddump ssh shell
	

	![image](https://user-images.githubusercontent.com/71016915/114301206-7b301580-9ae1-11eb-9a3d-876186b69f47.png)

	We see a unusual hidden directory called `.nothing`
	
	Lets browse in it and check its content.
	
	![image](https://user-images.githubusercontent.com/71016915/114301230-9864e400-9ae1-11eb-90f9-39d744b5592c.png)
	
	We see a `r00t.txt` file. Lets get the contents of the file -  `cat r00t.txt`
	
	![image](https://user-images.githubusercontent.com/71016915/114301267-c5b19200-9ae1-11eb-8e66-411e3ad90891.png)


	By copying the **what am i?** string and paste it in [CyberChef](https://gchq.github.io/CyberChef/)
	
	![image](https://user-images.githubusercontent.com/71016915/114301483-c39c0300-9ae2-11eb-972f-6d9dc163da11.png)
	
	
	Now we get the decoded string which has the root flag!

	![image](https://user-images.githubusercontent.com/71016915/114301423-6b650100-9ae2-11eb-8d01-4ce537fb72b1.png)
	
	
	Root_flag: flag{REDACTED}
	
	---
	
	
	#### Just for fun, I decoded the other string and this is the decoded string.
	
	![image](https://user-images.githubusercontent.com/71016915/114301647-6fdde980-9ae3-11eb-889e-cc1271c0fdf5.png)

		
	![image](https://user-images.githubusercontent.com/71016915/114301611-44f39580-9ae3-11eb-8461-fa7f3b81a3d9.png)

	
	---
	#### ***Thank You for reading my writeup. Hope you enjoyed it!!***
