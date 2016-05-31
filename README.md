# Apache 2.4.x with HTTP/2

## Instruction
copy directory `custom` to /path/to/Apache2.4/conf

edit `httpd.conf` by add at bottom of file
```
Include conf/custom/*.conf
```

gen SSL self-signed certificate

```
cd /path/to/Apache2.4/bin
openssl req -config ../conf/openssl.cnf -x509 -sha256 -nodes -days 3650 -newkey rsa:2048 -keyout ../conf/custom/cert/server-sha256-2048.key -out ../conf/custom/cert/server-sha256-2048.crt
```

## Apache
check apache configure is valid
```
cd /path/to/Apache2.4/bin
httpd -t
Syntax OK
```
### install service
```
httpd -k install
```

### start service
```
httpd -k start
```

### stop service
```
httpd -k stop
```

### Test open Web Browser
URL https://localhost

## HTTP/2
conf/custom/httpd-ssl.conf

```
<IfModule !http2_module>
    LoadModule http2_module modules/mod_http2.so
</IfModule>

<VirtualHost *:443>
    ServerName example.com

    ProtocolsHonorOrder On
    Protocols h2 http/1.1

    # Other configuration stuff here
</VirtualHost>
```
## Appendix

### cURL
https://curl.haxx.se/download.html
https://bintray.com/vszakats/generic/curl/
```
D:\>curl -Ik https://localhost/
HTTP/2.0 200
date: Tue, 31 May 2016 09:38:27 GMT
server: Apache
cache-control: private, max-age=0
pragma: no-cache
expires: -1
x-frame-options: DENY
x-xss-protection: 1; mode=block
x-content-type-options: nosniff
strict-transport-security: max-age=31536000; includeSubDomains
content-type: text/html;charset=UTF-8
```
### Apache 2.4.x
http://www.apachelounge.com/download/
Be sure that you have installed the latest C++ Redistributable Visual Studio 2015 : vc_redist_x64/86.exe

### Directory Structure
```
/path/to/Apache2.4/conf
|
|-- charset.conv
|-- httpd.conf
|-- magic
|-- mime.types
|-- openssl.cnf
|   
+---custom
|   |-- httpd-default.conf
|   |-- httpd-ssl.conf
|   |-- mod_deflate.conf
|   |-- mod_php5.conf
|   |-- mod_proxy_epay.conf
|   |-- security.conf
|   |   
|   \---cert
|       |-- gen-cert.txt
|           
+---extra
|       
\---original
```
