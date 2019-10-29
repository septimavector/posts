---
layout: default
---

## Welcome to another page
**Disclaimer: Changing IMEI is illegal in some jurisdiction.**

To change the IMEI on 
* QUECTEL modem 
```
TG100*CLI>gsm send at 2 AT+EGMR=1,7,"868998034998811"
```

* SIMCOM modem
```
TG100*CLI>gsm send at 2 AT+SIMEI=460005011208349
```

* WAVECOM modem
```
TG100*CLI>gsm send at 2 AT+WIMEI="460005011208349"
```

For all modems you should then reset the modem module 
```
TG100*CLI>gsm send at 2 AT+CFUN=1,1
```

_yay_

[back](./)
