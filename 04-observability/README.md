# Istio & Observability

## Download istio release source

```bash
wget https://github.com/istio/istio/archive/refs/tags/1.21.2.zip
unzip 1.21.2.zip
```

## Install the sample add-ons supplied

```bash
kubectl apply -f istio-1.21.2/samples/addons
kubectl rollout status deployment/kiali -n istio-system
```

## Acces kiali dashboard

```bash
istioctl dashboard kiali
```