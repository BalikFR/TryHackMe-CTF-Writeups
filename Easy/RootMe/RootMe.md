## RootMe!

<img align="right" src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/logo.png" width="150" height="150">

A ctf for beginners, can you root me?

CTF Link : [**RootMe**](https://tryhackme.com/room/rrootme)

# Steps

1 - Connect to TryHackMe network and deploy the machine.

2 - Scan the ports using **nmap**.

Execute the following command:

* ```# nmap -A <target IP>```

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/nmapscan.png">

Based on the nmap scan results, we have determined that ports **22 (SSH)** and **80 (HTTP)** are open on the target machine, showcasing the presence of these services.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/responseports.png">

We will now explore the website hosted on port **80** of the target machine.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/siteport80.png">

3 - What version of Apache is running?

By utilizing the **nmap** scan with the **"-A"** argument, we were able to gather detailed information about the target machine, including the version of Apache running on port 80. The scan revealed that **Apache version 2.4.29** is in use.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/apacheversion.png">
<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/responseapache.png">

4 - What service is running on port 22?

By utilizing nmap to scan the target machine, we were able to identify the service running on port **22**. The scan results revealed that port 22 is associated with the **SSH (Secure Shell)** service.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/port22open.png">
<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/responseport22.png">

5 - Find directories on the web server using the GoBuster tool.

To discover directories on the web server located at `<target IP>`, we utilized the **GoBuster** tool with the **`gobuster dir`** command. By specifying the target URL using the **`-u`** flag and the wordlist file **`/usr/share/dirb/wordlists/common.txt`** using the **`-w`** flag, GoBuster initiated a **directory brute-force scan**, searching for hidden directories on the web server.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/gobusterresult.png">

We discovered a **"panel"** directory during the **GoBuster** directory scan on the web server.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/responsepanel.png">

6 - Getting a shell.

We will now navigate to the **`/panel/`** directory on the target machine's website.

The **`/panel/`** directory on the website offers us the functionality to **upload files**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/uploadfilepanel.png">

In the file upload section, I will utilize a **PHP-based reverse shell** that I obtained from the GitHub repository at [**https://github.com/pentestmonkey/php-reverse-shell**](https://github.com/pentestmonkey/php-reverse-shell). This reverse shell script will enable me to **establish a remote connection** to the target machine, providing me with interactive shell access and control. I will update the reverse shell script with my **own IP** address to ensure that the reverse connection is directed to my machine.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/nanoreverseshell.png">
<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/changeportip.png">

I will proceed to upload the **PHP reverse shell file** to the **`/panel`** directory on the website.

  
I will set up a **listener** using **netcat** on the port specified in the reverse shell to establish a connection with the uploaded PHP reverse shell.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/netcatcommand.png">

Upon attempting to upload the file, an error message informs me that the **file extension is not accepted**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/erroruploadphp.png">


In order to **bypass the security restriction**, I will **rename** the file extension from **`.php`** to **`.phtml`** before reattempting the upload.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/renamephpphtml.png">

Upon retrying the upload with the **`.phtml`** extension, the file is **successfully uploaded to the server**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/successupload.png">

I click on the **"Veja!"** button to **access** the uploaded file.

I successfully **gained access** to the machine through the established connection using **netcat**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/succesreverseshell.png">

Upon executing the command **"whoami"**, I observe that the **current user** is identified as **"www-data"**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/whoamicommand.png">

Upon executing **"ls"** to list the files, the **"user.txt"** flag was not found. Further exploration is required to locate the flag.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/lscommand.png">

To locate the **"user.txt"** flag file, I will employ the **"find"** command with the following syntax: **"find / -name "user.txt" 2>&0"**. This command performs a search across the entire file system, starting from the root directory **("/")**, for any file named **"user.txt"**. The **"2>&0"** part redirects any error messages to the standard output, ensuring that any permission-related errors or warnings are displayed. By executing this command, we aim to locate the specific file containing the **"user.txt"** flag.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/findusertxt.png">

The **"user.txt"** flag file has been discovered in the **"/var/www"** directory.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/locationusertxt.png">

I display the contents of the flag file using the **"cat"** command. Here's the command you provided: **`cat /var/www/user.txt`**. 

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/flagusertxt.png">

To **stabilize the shell** and enhance interactivity, we will perform the following steps. Firstly, we execute the command **`python3 -c 'import pty;pty.spawn("/bin/bash")'`** to spawn a new shell session with improved features. Then, we press **Ctrl + Z** to temporarily suspend the shell. Next, we run **`stty raw -echo; fg`** to modify the terminal settings and bring the suspended shell session back to the foreground. Lastly, we set the **`TERM`** environment variable to **`xterm`** by executing **`export TERM=xterm`**. These steps are crucial for **stabilizing the shell** and providing a **more seamless and efficient experience**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/stabshell.png">

To **escalate privileges** and **gain root access**, we will use the **"find"** command to search for files with the **SUID permission**. Here's the command you provided: **`find / -type f -user root -perm -4000 2>/dev/null`**. This command searches the entire file system ("/") for files **owned by the "root" user** ("-user root") with the **SUID permission ("-perm -4000")**, while redirecting any **error messages** to **"/dev/null"**. By executing this command, we aim to identify files that may enable us to **escalate privileges** and **gain root access**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/findsuidperm.png">

With the discovery of the **`/usr/bin/python`** file having the **SUID permission**, we can explore **potential privilege escalation techniques**. To aid us in this process, we will refer to the website [**GTFOBins**](https://gtfobins.github.io/) which provides a collection of **methods** for **exploiting binaries** with **SUID permissions** to **escalate privileges**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/suidgtfobins.png">

We utilize the command **`python -c 'import os; os.execl("/bin/sh", "sh", "-p")'`**, which was found on **GTFOBins**, to escalate our privileges and obtain root access. This command leverages **Python** to execute a **new shell ("/bin/sh")** with the **`-p`** option, which preserves the environment variables. By executing this command, we gain a **root shell**, enabling us to perform **actions** with **elevated privileges**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/privescpython.png">

We will use the **"find"** command again to **locate** the **"root.txt" flag**. Here's the command you provided: **`find / -name "root.txt"`**. After executing this command, we have identified that the **"root.txt" flag** is located at **`/root/root.txt`**.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/findroottxt.png">
<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/locationflagroottxt.png">

We can display the contents of the **"root.txt" flag** file using the **"cat" command**. Simply execute the command **`cat /root/root.txt`** to print the contents of the file to the terminal.

<img src="https://github.com/BalikFR/TryHackMe-CTF-Writeups/blob/master/Easy/RootMe/Images/catflagroottxt.png">

**Congratulations**! You have successfully completed the **RootMe CTF challenge** and obtained both the **"user.txt"** and **"root.txt"** flags. By following the steps outlined in this write-up, you have demonstrated your skills in **penetration testing**, **privilege escalation**, and **flag retrieval**.