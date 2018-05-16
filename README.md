# k8s-pod-monitor

# Dependency package list

main.go

* os
* os/signal
* syscall
* github.com/sirupsen/logrus
* k8s.io/api/core/v1
* k8s.io/apimachinery/pkg/apis/meta/v1
* k8s.io/apimachinery/pkg/runtime
* k8s.io/apimachinery/pkg/watch
* k8s.io/client-go/kubernetes
* k8s.io/client-go/tools/cache
* k8s.io/client-go/tools/clientcmd
* k8s.io/client-go/util/workqueue

controller.go

* fmt
* time
* github.com/sirupsen/logrus
* k8s.io/apimachinery/pkg/util/runtime
* k8s.io/apimachinery/pkg/util/wait
* k8s.io/client-go/kubernetes
* k8s.io/client-go/tools/cache
* k8s.io/client-go/util/workqueue
	
handler.go

* github.com/sirupsen/logrus
* k8s.io/api/core/v1

# Golang project setup

Create a custom controller project structure from scratch

```
cp -r hello-controller/*.go $GOPATH/src/github.com/<GITHUB USERNAME>/<REPO>/

# $GOPATH default is ~/go
```

Use `dep` to manage dependency package version

```
cd $GOPATH/src/github.com/<GITHUB USERNAME>/<REPO>/
dep init

# auto-generate
# 
# * Gopkg.lock
# * Gopkg.toml
# * vendor/

dep status
```

Use `vscode` to edit source code

```
# Open folder ...
# Choose $GOPATH/src/github.com/<GITHUB USERNAME>/<REPO>/
# Edit main.go
# Edit controller.go
# Edit handler.go
```

Use `dep` again after changing package dependency

```
dep ensure

# auto-update
# 
# * Gopkg.lock
# * vendor/

dep status

# sample output
#
# PROJECT                          CONSTRAINT     VERSION        REVISION  LATEST   PKGS USED
# github.com/Sirupsen/logrus       v1.0.5         v1.0.5         c155da1   v1.0.5   1   
# github.com/davecgh/go-spew       v1.1.0         v1.1.0         346938d   v1.1.0   1   
# github.com/ghodss/yaml           v1.0.0         v1.0.0         0ca9ea5   v1.0.0   1   
# github.com/gogo/protobuf         v1.0.0         v1.0.0         1adfc12   v1.0.0   2   
# github.com/golang/glog           branch master  branch master  23def4e   23def4e  1   
# github.com/golang/protobuf       v1.1.0         v1.1.0         b4deda0   v1.1.0   5   
# github.com/google/gofuzz         branch master  branch master  24818f7   24818f7  1   
# github.com/googleapis/gnostic    v0.1.0         v0.1.0         ee43cbb   v0.1.0   3   
# github.com/hashicorp/golang-lru  branch master  branch master  0fb14ef   0fb14ef  2   
# github.com/howeyc/gopass         branch master  branch master  bf9dde6   bf9dde6  1   
# github.com/imdario/mergo         v0.3.4         v0.3.4         9d5f127   v0.3.4   1   
# github.com/json-iterator/go      1.1.3          1.1.3          ca39e5a   1.1.3    1   
# github.com/modern-go/concurrent  1.0.3          1.0.3          bacd9c7   1.0.3    1   
# github.com/modern-go/reflect2    1.0.0          1.0.0          1df9eeb   1.0.0    1   
# github.com/spf13/pflag           v1.0.1         v1.0.1         583c0c0   v1.0.1   1   
# golang.org/x/crypto              branch master  branch master  1a580b3   1a580b3  1   
# golang.org/x/net                 branch master  branch master  2491c5d   2491c5d  5   
# golang.org/x/sys                 branch master  branch master  7c87d13   7c87d13  2   
# golang.org/x/text                v0.3.0         v0.3.0         f21a4df   v0.3.0   14  
# golang.org/x/time                branch master  branch master  fbb02b2   fbb02b2  1   
# gopkg.in/inf.v0                  v0.9.1         v0.9.1         d2d2541   v0.9.1   1   
# gopkg.in/yaml.v2                 v2.2.1         v2.2.1         5420a8b   v2.2.1   1   
# k8s.io/api                       branch master  branch master  1b6ea75   1b6ea75  28  
# k8s.io/apimachinery              branch master  branch master  8e510c8   8e510c8  37  
# k8s.io/client-go                 v7.0.0         v7.0.0         23781f4   v7.0.0   54  
```

# Reference

* Create controllers for core resources [[blog](https://medium.com/@trstringer/create-kubernetes-controllers-for-core-and-custom-resources-62fc35ad64a3)] [[src](https://github.com/trstringer/k8s-controller-core-resource)]
* [Explain client-go](https://www.kubernetes.org.cn/1309.html)
* [Explain controller pattern and controller components](https://engineering.bitnami.com/articles/a-deep-dive-into-kubernetes-controllers.html)
* Example controller - kubewatch [[blog](https://engineering.bitnami.com/articles/kubewatch-an-example-of-kubernetes-custom-controller.html)] [[src](https://github.com/bitnami-labs/kubewatch)]
* [Kubernetes official sample controller - custom controller example, which uses custom resources](https://github.com/kubernetes/sample-controller)
* Explain dep [[how to use it](https://yushuangqi.com/blog/2017/gozui-xin-de-depxiang-jie.html)] [[the reason why not to use it](https://blog.wu-boy.com/2017/03/golang-dependency-management-tool-dep/)]