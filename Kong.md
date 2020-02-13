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
We'll create a demo Service and a corresponding Ingress.

```
kubectl apply -f https://bit.ly/echo-service

echo "
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: demo
spec:
  rules:
  - http:
      paths:
      - path: /foo
        backend:
          serviceName: echo
          servicePort: 80
" | kubectl apply -f -

curl -i $PROXY_IP/foo
```

Now we'll configure Kong to only respond to GET requests and not strip the path defined in the Ingress

```
echo "apiVersion: configuration.konghq.com/v1
kind: KongIngress
metadata:
  name: sample-customization
route:
  methods:
  - GET
  strip_path: false" | kubectl apply -f -
  
kubectl patch ingress demo -p '{"metadata":{"annotations":{"configuration.konghq.com":"sample-customization"}}}'
```

### Load Balancing
### Proxy Behaviour
### Health Checks
