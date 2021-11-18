# IoTeX Node in Kubernetes – GCP GKE

## Prerequisites

- GCP account and new project 
- Authenticated [gcloud](https://cloud.google.com/sdk/install) CLI

## Creating GKE Kubernetes Cluster

Replace the values for MASTER_ZONE and PROJECT_NAME with your project's values.

```bash
NODE_COUNT=1
CLUSTER_INDEX=0
CLUSTER_NAME=iotex-node-${CLUSTER_INDEX} && echo "Cluster name is ${CLUSTER_NAME}"
MASTER_ZONE=us-central1-a
PROJECT_NAME=strong-compiler-327520

gcloud container clusters create $CLUSTER_NAME \
    --num-nodes $NODE_COUNT \
    --enable-autoscaling --max-nodes=1 --min-nodes=1 \
    --machine-type=n1-standard-1 \
    --cluster-version latest \
    --enable-autorepair \
    --enable-ip-alias \
    --zone=$MASTER_ZONE \
    --project=$PROJECT_NAME

gcloud container clusters get-credentials $CLUSTER_NAME \
    --zone=$MASTER_ZONE \
    --project=$PROJECT_NAME

kubectl create -f ./deployments/gcp-gke/gcp-storageclass-ssd.yaml
```

## Resizing cluster (helpful for development on/off toggle)

```bash
NODE_COUNT=0 ## Or 1 to reinstate the node

gcloud container clusters resize $CLUSTER_NAME --num-nodes $NODE_COUNT \
    --zone $MASTER_ZONE \
    --project $PROJECT_NAME
```

## Configure your custom [values](../../iotex/values.yaml)

`externalLBp2pIP` and `internalLBIP`: Update with the EXTERNAL-IP INTERNAL-IP returned from `kubectl get nodes --output wide`