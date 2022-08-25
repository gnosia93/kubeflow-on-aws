아래의 명령어를 이용하여 EKS 클러스터를 삭제한다. EKS 위에서 동작하는 큐브플로우 역시 삭제된다.

```
export CLUSTER_NAME="kubeflow"

eksctl delete cluster --name ${CLUSTER_NAME}
```
