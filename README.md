# docker-nginx-proxy
Boilerplate for Nginx-proxy server for a Docker container

Reference: https://blog.ssdnodes.com/blog/host-multiple-websites-docker-nginx/ 

### Prerequisites:

Install Docker Desktop for Mac - https://docs.docker.com/docker-for-mac/install/ 
Install Brew Open SSL (for self signed ssl certs)
 $ brew install openssl

## Set up Nginx-proxy

### 1. Clone the repository into an empty folder nginx-proxy/
git clone git@github.com:pango-studio/docker-nginx-proxy.git .

### 2. Create the required folders
mkdir certs

### 3. Run docker-compose
$ docker-compose up -d

.. Your network is up!

## Set up self signed certs
In the nginx-proxy folder add a /certs folder where the self-signed certs can be housed

### Create certs
Create the relevant certs for EACH local dev site via terminal in the /certs folder. Use the command below to do this remembering to change the domains to your local dev domain:

openssl req \
    -newkey rsa:2048 \
    -x509 \
    -nodes \
    -keyout example.local.key \
    -new \
    -out example.local.crt \
    -subj /CN=example.local \
    -reqexts SAN \
    -extensions SAN \
    -config <(cat /System/Library/OpenSSL/openssl.cnf \
        <(printf '[SAN]\nsubjectAltName=DNS:example.local')) \
    -sha256 \
    -days 3650

.. Your certs are set up!

### Allow the local domains to map to your local IP address

You need to edit your /etc/hosts file to add the local mappings of custom dev urls
In terminal go to the etc directory
$ cd /private/etc
$ sudo vim hosts

Add your new local domains like lms.local & example.local below
127.0.0.1   localhost
127.0.0.1   example.local
127.0.0.1   lms.local

.. your local domains are mapped

