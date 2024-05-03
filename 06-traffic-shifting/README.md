# (Service) Version control

[Reference](https://istio.io/latest/docs/tasks/traffic-management/request-routing/)

## Destination weights

We use weights to redirect 50% of the traffic to the v3 of the service.
This is done at the request level, it doesn't matter how many serving pods we have

```yaml
{{#include v1-v3-co-existance.yaml}}
```

Traditional kubernets can only achieve this by playing with the number of replicas of pods
of different e.g. ReplicaSet

### Test it

```bash
kubectl apply -f v1-v3-co-existance.yaml
```

## Complete the migration, then simulate a roll-back

Play around with weights and kubectl apply.
Use the /productpage and kiali to test if it worked