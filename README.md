# A better protocol-bound world 

This repo is meant to be an example of what I want from Platform Engineering groups. Migrations are unavoidable so take advantage of protocols. Below is a rough example of how you could accomplish this with npm and maven using Keyclock and Envoy.


## Setup 
*I am doing this on a Mac and not worrying about other OS compatibility*

```bash
brew install envoy
brew install --cask rancher
```

## "Unauthenticated" proxy to the default NPM registry

### run the proxy

```bash
envoy -c envoy-npm-proxy.yaml
```

### installing something with npm

```bash
npm login #this will set your `_authToken` for https://registry.npmjs.org
npm config set registry http://0.0.0.0:1337
```

Adjust your `~/.npmrc` file to look like

``` 
registry=http://0.0.0.0:1337
//0.0.0.0:1337/:_authToken=... 
```

Now you can run `npm install backbone` and see your request proxied through Envoy


## "Unauthenticated" proxy to maven central

*In the real implementation, you should remove `isAllowInsecureProtocol`. I can't believe this is still an available option TBH but I might not prioritize adding a cert to Envoy for this example.*

### run the proxy

```bash
envoy -c envoy-maven-proxy-with-auth.yaml
```

### run Keyclock

```bash
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:24.0.2 start-dev
```

### build the project (maven repo is configured at the proxy)

```bash
cd ktor-sample-project
./gradlew build
```

