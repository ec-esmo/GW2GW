# GW2GW
Gateway to gateway microservice


**Docker container** 

Find the related image at DockerHub: *mvjatos/gw2gw* (check for the current version)

The following versions are available:

|**Tag**|**Description**|
| ------ | ------ |
| **1.0.0**| Published at GitHub| 
| **1.0.1**| Headers updated|

(*) Remember to update your *msMetadaList.json* and the *LGW_config.json* with the related https urls.

**Environment variables**

1. ASYNC_SIGNATURE= boolean, if true denotes RSA signing for JWTs, else HS256 signing is conducted
1. KEYSTORE_PATH = path to the keystore holding the RSA certificate used for signing JWTs, encrypting JWEs, ...
1. KEY_PASS= password for the certificate
1. STORE_PASS= password for the keystore containing the certificate
1. JWT_CERT_ALIAS= alias of the certificate in the keystore (**signing the ESMOToken**. Related to the one specified the configuration filein the internal/LGW_config.json)
1. HTTPSIG_CERT_ALIAS= for the httpsig protocol
1. JWE_CERT_ALIAS= for the JWE (**encrypting the ESMOToken**. Related to the one specified in the configuration file internal/LGW_config.json)
1. SIGNING_SECRET= HS256 secret used for symmetric signing of jwts, e.g. QjG+wP1CbAH2z4PWlWIDkxP4oRlgK2vos5/jXFfeBw8=
1. RSA_CIPHERED_ESMOTOKEN= boolean, if true then the ESMOToken is encrypted, otherwise it is signed only.
1. CONFIGURATION_MANAGER_URL= location of the configuration manager
1. SESSION_MANAGER_URL= location of the session manager
1. SSL_* = information related to your ssl certificate


**docker-compose example**:

```
    GW2GW:
        image: mvjatos/gw2gw:1.0.0
        environment:
            - KEYSTORE_PATH=/resources/testKeys/keystore.jks
            - KEY_PASS=selfsignedpass
            - STORE_PASS=keystorepass
            - JWT_CERT_ALIAS=1
            - HTTPSIG_CERT_ALIAS=1
            - JWE_CERT_ALIAS=1
            - SIGNING_SECRET=QjG+wP1CbAH2z4PWlWIDkxP4oRlgK2vos5/jXFfeBw8=
            - ASYNC_SIGNATURE=true
            - RSA_CIPHERED_ESMOTOKEN=false   #(ESMOToken with or without encryption)
            - CONFIGURATION_MANAGER_URL=https://vm.esmo-project.eu:8083
            - SESSION_MANAGER_URL=http://SessionManager:8090
            - SSL_KEYSTORE_PATH=/resources/keystoreatos.jks
            - SSL_STORE_PASS=AtosCert1
            - SSL_KEY_PASS=AtosCert1
            - SSL_CERT_ALIAS=atoscert


        volumes:
            - /ESMO/GW2GW/resources:/resources
        links:
            - ConfManager:vm.esmo-project.eu
        ports:
          - 8050:8050
          - 8053:8053
        depends_on:
          - SessionManager
          - ConfManager

```
