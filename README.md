# Build
This builds on all RHEL7 / CentOS 7 releases.
Make sure the linux kernel-headers package is installed.
   
```
 $ make all
```
The build will produce:
-    ipfw_mod.ko: the  kernel module
-    ipfw: the  userland program


#  Operation 

## Load the kernel module
```
# insmod ./kipfw-mod/ipfw_mod.ko
```

## Add some ipfw rules
```
# ipfw -q flush
# ipfw -q pipe flush
# ipfw add pipe 3 in dst-port 80,443,8080
# ipfw pipe 3 config delay 30ms bw 80000Kbit/s plr 0.00 queue 1000Kbytes jitter 50ms queue-delay 100ms queue-increments 50
```
This would setup a pipe with 
* 30ms delay
* 80Mbps bandwidth
* 0% packet loss
* 1MB Queue
* 50ms jitter on the connection
* Queue delay of 100ms - so as 1MB Queue fills this will be an additional delay added
* Queue increments of 50 so 100ms/50 means 2ms of delay added for each equivalent portion of the 1MB queue filled

## Additional options to use in ipfw rules i.e. jitter/queue-delay/queue-increments
```
# ipfw -q flush
# ipfw -q pipe flush
# ipfw add pipe 3 in dst-port 80,443,8080
# ipfw pipe 3 config delay 80ms bw 40Mbit/s plr 0.01 queue 1000Kbytes
```

## Unload the kernel module
```
# rmmod ipfw_mod.ko
```


# Authors

This directory contains a port of ipfw and dummynet to Linux and Windows.
This version of ipfw and dummynet is called "ipfw3" as it is the
third major rewrite of the code.  The source code here comes straight
from FreeBSD (roughly the version in HEAD as of February 2010),
plus some glue code and headers written from scratch.  Unless
specified otherwise, all the code here is under a BSD license.


## CREDITS:

- Luigi Rizzo (main design and development)
- Marta Carbone (Linux and Planetlab ports)
- Riccardo Panicucci (modular scheduler support)
- Francesco Magno (Windows port)
- Fabio Checconi (the QFQ scheduler)
- Funding from Universita` di Pisa (NETOS project),
- European Commission (ONELAB2 project)
- ACM SIGCOMM (Sigcomm Community Projects Award, April 2012)


