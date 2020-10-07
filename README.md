# Creating Secure Docker Registry Container with Self-Signed Certificate
## Set or Change Hostname in CentOS/RHEL 7/8
```bash
# hostnamectl set-hostname your-new-hostname
example
# hostname set-hostname repo.docker.local
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
Common Name (eg, your name or your server's hostname) []: repo.docker.local  ****your prefered hostname****
Email Address []:
-------
# mkdir -p /etc/docker/certs.d/<your-new-hostname:port>
example
# mkdir -p /etc/dockercerts.d/repo.docker.local:5000

# cp /docker-registry/certs/domain.crt /etc/dockercerts.d/repo.docker.local:5000/ca/crt

# vim /etc/hosts
   
   <your ip>  repo.docker.local
   
# systemctl restart docker
```
## Creating docker-compose file
```bash
# mkdir /docker-registry/images
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
    - "${PWD}/images/:/var/lib/registry"
    environment:
    - REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt
    - REGISTRY_HTTP_TLS_KEY=/certs/domain.key
 
 ```bash
 # docker-compose up -d
 ```
 # Test Docker Registry
 
 ```bash
 # docker create alpine
 # docekr image tag alpine repo.docker.local:5000/alpine
 # docker push repo.docker.local:5000/alpine
    
