# kubeflow 로 분석 플랫폼 구축하기

---

## kustomize 다운 받기

- windows terminal을 열어 ubuntu tab을 생성한다.

- 다음 명령어를 입력해 kustomize 를 다운 받는다.

```
$ wget https://github.com/kubernetes-sigs/kustomize/releases/download/v3.2.0/kustomize_3.2.0_linux_amd64
```

##

- 우분투 상에서 다음 명령어를 수행하여 kustomize를 설치한다.

```
$ chmod u+x kustomize_3.2.0_linux_amd64
$ sudo mv kustomize /usr/local/bin/
$ kustomize version
```

## kubeflow 다운로드 받기

```
$ git clone https://github.com/CUKykkim/manifests.git
$ cd manifests
```

## kubeflow 설치하기

```
$ while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```


## kubeflow 접속 하기

```
$ kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80
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