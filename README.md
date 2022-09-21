# Keycloak BCrypt

This fork aims to add a custom password hash provider to handle BCrypt passwords inside Keycloak, which is used in
project [OpenERP](https://github.com/dungkhmt/openerp).

## Build JAR

Change version of Keycloak if needed and run:

```bash
./gradlew assemble
```

## Build Docker image

```bash
cp build/libs/keycloak-bcrypt-${KEYCLOAK_BCRYPT_VERSION}.jar docker
```

```bash
docker build \
    --build-arg keycloak_version=${KEYCLOAK_VERSION} \
    --build-arg keycloak_bcrypt_version=${KEYCLOAK_BCRYPT_VERSION} \
    -t gleroy/keycloak-bcrypt \
    docker
```

## Test with docker-compose

```bash
docker-compose up -d
```

## How to use (released from [original repo](https://github.com/leroyguillaume/keycloak-bcrypt))

Depending on the specific context, you can choose one of 2 ways:

### 1. Install

With Keycloak version:

### \>= 17.0.0

```bash
curl -L https://github.com/leroyguillaume/keycloak-bcrypt/releases/download/${KEYCLOAK_BCRYPT_VERSION}/keycloak-bcrypt-${KEYCLOAK_BCRYPT_VERSION}.jar > ${KEYCLOAK_HOME}/providers/keycloak-bcrypt-${KEYCLOAK_BCRYPT_VERSION}.jar
```

You need to restart Keycloak.

### < 17.0.0

```bash
curl -L https://github.com/leroyguillaume/keycloak-bcrypt/releases/download/${KEYCLOAK_BCRYPT_VERSION}/keycloak-bcrypt-${KEYCLOAK_BCRYPT_VERSION}.jar > ${KEYCLOAK_HOME}/standalone/deployments/keycloak-bcrypt-${KEYCLOAK_BCRYPT_VERSION}.jar
```
You need to restart Keycloak.

### 2. Run with Docker

```bash
docker run \
    -e KEYCLOAK_ADMIN=${KEYCLOAK_ADMIN} \
    -e KEYCLOAK_ADMIN_PASSWORD=${KEYCLOAK_ADMIN_PASSWORD} \
    -e KC_HOSTNAME=${KC_HOSTNAME} \
    gleroy/keycloak-bcrypt \
    start
```

The image is based on [Keycloak official](https://quay.io/repository/keycloak/keycloak) one.

Go to `Authentication` / `Password policy` and add hashing algorithm policy with value `bcrypt`. To test if installation
works, create new user and set its credentials.
