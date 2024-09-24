## syntax 
scp src dest

## local to remote
scp /path/to/file.txt student@10.50.xx.xx:/path/to/destination/file.txt .

## remote to local
ssh -MS /tmp/jump student@10.50.xx.xx

ssh -S /tmp/jumpdummy -O forward -L111:192.168.28.xx:2222

scp -P creds2127.0.0.1:/path/to/src/file.txt .

## local to remote va tunnel 
scp -P 1111 /path /to/src/file.txt ced@127.0.0.1:/path/to/destination/file.txt
