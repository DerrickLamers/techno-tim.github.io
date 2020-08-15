# Self-Hosting Your Homelab Services with SSL -- Let's Encrypt, MetalLB, Traefik, Rancher, Kubernetes

Are you self-hosting lots of services at home in your homelab?  Have you been port forwarding or using VPN to access your self-hosted services wishing you had certificates so that you can access them securely over SSL?  Well after this video, you can!  In this step by step tutorial we'll walk through setting up Rancher and Kubernetes with a reverse proxy, Kubernetes Ingress, MetalLB, Traefik, Let's Encrypt, and DNS giving you free certificates.   

https://www.youtube.com/watch?v=pAM2GBCDGTo



---


## Install WSL on Windows 10

https://www.youtube.com/watch?v=kL8iGErULiw


## Install `kubectl`

https://kubernetes.io/docs/tasks/tools/install-kubectl/


## Install MetalLB

https://metallb.universe.tf/installation/

`kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.3/manifests/namespace.yaml`

`kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.3/manifests/metallb.yaml`

`kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"`


sample `config.yaml`

```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.1.240-192.168.1.250
```

## Traefik

traefix sample answers yarl



```
---
  defaultImage: true
  imageTag: "1.7.14"
  serviceType: "LoadBalancer"
  debug: 
    enabled: false
  rbac: 
    enabled: true
  ssl: 
    enabled: true
    enforced: true
    permanentRedirect: false
  acme: 
    enabled: true
    email: "you@example.com"
    onHostRule: true
    staging: false
    logging: true
    challengeType: "dns-01"
    dnsProvider:
      name: "cloudflare"
      existingSecretName: "cloudflare-dns"
  persistence: 
    enabled: true
  dashboard: 
    enabled: true
    domain: "traefik.example.com"
    auth: 
      basic: ""
```


## Traefik Helm

https://hub.helm.sh/charts/stable/traefik



## Traefic DNS Providers


https://docs.traefik.io/https/acme/#providers