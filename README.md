# #CTFFriday 2020MayWeek5 - Web/Crypto CTF

* Date: 29nd May 2020
* Time: 2:00 to 3:00 PM

## Organized By

| ![](media/rsz_1rsz_netsquare.png)    |![](media/rsz_conclave.png)|
|:-----------------------------------------------:|:---------------------------------------------:|
| Net Square Solutions Pvt. Ltd.                              | NSConclave                             |

## About Me

Hi,

I am [Vitthal Shinde](https://twitter.com/0_1VitthalS) goes on CTF by name `0_1` and I am currently working in [Net Square Solutions Pvt. Ltd.](https://net-square.com) as a Security Analyst.


## Overview

There were 2 Crypto challenges and 2 Web challenges, In web we have to exploit multiple vulnerabilities such as SSRF, SSJI, Desearilization, etc to get the flag.

![alt text](https://i.ibb.co/WK6f1Rt/challenges.png "Challenges")

------------------------------------------------------------------

## Tools Used
- BurpSuite
 
-------------------------------------------------------------------------
|Challenge 1|
|------------|
|2020MayW5: FLAG 1|
|Can you Decrypt this? |
| 50 pts|


**Description**

- This challenge was based on crypto and there was a `key.zip` file given and one hint as follows

    ![alt text](https://i.ibb.co/9Y7nfvj/hint.png "netsquare") 

- After extracting the `key.zip` file I got the key.txt file which was in encrypted form.
        
    ![alt text](https://i.ibb.co/BVXGr5b/key.png "key") 

- To decrypt this i have searched for fernet decrypt on google and stumbled upon below site

    ![alt text](https://i.ibb.co/LRscQP8/site.png "fernet_site") 
    
After replacing with given data 

```
    key : 0nAadkf8m9sjPvkmT5lfYbkzsqXZKFcs6NYhQkl-gf8=
    Token : gAAAAABeul1DK_oLEMNOgjTYQIgY0CBwV-0__lTuk9noqBIa58OZGOGKEDPhIXY-g7I5DKi_mcSYLtSLLoQ_XqfMxx7eeL1v8Nz00Pag00I-7e_dwKd8t2I= 
```


   ![alt text](https://i.ibb.co/54LrRJY/flag1.png "flag1")

I got my first flag `nsctf{Y3h_b@burao_ka_style_hai}`.

-------------------------------------------------------------------
|Challenge 2|
|------------|
|2020MayW5: Flag 2|
|You get the crime scene of a bank and you have caught one person. Under further analysis of the persons flip phone you see a message that seems suspicious. Can you help me to figure out what the message?
(75 pts)|

#### Description

![alt text](https://i.ibb.co/fGvrQ5Q/challenge2.png "challenge2")

We have given one `suspicious.zip` file and one message which basically says to figure out what the message is?
So I have download the `suspicious.zip` file and extracted which contains `suspicious.txt` file having all the numbers.

![alt text](https://i.ibb.co/G5FJDBX/suspicious.png "suspicious")


![alt text](https://i.ibb.co/Kh3hJdj/babu.jpg "baburao")

So I have tried multiple decoders/decrypters online and got to know that it is **Multi-Tap Phone Cipher**, So After Decrypting it i got the following message

![alt text](https://i.ibb.co/PcSJshZ/multitap.png "multitap")

So Now I have decrypt again given message
`MHXGUBLFZIVTIVZGVIGSZMBLFIZWWRXGRLM`. Again after multiple trial and errors i got to know that it's a **Mirror Cipher**.

After decrypting it I got the 2nd Flag.

![alt text](https://i.ibb.co/zHRcz8y/flag2.png "flag2")

--------------------------------------------------------

|Challenge 3|
|------------|
|2020MayW5: Flag 3|
| http://10.90.137.137:7676
(200 pts)|

#### Description

The challenge was based on Web and we have given the URL `http://10.90.137.137:7676`, After visiting the URL we were presented with one simple website.

![alt text](https://i.ibb.co/KFM0RK0/bob.png "bob")

After submitting on random value, website was showing an error like `Try with proper address`, As the challenge suggests to try with proper address, i fired up my burp and Intercepted the request and got to know that there is one parameter `url`, So I have checked if i will get hit in my collaborator, but no success and the same error was thrown.

![alt text](https://i.ibb.co/dft6Nd2/collab.png "collab")

Then I have tried with `http://127.0.0.1:80` and woahh we got success and we got the message `Going great. Try further to find Bob....`.

So Now we know that application is vulnerable to SSRF, lets do ***Port Scanning***.

After fuzzing with common ports using Intruder, I got that **Port 3000** was open and we have given a message `Awesome!!! Command Bob to go to your place. Care to give cmd??` and one hint to look the wappalyzer.

After analyzing the header `X-Powered-By` header i got know that application is built in js technology.

So I have tried for some basic commands like whoami,id,etc. but no success and everytime i got one error like `ls is not defined`.

![alt text](https://i.ibb.co/ZBhjYGD/ls.png "ls")

So I got that Applcation is vulnerable to SSJI and after trying detection payload i got the confirmation.

Then I have tried to list the directories and files in current directory, and I got 2 folders.

![alt text](https://i.ibb.co/fnWYXH8/files.png "files")

After Viewing the content of `Secret` folder, there was our ***flag.txt***

![alt text](https://i.ibb.co/VBSGPh2/read.png "files")

Let's read the flag.

![alt text](https://i.ibb.co/MDDfNsm/flag3.png "flag3")




--------------------------------------------------------

|Challenge 4|
|------------|
|2020MayW5: Flag 4|
|http://10.90.137.137:4747 
(200 pts)|

#### Description

We have given a URL `http://10.90.137.137:4747`, After visiting the URL we have greeted with simple website.

![alt text](https://i.ibb.co/X78zrdz/web.png "web")

And it was redirecting us to `http://10.90.137.137:4747/scotch` and there was a message `Cookie Set: Bobmarley`

![alt text](https://i.ibb.co/QbwHKc3/cookie.png "cookie")

There was one cookie `username` set. 

![alt text](https://i.ibb.co/jV0FXZs/scotch.png "scotch")


I have decoded it with base64.



![alt text](https://i.ibb.co/kGmr3yc/base64.png "base64")

After googling a bit I got that application is vulnerable to Deserialization vulnerability.

Let's forge our cookie and get the reverse connection.

![alt text](https://i.ibb.co/XjPcCG7/payload.png "payload")

Change the cookie with our forged one and send the request.

![alt text](https://i.ibb.co/Sn7dxVr/req.png "req")

Start the netcat listner.

Lets Read the flag.

![alt text](https://i.ibb.co/wCSdWgS/flag4.png "flag4")

And I got the final flag.

Here are the stats from the CTF.

![alt text](https://i.ibb.co/bFZyJKw/score.png "scoreboard") 


It was a great CTF by [kalyan](https://twitter.com/3kr426) having different challenges such as SSRF, SSJI, Desearilization,etc.
I had fun solving them.



--------------------------------------------------------


