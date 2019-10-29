---
layout: post
title:  "Working with AT Commands on Yeastar TG100"
author: Septima Vector
date:   2019-10-28 15:16:24 +0100
excerpt_separator: <!--more-->
---

Yeastar TG100 is a price affordable UMTS-SIP Gateway based on Asterisk.  By accessing the modem via AT commands, powerful solutions can be developed.
<!--more-->

The default device IP is 192.168.5.150, reachable via web UI.

From the web UI, is possible to enable SSH service. It is on LAN Settings page or Service menu under LAN settings. Default SSH port is 8022.

The default login credentials for SSH is different from web UI ones.

Default username: root 

Default password: ys123456

Once logged in, you can execute the Linux based commands, like ls, cd, route and so on.

You can also enter the Asterisk CLI by:
```
root@TG100:~# asterisk -vvvvvvvvvr
```

Execute gsm show spans to find your target GSM port; on TG100 there is a single port
```
TG100*CLI> gsm show spans
GSM span 2: Power on, Provisioned, Up, Active,Standard
```

Then execute gsm show span X (X is the span number of your target GSM port).

The span 2 means GSM port 1.

Usually the span number equals the GSM port number plus one. 

That is Span Number = GSM port Number + 1.

In this way you're able for example to check the signal level.

```
TG100*CLI> gsm show span 2
D-channel: 2
Status: Power on, Provisioned, Up, Active,Standard
Type: CPE
Manufacturer: 
Model Name: UC15
Model IMEI: 868998034998822
Model CBAND: 
Revision: 3A12E1G
Network Name: **OMISSIS**
Network Status: Registered (Home network)
Signal Quality (0,31): 5
SIM IMSI: **OMISSIS**
SIM SMS Center Number: **OMISSIS**
Send SMS Center Number: Undefined
Last event: Unknown Event
State: READY
Last send AT: AT+CREG?\r\n\n
Last receive AT: \r\nOK\r\n
```
Under manufacturer and Model name, you can see the specific modem used in your TG100. The manufacturer is omitted here, I think by Yeastar choice. I infer that it is probably a QUECTEL UC15.

To send AT commands to the UMTS modem, you need to go into debug mode to see the modem answer.
```
TG100*CLI>gsm debug span 2
```

The general format to send AT commands is: gsm send at 2 AT+XXX. 
2 is the span nubmer, and XXX means the corresponding AT commands.
Let's verify what modem it is.
```
TG100*CLI> gsm send at 2 ATI
2:>> ATII> 
2:<< \r\nQuectel\r\n
		2:<<< 0 ATI -- Quectel , 7
2:<< UC15\r\n
		2:<<< 0 ATI -- UC15 , 4
2:<< Revision: UC15EQBR03A12E1G\r\n
		2:<<< 0 ATI -- Revision: UC15EQBR03A12E1G , 26
2:<< \r\nOK\r\n
		2:<<< 0 ATI -- OK , 2
```
And yes, it is a QUECTEL UC15.
The complete list of AT Commands of this modem is available in this [PDF Manual](assets/files/Quectel_UC15_AT_Commands_Manual_V1.1.pdf) directly.

To get for example the current IMEI
```
TG100*CLI> gsm send at 2 AT+CGSN
2:>> AT+CGSN
2:<< \r\n868998034998822\r\n
		2:<<< 0 AT+CGSN -- 868998034998822 , 15
2:<< \r\nOK\r\n
		2:<<< 0 AT+CGSN -- OK , 2

```

Done the job, you can disable the debug for that specific port by input command like: 
```
TG100*CLI>gsm no debug span 2
```




