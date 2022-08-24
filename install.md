
0. 소프트웨어 설치

```
brew install yq
brew install jq
brew install kustomize <-- v 3.2.0 으로만 설치해야 함.. ㅜㅜㅜ  

```



1. EKS 클러스터 설치

```
export CLUSTER_NAME="kubeflow"
export CLUSTER_REGION="ap-northeast-2"

eksctl create cluster \
--name ${CLUSTER_NAME} \
--version 1.21 \
--region ${CLUSTER_REGION} \
--nodegroup-name linux-nodes \
--node-type m5.xlarge \
--nodes 5 \
--nodes-min 3 \
--nodes-max 5 \
--managed \
--with-oidc

aws eks describe-cluster --name $CLUSTER_NAME --region $CLUSTER_REGION

```

2. Kubeflow 설치

```
export KUBEFLOW_RELEASE_VERSION=v1.5.1
export AWS_RELEASE_VERSION=v1.5.1-aws-b1.0.1
git clone https://github.com/awslabs/kubeflow-manifests.git && cd kubeflow-manifests
git checkout ${AWS_RELEASE_VERSION}
git clone --branch ${KUBEFLOW_RELEASE_VERSION} https://github.com/kubeflow/manifests.git upstream

while ! kustomize build deployments/vanilla | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 30; done
```

3. Kubeflow 동작 확인

```
kubectl get pods -n cert-manager
kubectl get pods -n istio-system
kubectl get pods -n auth
kubectl get pods -n knative-eventing
kubectl get pods -n knative-serving
kubectl get pods -n kubeflow
kubectl get pods -n kubeflow-user-example-com
# Depending on your installation if you installed KServe
kubectl get pods -n kserve
```

4. 콘솔 로그인

```
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80

kubectl get pods --all-namespaces | grep dex
auth                        dex-5ddf47d88d-2jlsd                                         1/1     Running            0          63m
```


## 참고자료 ##
* https://aws.amazon.com/ko/about-aws/whats-new/2022/05/aws-distribution-kubeflow-supporting-kubeflow-v1-4-1-generally-available/
* [kubeflow 1.4 설치](https://velog.io/@csk6124/Kubeflow-1.4-%EC%84%A4%EC%B9%98)
* https://aiden-jeon.github.io/issue_with_auth/
* https://lcc3108.github.io/articles/2020-12/Istio+Dex-%EC%9D%B8%EC%A6%9D
* https://awslabs.github.io/kubeflow-manifests/docs/deployment/
* https://kangwoo.kr/2021/01/22/dex-%ea%b8%b0%eb%b0%98%ec%9d%98-kubeflow%ec%97%90-%ec%82%ac%ec%9a%a9%ec%9e%90-%ec%b6%94%ea%b0%80%ed%95%98%ea%b8%b0/
