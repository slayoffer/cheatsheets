### Check ENVs in pod

```bash
kubectl exec -n isup-work back-648c4bf96b-ww6wp -- env

# or
kubectl describe po -n smarteka-dev chat-7786649896-5jhwn
```