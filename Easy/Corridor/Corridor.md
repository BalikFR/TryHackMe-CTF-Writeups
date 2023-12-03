## Corridor

<img align="right" src="https://i.imgur.com/g3anER1.png" width="150" height="150">

Can you escape the Corridor?

CTF Link : [**Corridor**](https://tryhackme.com/room/corridor)

# Steps

### Step 1: Initial Reconnaissance

Scan the ports using **nmap**.

Execute the following command:

* ```# nmap <target IP>```

<img src="https://i.imgur.com/DK4adbJ.png">

### Step 2: Web Exploration

Discover a web server on port 80 with an image featuring several doors, each leading to an empty room. Observe that each door's URL is unique.

<img src="https://i.imgur.com/CXSQz0j.png" width="500" height="250">

![Image 2](https://i.imgur.com/nspSbSu.png)

![Image 3](https://i.imgur.com/PnmAN6h.png)

![Image 4](https://i.imgur.com/bUjNi8I.png)

### Step 3: Hexadecimal Values Identification

Inspect the URLs and recognize hexadecimal values resembling hashes. To identify the encryption method, use an online tool like **dCode**: **https://www.dcode.fr/identification-chiffrement**


### Step 4: MD5 Decryption

Recognize that the hashes are potentially MD5 encrypted.

![Image 5](https://i.imgur.com/dMjMLTM.png)

Decrypt the values to obtain numerical results (1, 2...).
Utilize an MD5 decryption tool for this task.


![Image 6](https://i.imgur.com/cctU5FG.png)

![Image 7](https://i.imgur.com/H9gHK8T.png)


### Step 5: Flag Retrieval

Start with the value 0 and convert it to MD5: cfcd208495d565ef66e7dff9f98764da.

![Image 8](https://i.imgur.com/8bcqn6W.png)

Append this decrypted value to the URL, leading to the flag page.

![Image 9](https://i.imgur.com/jhfAaCO.png)

![Image 10](https://i.postimg.cc/3JCFJr73/Capture-d-cran-2023-12-03-153155.png)

### Conclusion

Congratulations! You successfully navigated through the Cryptic Corridor Challenge, employing reconnaissance, decoding, and MD5 decryption techniques to retrieve the hidden flag.