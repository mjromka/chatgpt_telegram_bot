## Deploy to K8s (Open Telekom Cloud)

### Manual steps:

- Create Elastic Volume Service (EVS) [separate resource outside the cluster]
> **Note:** EVS should be in AZ where we have at least one node
- Create Persistent Volume (PV) **pv-evs-mongo** in K8s/Storages and bound to the EVS
- Place proper kubectl config in `~/.kube/config` (or use `KUBECONFIG` env var)

### Automated Deployment

_Choose own namespace name before execution_

```
namespace=copilot
sed "s/{{NAMESPACE}}/${namespace}/" namespace.yml | kubectl apply -f -
for file in *.yaml; do
    sed "s/{{NAMESPACE}}/${namespace}/" "$file" | kubectl apply -f -
done
```

### Networking

1. Create Elastic IP (EIP)
2. Create NAT Gateway with SNAT Rule associated with the EIP
3. Attach the NAT Gateway to K8s VPC
