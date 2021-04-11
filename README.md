# Super-Spam Official Walkthrough
 ---
 
 **(Note to tester: the machine needs more resources to speed up this time)**
 
This machine can take 5-10 minutes to setup. Sometimes after 30 minutes the machine boots up, the web server can have an issue, in that case please reboot the machine, we recommend machine notes so that u dont have to do forget what u did, thanks :)

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
	
- #### FTP
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
	 
	
	 ![image](https://user-images.githubusercontent.com/71016915/114277040-10ca9700-9a47-11eb-838b-9b0c1dfb4441.png)


	 
	 Lots of files, but if u see the file size, there are huge files and some ***0 bytes*** files, so i don't think they shall be the files we need.
	 
	 We can see a file with **_370488 bytes_** which looks like it is a legit file we can download on our machine.
	 
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

- #### Website
	Lets check out the *website*.
	
	![image](https://user-images.githubusercontent.com/71016915/114277108-7585f180-9a47-11eb-9103-7e41a765222f.png)
	
	Lets try finding username's in the website
	
	![image](https://user-images.githubusercontent.com/71016915/114277145-a0704580-9a47-11eb-890a-b21c083056c1.png)

	Click on **Blog**

	After viewing all the blogs, I found these usernames
	 -	***Adam_Admin***
	 -	***Benjamin_Blogger***
	 -	***Donalddump***
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
	
	And you will see a site like this
	
	![image](https://user-images.githubusercontent.com/71016915/114277739-22fa0480-9a4a-11eb-8014-e3ff85c94b28.png)
	
	Click on **File Manager** under **Files** section
	
	![image](https://user-images.githubusercontent.com/71016915/114277798-5d63a180-9a4a-11eb-985a-e2ab9becb682.png)

	You will see a site like this
	![image](https://user-images.githubusercontent.com/71016915/114277864-a156a680-9a4a-11eb-9709-11aebc9b2e6e.png)
	Click on `upload file`
	
		
	Upload a PHP revserse shell. You can find it [here](https://github.com/pentestmonkey/php-reverse-shell).
	
	Download this on your machine and change the ip address in it using a text edittor
	
	Now lets upload this in the website and set up a listener on *our*  machine - `nc -lvnp 1234` (replace 1234 with the port you have changed in the php reverse shell script)
	
	![image](https://user-images.githubusercontent.com/71016915/114291777-a564e180-9aa7-11eb-9d54-7a2b7ff3c5a5.png)


	Looks like .php extension is an invalid extension
	
	Lets fix that.
	
	![image](https://user-images.githubusercontent.com/71016915/114278719-b9c8c000-9a4e-11eb-82ac-177523e7a080.png)
	
	
	![image](https://user-images.githubusercontent.com/71016915/114278749-d238da80-9a4e-11eb-8193-0471ed7a4b67.png)


	![image](https://user-images.githubusercontent.com/71016915/114278804-0dd3a480-9a4f-11eb-8497-bee8ff050ef9.png)
	
	Click on Save
	
	Now lets try to upload again
	
	![image](https://user-images.githubusercontent.com/71016915/114278834-3bb8e900-9a4f-11eb-817f-be17d5e30028.png)

	
	Click on `Upload Files` on the top right
	
	![image](https://user-images.githubusercontent.com/71016915/114291733-57e87480-9aa7-11eb-8eb2-d2e2c1ed078f.png)


	Boom!! Its successfull!
	
	Now lets click `close`.
	
	Now set up a listener `nc -lvnp 1234`(replace 1234 with the port you have changed in the php reverse shell script)
	
	![image](https://user-images.githubusercontent.com/71016915/114278943-a9fdab80-9a4f-11eb-9a21-aac06af1fefe.png)
	
	![image](https://user-images.githubusercontent.com/71016915/114278906-820e4800-9a4f-11eb-954a-1ea962eb3425.png)
	
	After you click on that link. You will get a reverse shell.
	
	![image](https://user-images.githubusercontent.com/71016915/114279027-33ad7900-9a50-11eb-8462-96184c647197.png)
	
	Navigate to `/home/personal`.
	
	There are a lot of files
	
	To find the user flag we need to `grep` through all the files - `cat * | grep flag`
	
	![image](https://user-images.githubusercontent.com/71016915/114291839-3471f980-9aa8-11eb-90d5-15ea8ef5bbb5.png)

	Now go to `/home/lucy_loser` and list the files


	![image](https://user-images.githubusercontent.com/71016915/114291947-c5e16b80-9aa8-11eb-8be3-e4e3305f4bd4.png)
	
	Q3. What type of encryption did super-spam use to send his encrypted messages?

	![image](https://user-images.githubusercontent.com/71016915/114291969-f4f7dd00-9aa8-11eb-98b2-5e5a10198d73.png)
	


