# Node.js-App-with-K8s
Running Nodejs application using K8s

Prerequisite to run on windows:
1. Choco package manager
2. Kubectl https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
3. minikube , enable hyperv on windows os https://minikube.sigs.k8s.io/docs/start/
4. Check status using " minikube status "
5. start minikube cluster : "minikube start --driver=hyperv"
6. now check minikube status again
     > minikube status
        minikube
        type: Control Plane
        host: Running
        kubelet: Running
        apiserver: Running
        kubeconfig: Configured
   
