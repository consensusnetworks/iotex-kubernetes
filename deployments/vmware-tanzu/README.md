# IoTeX Node in Kubernetes – VMware Tanzu

## Prerequisites

- Todo, follow the instructions in the Tanzu [docs](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.4/vmware-tanzu-kubernetes-grid-14/GUID-tanzu-k8s-clusters-index.html) for now

## Creating GKE Kubernetes Cluster

- Todo, follow the instructions in the Tanzu [docs](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.4/vmware-tanzu-kubernetes-grid-14/GUID-tanzu-k8s-clusters-index.html) for now

## Configure your custom [values](../../iotex/values.yaml)

`externalLBp2pIP` and `internalLBIP`: Update with the EXTERNAL-IP INTERNAL-IP returned from `kubectl get nodes --output wide`