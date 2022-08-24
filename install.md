* https://awslabs.github.io/kubeflow-manifests/docs/deployment/

1. EKS 클러스터 설치

```
export CUSTER_NAME="kubeflow"
export CLUSTER_REGION="ap-northeast-2"

eksctl create cluster \
--name ${CLUSTER_NAME} \
--version 1.21 \
--region ${CLUSTER_REGION} \
--nodegroup-name linux-nodes \
--node-type m5.xlarge \
--nodes 5 \
--nodes-min 5 \
--nodes-max 10 \
--managed \
--with-oidc
```
