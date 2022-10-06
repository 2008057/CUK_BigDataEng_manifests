# kubeflow 로 분석 플랫폼 구축하기

## kubernetes 버전 확인

- kubernetes 1.21 버전으로 설치해야 안정적으로 설치가능한 것으로 확인되었습니다. 
- docker를 이미 설치하신 분이시라면 아래 버전으로 docker를 재 설치 하여 주시길 바랍니다.

  * [Windows 사용자용 Docker Desktop](https://docs.docker.com/desktop/previous-versions/3.x-windows/)
  * [Mac 사용자용 Docker Desktop](https://docs.docker.com/desktop/previous-versions/3.x-mac/)


## kustomize 다운 받기

- windows terminal을 열어 ubuntu tab을 생성한다.

- 다음 명령어를 입력해 kustomize 를 다운 받는다.

```
wget https://github.com/kubernetes-sigs/kustomize/releases/download/v3.2.0/kustomize_3.2.0_linux_amd64
```

## kustomize를 활용한 kubeflow 설치

- 우분투 상에서 다음 명령어를 수행하여 kustomize를 설치한다.

```
sudo mv kustomize_3.2.0_linux_amd64 /usr/local/bin/kustomize
sudo mv kustomize /usr/local/bin/
kustomize version
```

## kubeflow 다운로드 받기

```
git clone https://github.com/CUKykkim/manifests.git
cd manifests
```


## kubeflow 설치하기

- `kustomize` 명령어를 입력하여 모든 yaml파일을 k8s상에서 수행
```
while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```

- 설치가 다 끝났다면, 다음 명령어를 사용하여 k8s 시스템상에 pod의 상태를 확인한다.

```
kubectl get po -A
```

- 설치가 완료되면 아래와 같은 결과가 나온다.

```
NAMESPACE                   NAME                                                        READY   STATUS             RESTARTS   AGE
auth                        dex-5ddf47d88d-82l5c                                        1/1     Running            1          75m
cert-manager                cert-manager-7dd5854bb4-bm2tq                               1/1     Running            0          75m
cert-manager                cert-manager-cainjector-64c949654c-wx66q                    1/1     Running            3          75m
cert-manager                cert-manager-webhook-6b57b9b886-cgvt9                       1/1     Running            0          75m
istio-system                authservice-0                                               1/1     Running            0          75m
istio-system                cluster-local-gateway-75cb7c6c88-9pwrw                      1/1     Running            0          75m
istio-system                istio-ingressgateway-79b665c95-pknck                        1/1     Running            0          75m
istio-system                istiod-86457659bb-29sb2                                     1/1     Running            0          75m
knative-eventing            eventing-controller-79895f9c56-h9gsl                        1/1     Running            0          75m
knative-eventing            eventing-webhook-78f897666-d5c5j                            1/1     Running            0          75m
knative-eventing            imc-controller-688df5bdb4-4bpdf                             1/1     Running            0          75m
knative-eventing            imc-dispatcher-646978d797-6cxlv                             1/1     Running            0          75m
knative-eventing            mt-broker-controller-67c977497-jhzsc                        1/1     Running            0          75m
knative-eventing            mt-broker-filter-66d4d77c8b-qs6ct                           1/1     Running            0          75m
knative-eventing            mt-broker-ingress-5c8dc4b5d7-qt26s                          1/1     Running            0          75m
knative-serving             activator-7476cc56d4-ngn67                                  2/2     Running            1          74m
knative-serving             autoscaler-5c648f7465-5mzf7                                 2/2     Running            1          74m
knative-serving             controller-57c545cbfb-tqwlv                                 2/2     Running            1          74m
knative-serving             istio-webhook-578b6b7654-kpmml                              2/2     Running            1          74m
knative-serving             networking-istio-6b88f745c-ptq22                            2/2     Running            1          74m
knative-serving             webhook-6fffdc4d78-2x5fh                                    2/2     Running            1          74m
kube-system                 coredns-558bd4d5db-57xg2                                    1/1     Running            0          77m
kube-system                 coredns-558bd4d5db-mctm6                                    1/1     Running            0          77m
kube-system                 etcd-docker-desktop                                         1/1     Running            0          77m
kube-system                 kube-apiserver-docker-desktop                               1/1     Running            0          77m
kube-system                 kube-controller-manager-docker-desktop                      1/1     Running            0          77m
kube-system                 kube-proxy-b9hkn                                            1/1     Running            0          77m
kube-system                 kube-scheduler-docker-desktop                               1/1     Running            2          77m
kube-system                 storage-provisioner                                         1/1     Running            3          76m
kube-system                 vpnkit-controller                                           1/1     Running            3          76m
kubeflow-user-example-com   hellokubeflow-2-r5dd4-433021892                             0/2     Completed          0          30m
kubeflow-user-example-com   ml-pipeline-ui-artifact-5dd95d555b-ht5ww                    2/2     Running            0          68m
kubeflow-user-example-com   ml-pipeline-visualizationserver-6b44c6759f-vccd5            2/2     Running            0          68m
kubeflow                    admission-webhook-deployment-7c6d8876db-w7f48               1/1     Running            0          74m
kubeflow                    cache-deployer-deployment-79fdf9c5c9-6hjh7                  2/2     Running            1          74m
kubeflow                    cache-server-5bdf4f4457-6skz9                               2/2     Running            0          74m
kubeflow                    centraldashboard-6f56b874f9-p9mlm                           1/1     Running            0          74m
kubeflow                    jupyter-web-app-deployment-7d4bfc79b-7cjtg                  1/1     Running            0          74m
kubeflow                    katib-controller-557f4f848-ph5f9                            1/1     Running            0          74m
kubeflow                    katib-db-manager-5cb7c77df8-p4h6d                           1/1     Running            0          74m
kubeflow                    katib-mysql-7894994f88-dpt2g                                1/1     Running            0          74m
kubeflow                    katib-ui-8cc499fcf-tfsvm                                    1/1     Running            0          74m
kubeflow                    kfserving-controller-manager-0                              2/2     Running            0          73m
kubeflow                    kfserving-models-web-app-db6695f6-pjg4t                     2/2     Running            0          74m
kubeflow                    kubeflow-pipelines-profile-controller-7b947f4748-dgkkh      1/1     Running            0          74m
kubeflow                    metacontroller-0                                            1/1     Running            0          73m
kubeflow                    metadata-envoy-deployment-5b4856dd5-gdchz                   1/1     Running            0          74m
kubeflow                    metadata-grpc-deployment-6b5685488-c72pq                    2/2     Running            3          74m
kubeflow                    metadata-writer-548bd879bb-flwcn                            2/2     Running            1          74m
kubeflow                    minio-5b65df66c9-4h77p                                      2/2     Running            0          74m
kubeflow                    ml-pipeline-8c4b99589-h6vrn                                 2/2     Running            1          74m
kubeflow                    ml-pipeline-persistenceagent-d6bdc77bd-zq2gn                2/2     Running            0          74m
kubeflow                    ml-pipeline-scheduledworkflow-5db54d75c5-272tw              2/2     Running            0          74m
kubeflow                    ml-pipeline-ui-5bd8d6dc84-5mnxg                             2/2     Running            0          74m
kubeflow                    ml-pipeline-viewer-crd-68fb5f4d58-fgljw                     2/2     Running            1          74m
kubeflow                    ml-pipeline-visualizationserver-8476b5c645-7w64q            2/2     Running            0          74m
kubeflow                    mpi-operator-d5bfb8489-bvz6s                                0/1     CrashLoopBackOff   17         74m
kubeflow                    mysql-f7b9b7dd4-h9trv                                       2/2     Running            0          74m
kubeflow                    notebook-controller-deployment-8b64878d4-dqw7w              1/1     Running            0          74m
kubeflow                    profiles-deployment-64675b488b-g5k9k                        2/2     Running            0          74m
kubeflow                    tensorboard-controller-controller-manager-9ff658cf8-q2szk   3/3     Running            4          74m
kubeflow                    tensorboards-web-app-deployment-754f55996d-z76cq            1/1     Running            0          74m
kubeflow                    training-operator-868876bcc8-vtglh                          1/1     Running            0          74m
kubeflow                    volumes-web-app-deployment-6dcbf595b5-74b8n                 1/1     Running            0          74m
kubeflow                    workflow-controller-5cbbb49bd8-cgkth                        2/2     Running            3          74m
```

## kubeflow 접속 하기

**다음 둘 중 하나의 방식을 사용한다.**


+ port forward 방식

```
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80
```

+ End Point를 외부로 노출하는 방식

```
kubectl edit svc istio-ingressgateway -n istio-system
```

# 대시 보드에 접속하기

초기에 비밀번호는 다음과 같다.

```
user@example.com
12341234
```



## kubeflow pipeline 돌려보기


- hello pipeline

```
import kfp
import kfp.dsl as dsl

@dsl.pipeline(
    name='HelloKubeflow',
    description='Print HelloWorld'
)
def hellokubelfow_pipeline():
    hello = dsl.ContainerOp(
        name='HelloKubeflow',
        image='alpine',
        command=['sh', '-c'],
        arguments=['echo "hello Kubeflow"']
    )

if __name__ == '__main__':
    kfp.compiler.Compiler().compile(hellokubelfow_pipeline, 'containerop.pipeline.tar.gz')
```