ocker images# Node.js-App-with-K8s
Running Nodejs application using K8s

## Prerequisite to run on windows:
1. Docker account and Docker desktop installed, must be in running state
2. Choco package manager
3. Kubectl https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
4. minikube , enable hyperv on windows os https://minikube.sigs.k8s.io/docs/start/
5. Check status using " minikube status "
6. start minikube cluster : "minikube start --driver=hyperv"
7. now check minikube status again
     > minikube status
        minikube
        type: Control Plane
        host: Running
        kubelet: Running
        apiserver: Running
        kubeconfig: Configured
## Steps:
1. Clone this repository in VS code
2. got to current project directory and go to terminal
3. initiate project with json file and  to install required dependencies , its ok to delete npm_module folder later on before doing actual k8s work. Do "npm init -y" and then "npm install express"
4. if you want to clear docker build cache "docker builder prune -a"
5. Make sure to logiin to your personal docker account - "docker login"
6. then build project with - "docker build . -t namu123/k8s-web-hello" . This will push your repo to docker hub which has account name namu123
7. now you can check running docker images with - "docker images"
8. Now, this repository is dockerized & now its time add it to k8s cluster & perform other activities like scaling, accessing on cluster ip , adding load balancer port service etc. For this below remaining prt
9. go to powershell in windows  and do "minikube start"
10. Check status:  minikube status
                    minikube
                    type: Control Plane
                    host: Running
                    kubelet: Running
                    apiserver: Running
                    kubeconfig: Configured
11. kubectl get pods
12. kubectl get deploy
13. kubectl get svc
14. To create new deployment of current project - " kubectl create deployment k8s-web-hello --image=namu123/k8s-web-hello "
15. This will start one pod in cluster -  "kubectl get pods"
NAME                             READY   STATUS    RESTARTS   AGE
k8s-web-hello-785b88d575-5vxbb   1/1     Running   0          35s

>"kubectl get deploy"
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
k8s-web-hello   1/1     1            1           8m13s

> "kubectl scale deploy k8s-web-hello --replicas=4"
deployment.apps/k8s-web-hello scaled

>   kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
k8s-web-hello-785b88d575-5vxbb   1/1     Running   0          9m22s
k8s-web-hello-785b88d575-fcdps   1/1     Running   0          29s
k8s-web-hello-785b88d575-h5lgh   1/1     Running   0          29s
k8s-web-hello-785b88d575-mwjd6   1/1     Running   0          29s

>  kubectl get pods -o wide
NAME                             READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
k8s-web-hello-785b88d575-5vxbb   1/1     Running   0          9m32s   10.244.0.3   minikube   <none>           <none>
k8s-web-hello-785b88d575-fcdps   1/1     Running   0          39s     10.244.0.5   minikube   <none>           <none>
k8s-web-hello-785b88d575-h5lgh   1/1     Running   0          39s     10.244.0.4   minikube   <none>           <none>
k8s-web-hello-785b88d575-mwjd6   1/1     Running   0          39s     10.244.0.6   minikube   <none>           <none>

17. Now , add service to access web app on port. Use - "expose deployment k8s-web-hello --port=3000"
     -> service/k8s-web-hello exposed
18. Now check services,  "kubectl get svc"
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
k8s-web-hello   ClusterIP   10.107.120.24   <none>        3000/TCP   14s
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP    10m
19. Now go inside minikube cluster and access webapp using ip and port
    > minikube ssh
                         _             _
            _         _ ( )           ( )
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ curl 10.107.120.24:3000
VERSION 2: Hello from the k8s-web-hello-785b88d575-5vxbb$
$ curl 10.107.120.24:3000; echo
VERSION 2: Hello from the k8s-web-hello-785b88d575-mwjd6

$ curl 10.107.120.24:3000
VERSION 2: Hello from the k8s-web-hello-785b88d575-h5lgh$
20. Now, you can delete service and expose again with new type NodePort OR Load Balancer using " kubectl expose deployment k8s-web-hello --type=NodePort --port=300" OR " kubectl expose deployment k8s-web-hello --type=LoadBalancer --port=3000"

