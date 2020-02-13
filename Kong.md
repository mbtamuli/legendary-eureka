# Kong: Microservices API Gateway
---

## Introduction

Kong is a Lua application running in Nginx and made possible using the lua-nginx-module. What this means is, Kong leverages the power of using Lua scripts with Nginx to have a pluggable architecture. This pluggable architecture allows plugin code to live in separate code bases and still be injected anywhere into each request’s lifecycle.

## Kong
There are multiple ways for installing Kong. We are going to use minikube for local testing and use helm for installing Kong

```bash
minikube start

helm repo update
helm install demo stable/kong

export PROXY_IP=$(minikube service demo-kong-proxy --url | head -1); echo $PROXY_IP
```

### Ingress Routing


### Load Balancing
### Proxy Behaviour
### Health Checks
