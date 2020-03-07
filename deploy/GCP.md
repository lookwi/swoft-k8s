


# GCP 操作指南
## 开通K8s 
    ### 1. 开启容器API
    ```$xslt
    gcloud services enable container.googleapis.com 
    ```
    
    
    ### 2. 设置K8S集群容器地区
    ```$xslt
    gcloud config set compute/zone 地区编码
    
    asia-southeast1 新加坡
    asia-east1 台湾
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
    
    ### 5. 设置集群
    ```
    CLUSTER_NAME=swoft-k8s
    gcloud container clusters create $CLUSTER_NAME \
        --num-nodes=2 --enable-autoupgrade --no-enable-basic-auth \
        --no-issue-client-certificate --enable-ip-alias --metadata \
        disable-legacy-endpoints=true
    ```


## 创建持久化磁盘

```$xslt
kubectl apply -f $WORKING_DIR/volume.yaml
```

## 部署Swoole

```$xslt
kubectl create -f $WORKING_DIR/swoole_deployment.yaml
# 推荐使用
kubectl apply -f $WORKING_DIR/swoole_deployment.yaml
kubectl replace -f $WORKING_DIR/swoole_deployment.yaml
# 出错，请查看 pod 是否正常
kubectl describe pod swoole-app
# 查看 部署状态
kubectl get pod -l app=swoole-app --watch
# 部署 swoole 服务
kubectl create -f $WORKING_DIR/swoole_services.yaml
kubectl apply -f $WORKING_DIR/swoole_services.yaml
# 查看 swoole 服务并等待分配外部地址
kubectl get svc -l app=swoole-app --watch

```

```$xslt
NAME         TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)        AGE
swoole-svc   LoadBalancer   10.0.7.196   外部地址   80:30375/TCP   11m
```
## 扩容

```angular2html
kubectl scale deployment swoole-app --replicas 3
```

## 自动扩容
```angular2html
kubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80

kubectl get hpa
```




