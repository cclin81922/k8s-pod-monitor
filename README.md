# k8s-pod-monitor

# Golang project setup

Create hello-world custom controller project structure from scratch

```
cp -r hello-world/*.go $GOPATH/src/github.com/<GITHUB USERNAME>/<REPO>/

# $GOPATH default is ~/go
```

Use `dep` to manage dependency package version

```
cd $GOPATH/src/github.com/<GITHUB USERNAME>/<REPO>/
dep init

# auto-generate 2 files and 1 folder
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
dep status
```

# Reference

* Create controllers for core resources [[blog](https://medium.com/@trstringer/create-kubernetes-controllers-for-core-and-custom-resources-62fc35ad64a3)] [[src](https://github.com/trstringer/k8s-controller-core-resource)]
* [Explain client-go](https://www.kubernetes.org.cn/1309.html)
* [Explaing controller pattern and controller components](https://engineering.bitnami.com/articles/a-deep-dive-into-kubernetes-controllers.html)
* Example kubewatch [[blog](https://engineering.bitnami.com/articles/kubewatch-an-example-of-kubernetes-custom-controller.html)] [[src](https://github.com/bitnami-labs/kubewatch)]
* Kubernetes official sample controller - custom controller example, which uses custom resources [[src](https://github.com/kubernetes/sample-controller)]
* Explaing dep [[how to use it](https://yushuangqi.com/blog/2017/gozui-xin-de-depxiang-jie.html)] [[the reason why not to use it](https://blog.wu-boy.com/2017/03/golang-dependency-management-tool-dep/)]