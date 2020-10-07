# Creating Secure Docker Registry Container 
## Set or Change Hostname in CentOS/RHEL 7/8
```bash
# hostnamectl set-hostname your-new-hostname
```
## Creating SSL certificate
```bash
# mkdir -p /docker-registry/certs
# openssl req -newkey rsa:4096 -nodes -sha256 -keyout /docker-registry/certs/domain.key -x509 -days 356 -out /docker-registry/certs/domain.crt

Generating a RSA private key

Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []: your-new-hostname
Email Address []:
```

## Creating docker-compose file
```bash
# vim docker-compose.yml
```

```yml
version: '3'

services:
  registry:
    container_name: secure_registry
    image: registry
    ports:
    - "5000:5000"
    volumes:
    - "${PWD}/certs/:/certs"
    environment:
    - REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt
    - REGISTRY_HTTP_TLS_KEY=/certs/domain.key
 
    
