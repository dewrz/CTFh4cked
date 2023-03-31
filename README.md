# CTF: h4cked

This is a red and blue team CTF from THM where you have to utilize a .pcap file to figure out how the attacker comprimised your system and hack back into the machin. There will be a handful of questions that we will need to answer and finally elevating to root and capturing the flag.
<br>
<br>
The first question that is posed is to figure out what service the attacker is trying to log into? It looks like a user by the name of "jenny" is trying to brute force FTP.
<br>
<br>
<a href="https://imgur.com/SGWjLhd"><img src="https://i.imgur.com/SGWjLhd.jpg" title="source: imgur.com" /></a>
<br>
<br>
The next question is an easy one, "There is a very popular tool by Van Hauser which can be used to brute force a series of services. What is the name of this tool? ". The answer is, Hydra.
<br>
<br>
Next they would like to know the username the attacker is using to login--this was answered a earlier--which is, Jenny
<br>
<br>
Now they would like to know the users password. If the brute force attempt was successful, we can find this by following the TCP stream in the pcap file.
<br>
<br>
<a href="https://imgur.com/rYbK9nb"><img src="https://i.imgur.com/rYbK9nb.jpg" title="source: imgur.com" /></a>
<br>
<br>
After the attacker was authenticate, what was the current working directory? This question was also present when we investigated the TCP stream.
<br>
<br>
<a href="https://imgur.com/OVLIE5N"><img src="https://i.imgur.com/OVLIE5N.jpg" title="source: imgur.com" /></a>
<br>
<br>
The attacker uploaded a backdoor, what is the file name? Earlier when I was looking for the password that was used to authenticate, I noticed a php file. So I used the filter, "frame contains "php"" which revealed the name of the shell.
<br>
<br>
<a href="https://imgur.com/nmJhoQL"><img src="https://i.imgur.com/nmJhoQL.jpg" title="source: imgur.com" /></a>
<br>
<br>
Next they want to know what the URL is that the attacker used to upload the shell from? Here we searched the ftp-data stream and you were able to view the shell and where it originated from.
<br>
<br>
<a href="https://imgur.com/nKafi90"><img src="https://i.imgur.com/nKafi90.jpg" title="source: imgur.com" /></a>
<br>
<br>
Which command did the attacker manually execute after getting a reverse shell? Again we followed the TCP stream to discover the command, "whoami".
<br>
<br>
<a href="https://imgur.com/jnG0PIC"><img src="https://i.imgur.com/jnG0PIC.jpg" title="source: imgur.com" /></a>
<br>
<br>
What is the computer's host name? This was also part of stream 20, as was the previous question.
<br>
<br>
<a href="https://imgur.com/Mlwyrye"><img src="https://i.imgur.com/Mlwyrye.jpg" title="source: imgur.com" /></a>
<br>
<br>
Which command did the attacker execute to spawn a new TTY shell? 
<br>
<br>
python3 -c 'import pty; pty.spawn("/bin/bash")'
<br>
<br>
Which command was executed to gain root shell? Sudo su
<br>
<br>
The attacker downloaded something from GitHub. What is the name of the GitHub project? The attacker cloned, Reptile. 
<br>
<br>
<a href="https://imgur.com/qLAhvno"><img src="https://i.imgur.com/qLAhvno.jpg" title="source: imgur.com" /></a>
<br>
<br>
The project can be used to install a stealthy backdoor on the system. It can be very hard to detect. What is this type of backdoor called? Rootkit
<br>
<br>
Ok, now its time to hack back in and take back what's ours!
<br>
<br>
I tried jenny:password123 first, but they changed the password. So I ran a hydra brute force and it quickly revealed the credentials.
<br>
<br>
<a href="https://imgur.com/5eoEUqs"><img src="https://i.imgur.com/5eoEUqs.jpg" title="source: imgur.com" /></a>
<br>
<br>
I then logged into FTP, uploaded a php shell and set a listener so we can gain access. I also upgraded the shell and switched user to jenny.
<br>
<br>
<a href="https://imgur.com/cd8UBKY"><img src="https://i.imgur.com/cd8UBKY.jpg" title="source: imgur.com" /></a>
<br>
<br>
<a href="https://imgur.com/OGvDILd"><img src="https://i.imgur.com/OGvDILd.jpg" title="source: imgur.com" /></a>
<br>
<br>
After switching to "jenny" I ran sudo -l and she had sudo for "All" actions. So i switched to root user and captured the flag.
<br>
<br>
<a href="https://imgur.com/kGjnX7D"><img src="https://i.imgur.com/kGjnX7D.jpg" title="source: imgur.com" /></a>
<br>
<br>
