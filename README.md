# Creating Secure Docker Registry Container 
## Set or Change Hostname in CentOS/RHEL 7/8
```bash
# hostnamectl set-hostname your-new-hostname
```
## Creating SSL certificate
```bash
# mkdir -p /certs
# openssl req -newkey rsa:4096 -nodes -sha256 -keyout /certs/domain.key -x509 -days 356 -out /certs/domain.crt

Generating a RSA private key

Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []: your-new-hostname
Email Address []:
```
