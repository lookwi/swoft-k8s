


# GCP 操作指南
## 开通K8s 
    ### 1. 开启容器API
    ```$xslt
    gcloud services enable container.googleapis.com 
    ```
    
    
    ### 2. 设置K8S集群容器地区
    ```$xslt
    gcloud config set compute/zone 地区编码
    ```
    
    ### 3. 拉取代码
    ```$xslt
    git clone https://github.com/lookwi/swoft-k8s.git
    ```
    
    
    ### 4. 转到部署代码目录
    
    ```$xslt
    cd swoft-k8s
    WORKING_DIR=$(pwd)
    ``` 


## 创建持久化磁盘

```$xslt
kubectl apply -f $WORKING_DIR/volume.yaml
```

## 部署Swoole

```$xslt
kubectl create -f $WORKING_DIR/swoole_depolyment.yaml
# 查看 部署状态
kubectl get pod -l app=swooleApp --watch
# 部署 swoole 服务
kubectl create -f $WORKING_DIR/swoole_services.yaml
# 查看 swoole 服务并等待分配外部地址
kubectl get svc -l app=swooleApp --watch

```

```$xslt
NAME        CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
swooleApp   10.51.243.233   外部地址    80:32418/TCP   1m
```




