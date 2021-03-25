1.  **查看集群节点状态** 

   kubectl  get  node

2.  **查看集群所有pod状态** 

    kubectl get pods --all-namespaces -o wide 

3.  **查看pod信息情况（describe）** 

    kubectl describe pod 服务pod名称 -n 服务空间名称 

4.  **查看节点信息情况** 

    kubectl describe node **节点名称** 

5.  实时查看日志信息 

    kubectl -n **空间名称** logs -f **服务pod名称** 

6.  日志下载到本地 

    kubectl -n **空间名称** logs **服务pod名称** > **/服务器路径/落地名称.log** 

   ------

   ###  **查看业务项目部署情况** 

7.  查看所有deployment项目部署信息
   kubectl get deployment --all-namespaces
   查看所有daemonset项目部署信息
   kubectl get daemonset --all-namespaces
   查看所有statefulset项目部署信息
   kubectl get statefulset --all-namespaces 

8.  查看某个 deployment项目的部署情况
   kubectl get deployment 项目名称 -n 空间名称 -o yaml 

9.  查看某个 daemonset项目的部署情况
   kubectl get daemonset 项目名称 -n 空间名称 -o yaml 

10.  查看某个 statefulset项目的部署情况
    kubectl get statefulset 项目名称 -n 空间名称 -o yaml 

    ------

     **查看路由信息** 

11. 查看所有业务的路由信息

    kubectl get ing --all-namespaces

12. 查看某个业务的路由信息

     kubectl get ing 业务路由名称 -n 空间名称 -o yaml