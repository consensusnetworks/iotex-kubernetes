# IoTeX Node in Kubernetes – AWS EKS

## Prerequisites

- AWS account and key pair
  - Run the following command to create a key pair for your AWS region (replace myRegion with your AWS region and    myKeyPair with your preferred key pair name):
    ```
    aws ec2 create-key-pair --region myRegion --key-name myKeyPair
    ```
  - TODO: what to do with the key pair...
  - 
- Installed [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

## Creating EKS Kubernetes Cluster

Replace the values for MASTER_ZONE and PUBLIC_KEY_NAME with your project's values.

```bash
NODE_COUNT=1
CLUSTER_INDEX=0
CLUSTER_NAME=iotex-node-${CLUSTER_INDEX} && echo "Cluster name is ${CLUSTER_NAME}"
MASTER_ZONE=us-east-2
PUBLIC_KEY_NAME=myKeyPair

eksctl create cluster \
    --name $CLUSTER_NAME \
    --nodes $NODE_COUNT \
    --nodes-min $NODE_COUNT \
    --nodes-max $NODE_COUNT \
    --region $MASTER_ZONE \
    --auto-kubeconfig \
    --with-oidc \
    --ssh-access \
    --ssh-public-key $PUBLIC_KEY_NAME

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

kubectl create -f ./deployments/aws-eks/aws-storageclass-gp2.yaml 
```

## Resizing cluster (helpful for development on/off toggle)

```bash
NODE_COUNT=0 ## Or 1 to reinstate the node

gcloud container clusters resize $CLUSTER_NAME --num-nodes $NODE_COUNT \
    --zone $MASTER_ZONE \
    --project $PROJECT_NAME
```

## Configure your custom [values](./values.yaml)

`externalLBp2pIP`: Update with the EXTERNAL-IP returned from `kubectl get nodes --output wide`