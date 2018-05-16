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

# test k8s-pod-monitor using imperative commands
kubectl run nginx --image=nginx # create
kubectl scale --replicas=2 deploy/nginx # update
kubectl delete deploy/nginx # delete
```

Note

1. Use `~/.kube/config` kubeconfig file
2. Only monitor pods in the `default` namespace -> see creation of `ctrl-informer` object

Code :: custom types

1. `Controller` struct type -> see [controller.go](controller.go)
2. `Handler` interface type -> see [handler.go](handler.go)
3. `TestHandler` struct type -> see [handler.go](handler.go)

Code :: main loop 1

1. main()
2. Run() -> see methods of `Controller` struct type
3. Run() -> see methods of `ctrl-informer` object
4. AddFunc() | UpdateFunc() | DeleteFunc() -> see event handlers of `ctrl-informer` object

Code :: main loop 2

1. main()
2. Run() -> see methods of `Controller` struct type
3. Until() -> see functions of `wait` package
4. runWorker() -> see methods of `Controller` struct type
5. processNextItem() -> see methods of `Controller` struct type
6. ObjectDeleted() | ObjectCreatedORUpdated() -> see methods of `Handler ` interface type
7. ObjectDeleted() | ObjectCreatedORUpdated() -> see methods of `TestHandler ` struct type

# Dependency package list

main.go

* os
  *	 to get value of environment variable `HOME`
* os/signal
  * to handle `POSIX signals`
* syscall
  * to handle `POSIX signals`
* github.com/sirupsen/logrus
  * to log message with `log levels`
* k8s.io/api/core/v1
  * to support creation of `ctrl-informer` object
* k8s.io/apimachinery/pkg/apis/meta/v1
  * to support creation of `ctrl-informer` object
* k8s.io/apimachinery/pkg/runtime
  * to support creation of `ctrl-informer` object
* k8s.io/apimachinery/pkg/watch
  * to support creation of `ctrl-informer` object
* k8s.io/client-go/kubernetes
  * to new `k8s-client` object of type `kubernetes.Interface` 
* k8s.io/client-go/tools/cache
  * to new `ctrl-informer` object
* k8s.io/client-go/tools/clientcmd
  * to new `k8s-config` object
* k8s.io/client-go/util/workqueue
  * to new `ctrl-workqueue` object

controller.go

* fmt
* time
* github.com/sirupsen/logrus
  * to log message with `log levels` 
* k8s.io/apimachinery/pkg/util/runtime
  * to handle `panic` and `err` 
* k8s.io/apimachinery/pkg/util/wait
  * to run the `runWorker` method every second with a stop channel 
* k8s.io/client-go/kubernetes
  * to support definition of `Controller` struct type
* k8s.io/client-go/tools/cache
  * to support definition of `Controller` struct type
* k8s.io/client-go/util/workqueue
  * to support definition of `Controller` struct type
	
handler.go

* github.com/sirupsen/logrus
  * to log message with `log levels`
* k8s.io/api/core/v1
  * to convert object type from `interface{}` to `Pod` struct

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
* [Go by Example: Signals](https://gobyexample.com/signals)
* [Go by Example: Defer](https://gobyexample.com/defer)
* [Go by Example: Channels](https://gobyexample.com/channels)
* [Go by Example: Closing Channels](https://gobyexample.com/closing-channels)
* [Go by Example: Channel Buffering](https://gobyexample.com/channel-buffering)
* [Go by Example: Channel Directions](https://gobyexample.com/channel-directions)
* [A Tour of Go: Structs](https://tour.golang.org/moretypes/2)
* [A Tour of Go: Pointer receivers](https://tour.golang.org/methods/4)
* [A Tour of Go: Interfaces](https://tour.golang.org/methods/9)
* [A Tour of Go: The empty interface](https://tour.golang.org/methods/14)
* [A Tour of Go: Type assertions](https://tour.golang.org/methods/15)
* [A Tour of Go: Goroutines](https://tour.golang.org/concurrency/1)
* [Explain the empty struct](https://dave.cheney.net/2014/03/25/the-empty-struct)
* [Managing kubernetes objects using imperative commands](https://kubernetes.io/docs/concepts/overview/object-management-kubectl/imperative-command/#how-to-update-objects)
