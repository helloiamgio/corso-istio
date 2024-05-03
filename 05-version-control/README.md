# Traffic shifting

[Reference](https://istio.io/latest/docs/tasks/traffic-management/traffic-shifting/)

## Objective

Send 50% of traffic to reviews:v1 and 50% to reviews:v3. 

Whenever we direct the traffic to the v1 subset of the productpage service,
pick pods with `version=v1` selector

```yaml
{{#include destination-rule.yaml:1:10}}
```

## Virtual Service

DNS hijack all requests to the productpage page and use the v1 subset of the
service we just defined

```yaml
{{#include v1-virtual-service.yaml:1:12}}
```


### Install it

```bash
kubectl apply -f destination-rule.yaml
kubectl apply -f v1-virtual-service.yaml
```

### Verify it

All reviews now have no stars. Only v1 is serving our requests

## Now let's do something a bit more complex

Jason must be able to reach v2 of our service

```yaml
{{#include v2-header-matching.yaml}}
```

### Install it

```bash
kubectl apply -f v2-header-matching.yaml
```

## Caveat emperor

Note that Kubernetes services must adhere to certain restrictions to take advantage of Istio’s L7 routing features.
[See the reference guide](https://istio.io/latest/docs/ops/deployment/requirements/)

- Pod labels: 
    - Application name: service.istio.io/canonical-name, app.kubernetes.io/name, or app.
    - Application version: service.istio.io/canonical-revision, app.kubernetes.io/version, or version.
- Named service ports: Service ports may optionally be named to explicitly specify a protocol.
- If a pod belongs to multiple Kubernetes services, the services cannot use the same port number for different protocols, for instance HTTP and TCP.

