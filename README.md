# README

An openldap docker-compose with a simple management ui. Useful for development
purposes only...

## Docker Setup

Just a regular installation of docker needed - all config can be changed in
docker-compose.yml. To pull and deploy the service, generate the certficates as
below and then run the following

    docker compose pull
    docker compose up -d

## Certificates

Require CFSSL utilities - https://github.com/cloudflare/cfssl

### Install

For go 1.18 and above run (otherwise read the above)

    go install github.com/cloudflare/cfssl/cmd/...@latest

### Gen CA

    cd certs
    cfssl gencert -initca ca.json | cfssljson -bare ca

### Gen Intermediate CA

    cfssl gencert -initca intermediate-ca.json | cfssljson -bare intermediate_ca

### Sign Intermediate CA

    cfssl sign -ca ca.pem -ca-key ca-key.pem -config cfssl.json -profile intermediate_ca intermediate_ca.csr | cfssljson -bare intermediate_ca

### Generate host cert and sign it

    cfssl gencert -ca intermediate_ca.pem -ca-key intermediate_ca-key.pem -config cfssl.json -profile=server openldap-host.json | cfssljson -bare openldap-host-server

### Copy files into folder

    cat ca.pem intermediate_ca.pem > ca.crt
    cp openldap-generic-host.pem server.crt
    cp openldap-generic-host-key.pem server.key
    

