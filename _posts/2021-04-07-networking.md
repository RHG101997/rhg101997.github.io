---
title: Networking
author: Richard Hernandez
date: 2021-04-07 11:32:00 +0800 
categories: [Tools, Networking]
tags: [netstat, ping, tools, networking, tcp, udp, protocols]
---

## Netstat

All the following commands can be combiening to create more complete outputs

```shell
netstat             #List using DNS(slow)
netstat -n          #List not using DNS(fast)
netstat -a          # Shows active connections also if port is listening 
netstat -b          # (Need sudo) Show program using connection
netstat -f          # Displays Fully Qualified Domain Names of the address 
netstat -?     #Help
```


## Ipconfig(Windows)

```shell
ipconfig            #Show most important info
ipconfig /all       #Shows all information, and configuration
ipconfig /displaydns    #Shows your DNS cache
ipconfig /flushdns  #Flushes your DNS cache
ipconfig /?        #Help
```

## Ping

```shell
ping <IP>           #Sends 4 packets and wait for the response to measure time
ping 127.0.0.1      #Check for network card(loop back test)
```

## Traceroute

This command in name different for linux: `traceroute`

```shell
tracert <IP>           #Shows round trip time between all hops that data package take
tracert -h <hops> <IP> # Custom amount of hops(TTL)
```

