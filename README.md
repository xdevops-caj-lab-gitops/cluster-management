# Kubernetes manifests for cluster management

## 前提条件

已经按照[gitops-repo-example](https://github.com/xdevops-caj-lab-gitops/gitops-repo-example)指引：
- 配置好`openshift-gitops`的ArgoCD的Admin权限
- 配置好自定义的`myargocd`的ArgoCD实例
- 配置好`myargocd`的ArgoCD的Admin权限
- 执行Clean Up命令

### Create cluster-scoped resoruces

Create cluster-scoped resources in default `openshift-gitops` argocd instance:

```bash
oc apply -f argocd/apps/cluster-config.yaml -n openshift-gitops
```

Sync `cluster-config` application mannualy on ArgoCD web console.

### Create application-scoped resources

Application config repositories：
- [spring-petclinic-config](https://github.com/xdevops-caj-lab-gitops/spring-petclinic-config)
- [parksmap-config](https://github.com/xdevops-caj-lab-gitops/parksmap-config)

Create application-scoped resources in customized `myargocd` argocd instance:
```bash
# parksmap
oc label namespace parksmap-dev argocd.argoproj.io/managed-by=myargocd
oc label namespace parksmap-test argocd.argoproj.io/managed-by=myargocd

oc apply -f argocd/apps/parksmap-dev.yaml -n myargocd
oc apply -f argocd/apps/parksmap-test.yaml -n myargocd

# spring-petclinic
oc label namespace spring-petclinic-dev argocd.argoproj.io/managed-by=myargocd
oc label namespace spring-petclinic-test argocd.argoproj.io/managed-by=myargocd

oc apply -f argocd/apps/spring-petclinic-dev.yaml -n myargocd
oc apply -f argocd/apps/spring-petclinic-test.yaml -n myargocd
```





