#Operator demo

This demo is about that use operator-sdk to write a operator.

##Install operator-sdk 
just vist [operator](https://github.com/operator-framework/operator-sdk/blob/master/doc/user/install-operator-sdk.md)

##Create a project
operator-sdk must in gopath and use go models as default.so before start,you must prepare this.

```bash
	mkdir $GOPATH/github.com/operator-learn
	cd $GOPATH/github.com/operator-learn 
	operator-sdk new demo 
```

this will take a long time after success.
Add api and controlor

```bash
	operator-sdk add api --api-version=travis.operator.com/v1 --kind=AppService 
	operator-sdk add controller --api-version=travis.operator.com/v1 --kind=AppService
```

define AppService type in operator-learn/demo/pkg/apis/travis/v1/appservice_types.go 

generate code

```bash
	operator-sdk generate k8s
```

write core code,controlor in operator-learn/demo/pkg/controller/appservice/appservice_controller.go 

##When you finish your code, you can debug use local run

```bash
	operator-sdk up local
```

deploy crd and test deployment

```bash
	kubectl apply -f deploy/crds/travis_v1_appservice_crd.yaml
	kubectl apply -f deploy/crds/travis_v1_appservice_cr.yaml
```




##Build operator image

```bash
	operator-sdk build fastop/operator-demo:v1 
	docker push fastop/operator-demo:v1
```
use your own registry addr.

update image in operator.yaml.

```bash
	image: fastop/operator-demo:v1
```

##Deploy operator to kubernetes

```bash
	 kubectl apply -f service_account.yaml -f role.yaml -f role_binding.yaml -f operator.yaml
```

