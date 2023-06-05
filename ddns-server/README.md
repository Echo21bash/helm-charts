# Ddns Helm Chart
> Dynamic DNS Server for Docker with Web UI written in Go,you can setup and maintain your dyndns entries via simple web ui.

官方github地址： 

- https://github.com/benjaminbear/docker-ddns-server


## 使用帮助

### 1. 环境需求
- Kubernetes 1.20+
- Helm 3.7.0+

### 2. 添加helm仓库
```bash
export NAMESPACE=140052-public
helm repo add $NAMESPACE https://repomanage.rdc.aliyun.com/helm_repositories/$NAMESPACE --username=jPm8Z0 --password=EXr5KzbS1G
helm repo list 
helm search repo 140052-public
```

若添加后有更新需求，使用下面命令更新helm仓库
```bash
helm repo update 140052-public
```
### 3. 部署服务
```bash
helm install -n ${namespace} --create-namespace  ${release_name} 140052-public/ddns-server
  --set env.DDNS_ADMIN_LOGIN: 'admin:$apr1$j1mUQ18Z$Moa8a7FOwjvnSS/BRYIgm0' \
  --set env.DDNS_DOMAINS: 'gioneco.local.com' \
  --set env.DDNS_PARENT_NS: 'ns.local.com' \
  --set env.DDNS_DEFAULT_TTL: '3600' \
  --set persistentStorage.dbData.storageClass='k8s-nfs-storage' \
  --set persistentStorage.cacheData.storageClass='k8s-nfs-storage'
```
- 命令解析：

    `-n ${namespace} --create-namespace`: 创建并部署在那个名称空间

    `${release_name}`: release实例的名称

    `zyh-harbor/ddns-server`: release实例的路径，可使用"helm search repo zyh-harbor"命令查看

    `--set`: 直接在命令行中设置值，允许多个值

    **注：**

    `DDNS_ADMIN_LOGIN`是用于 web ui 的 htpasswd 用户名密码组合。您可以使用 htpasswd 创建一个：
    
    ```shell
    #默认值admin 2l4NX#zmJjeg
    htpasswd -nb user password
    ```
    
    `DDNS_DOMAINS`是网络服务的域和您的 dyndns 服务器的域区域（逗号分隔列表）
    
    `DDNS_PARENT_NS`是您域的父名称服务器
    
    `DDNS_DEFAULT_TTL`是您的 dyndns 服务器的默认 TTL

### 4. 部署成功后查看状态
```bash
helm list -n ${namespace}
helm status -n ${namespace} ${release_name}
kubectl get po,svc -n ${namespace}
```

### 5.卸载chart
```bash
helm uninstall -n ${namespace} ${release_name}
```

