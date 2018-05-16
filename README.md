# k8s-pod-monitor

Usage

```
# download source code of k8s-pod-monitor
go get github.com/cclin81922/k8s-pod-monitor

# change current working dir
cd $GOPATH/bin/src/github.com/cclin81922/k8s-pod-monitor # $GOPATH default is ~/go

# prepare dependency package of k8s-pod-monitor
dep ensure

# build and install k8s-pod-monitor
go install

# start k8s-pod-monitor
$GOPATH/bin/k8s-pod-monitor # make sure that k8s cluster is running before starting k8s-pod-monitor
```

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
mkdir -p $GOPATH/src/github.com/<GITHUB USERNAME>/<REPO>/
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

Use `hello-controller.tgz` to restore a project

```
wget https://raw.githubusercontent.com/cclin81922/k8s-pod-monitor/master/hello-controller.tgz

tar -zxf hello-controller.tgz

cp hello-controller/*.go .
cp hello-controller/Gopkg.* .

rm -rf hello-controller hello-controller.tgz
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
# github.com/Sirupsen/logrus       ^1.0.5         v1.0.5         c155da1   v1.0.5   1   
# github.com/davecgh/go-spew       ^1.1.0         v1.1.0         346938d   v1.1.0   1   
# github.com/ghodss/yaml           ^1.0.0         v1.0.0         0ca9ea5   v1.0.0   1   
# github.com/gogo/protobuf         ^1.0.0         v1.0.0         1adfc12   v1.0.0   2   
# github.com/golang/glog           branch master  branch master  23def4e   23def4e  1   
# github.com/golang/protobuf       ^1.0.0         v1.0.0         9255415   v1.1.0   5   
# github.com/google/gofuzz         branch master  branch master  24818f7   24818f7  1   
# github.com/googleapis/gnostic    ^0.1.0         v0.1.0         ee43cbb   v0.1.0   3   
# github.com/hashicorp/golang-lru  branch master  branch master  0fb14ef   0fb14ef  2   
# github.com/howeyc/gopass         branch master  branch master  bf9dde6   bf9dde6  1   
# github.com/imdario/mergo         v0.3.4         v0.3.4         9d5f127   v0.3.4   1   
# github.com/json-iterator/go      ^1.1.3         1.1.3          ca39e5a   1.1.3    1   
# github.com/modern-go/concurrent  ^1.0.3         1.0.3          bacd9c7   1.0.3    1   
# github.com/modern-go/reflect2    ^1.0.0         1.0.0          1df9eeb   1.0.0    1   
# github.com/spf13/pflag           ^1.0.1         v1.0.1         583c0c0   v1.0.1   1   
# golang.org/x/crypto              branch master  branch master  d644981   1a580b3  1   
# golang.org/x/net                 branch master  branch master  61147c4   2491c5d  5   
# golang.org/x/sys                 branch master  branch master  2281fa9   7c87d13  2   
# golang.org/x/text                ^0.3.0         v0.3.0         f21a4df   v0.3.0   14  
# golang.org/x/time                branch master  branch master  fbb02b2   fbb02b2  1   
# gopkg.in/inf.v0                  ^0.9.1         v0.9.1         d2d2541   v0.9.1   1   
# gopkg.in/yaml.v2                 ^2.2.1         v2.2.1         5420a8b   v2.2.1   1   
# k8s.io/api                       branch master  branch master  6fb8575   1b6ea75  28  
# k8s.io/apimachinery              branch master  branch master  ba81bc6   8e510c8  38  
# k8s.io/client-go                 ^7.0.0         v7.0.0         23781f4   v7.0.0   54  
```

Use `go run` to start running k8s-pod-controller

```
go run *.go

# sample output
#
# INFO[0000] Successfully constructed k8s client          
# INFO[0000] Controller.Run: initiating                   
# INFO[0000] Controller.Run: cache sync complete          
# INFO[0000] Controller.runWorker: starting               
# INFO[0000] Controller.processNextItem: start       
```

Use `go build` to build k8s-pod-controller

```
go build

# auto-generate
#
# * k8s-pod-controller
```

Use `go install` to build and install k8s-pod-controller

```
go install

# auto-generate
#
# * $GOPATH/bin/k8s-pod-controller
```


# Reference

* Create controllers for core resources [[blog](https://medium.com/@trstringer/create-kubernetes-controllers-for-core-and-custom-resources-62fc35ad64a3)] [[src](https://github.com/trstringer/k8s-controller-core-resource)]
* [Explain client-go](https://www.kubernetes.org.cn/1309.html)
* [Explain controller pattern and controller components](https://engineering.bitnami.com/articles/a-deep-dive-into-kubernetes-controllers.html)
* Example controller - kubewatch [[blog](https://engineering.bitnami.com/articles/kubewatch-an-example-of-kubernetes-custom-controller.html)] [[src](https://github.com/bitnami-labs/kubewatch)]
* [Kubernetes official sample controller - custom controller example, which uses custom resources](https://github.com/kubernetes/sample-controller)
* Explain dep [[how to use it](https://yushuangqi.com/blog/2017/gozui-xin-de-depxiang-jie.html)] [[the reason why not to use it](https://blog.wu-boy.com/2017/03/golang-dependency-management-tool-dep/)]
