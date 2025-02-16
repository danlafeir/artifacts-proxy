# A better protocol-bound world 

This repo is meant to be an example of what I want from Platform Engineering groups. Migrations are unavoidable so take advantage of protocols. Below is a rough example of how you could accomplish this with npm using Keyclock and Envoy.


## Start up the envoy proxy

```bash
brew install envoy
envoy -c envoy-npm-proxy.yaml
```

## Run Keyclock

```bash
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:24.0.2 start-dev
```
