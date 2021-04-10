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
	![image](https://user-images.githubusercontent.com/71016915/114277339-66ec0a00-9a48-11eb-9523-41382e86f59b.png)

	After trying the password for all user's. We can see the the password [REDACTED] works for the user [REDACTED].	
	

