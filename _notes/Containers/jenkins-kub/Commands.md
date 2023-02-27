eze@eze-virtual-machine:~/jenkins-kub$ history
    1  docker build -t nodejs-api .
    2  ll
    3  kubectl create -f manifest.yml -n node
    4  kubectl create namespace node
    5  kubectl create -f manifest.yml -n node
    6  kubectl get po -o wide -n node
    7  docker images
    8  vim manifest.yml 
    9  kubectl get po -o wide -n node
   10  kubectl run busybox --image=busybox -it --restart=Never -- /bin/sh -n node
   11  kubectl run busybox --image=busybox -it --restart=Never -- /bin/sh
   12  cat index.html
   13  ll
   14  cat index.js
   15  kubectl delete -f manifest.yml -n node
   16  docker ps
   17  eval $(minikube docker-env).
   18  docker build -t nodejs-api .
   19  kubectl create -f manifest.yml -n node
   20  kubectl get po -o wide -n node
   21  kubectl delete -f manifest.yml -n node
   22  docker ps
   23  docker build -t nodejs-api .
   24  kubectl create -f manifest.yml -n node
   25  kubectl delete -f manifest.yml -n node
   26  kubectl create -f manifest.yml -n node
   27  kubectl get po -o wide -n node
   28  cd
   29  git clone https://github.com/bbachi/local-minikube-docker.git
   30  ll
   31  cd local-minikube-docker
   32  ll
   33  npm install
   34  sudo apt install npm
   35  npm start
   36  eval $(minikube docker-env).
   37  npm start
   38  eval $(minikube docker-env)
   39  npm start
   40  ll
   41  rm -rf
   42  ll
   43  rm deployment.yaml
   44  ll
   45  rm .deployment.yaml.swp
   46  eval $(minikube docker-env)
   47  docker run -d -p 5000:5000 --restart=always --name registry registry:2
   48  docker build -t my-app:1.0.0 .
   49  cd ..
   50  mkdir jenkins-kub
   51  cd jenkins-kub/
   52  touch deployment.yaml
   53  vim deployment.yaml
   54  kubectl apply -f deployment.yaml -n jenkins 
   55  vim deployment.yaml
   56  kubectl apply -f deployment.yaml -n jenkins 
   57  vim deployment.yaml
   58  kubectl apply -f deployment.yaml -n jenkins 
   59  vim deployment.yaml
   60  kubectl apply -f deployment.yaml -n jenkins 
   61  vim deployment.yaml
   62  kubectl apply -f deployment.yaml -n jenkins 
   63  vim deployment.yaml
   64  kubectl apply -f deployment.yaml -n jenkins 
   65  vim deployment.yaml
   66  minikube start
   67  eval $(minikube docker-env)
   68  mkdir jenkins
   69  cd jenkins/
   70  vim Dockerfile
   71  docker build -f Dockerfile -t mycustomcontainer:1 .
   72  docker run -it mycustomcontainer:1 bash
   73  docker build -f Dockerfile -t mycustomcontainer:1 .
   74  ll
   75  vim Dockerfile
   76  docker build -f Dockerfile -t mycustomcontainer:1 .
   77  docker search jenkins
   78  vim Dockerfile
   79  docker build -f Dockerfile -t mycustomcontainer:1 .
   80  vim Dockerfile
   81  docker build -f Dockerfile -t mycustomcontainer:1 .
   82  docker tag my-app localhost:5000/mycustomcontainer:1
   83  docker tag mycustomcontainer:1 localhost:5000/mycustomcontainer:1
   84  cd 
   85  mkdir alpine
   86  cd alpinbe
   87  cd alpine
   88  ll
   89  echo >> Dockerfile 
   90  vim Dockerfile 
   91  echo > Dockerfile 
   92  vim Dockerfile 
   93  vim index.html
   94  eval $(docker-machine env -u)
   95  eval $(minikube docker-env)
   96  kubectl get pods
   97  cd ..
   98  ll
   99  cd jenkins
  100  ll
  101  cat Dockerfile
  102  cd ..
  103  cd jenkins-kub
  104  ll
  105  touch Dockerfile
  106  vim Dockerfile
  107  touch deployment.yaml
  108  vim Deployment.yaml
  109  docker build . --tag jenkins-obi
  110  docker tag my-app localhost:5000/jenkins-obi
  111  docker images
  112  kubectl create -f deployment.yml
  113  ll
  114  cat Deployment.yaml
  115  kubectl create -f Deployment.yaml
  116  kubectl get pods
  117  kubectl delete -f Deployment.yaml
  118  kubectl create -f Deployment.yaml
  119  kubectl delete -f Deployment.yaml
  120  vim Deployment.yaml
  121  kubectl create -f Deployment.yaml
  122  kubectl get pods
  123  history
eze@eze-virtual-machine:~/jenkins-kub$ 

eze@eze-virtual-machine:~/jenkins-kub$ kubectl create -f Deployment.yaml
deployment.apps/jenkins-obi created
eze@eze-virtual-machine:~/jenkins-kub$ kubectl get pods
NAME                              READY   STATUS    RESTARTS         AGE
app-deployment-78f774d956-jpx6s   1/1     Running   0                130m
app-deployment-78f774d956-jvvj6   1/1     Running   0                130m
busybox                           0/1     Error     0                5h22m
jenkins-obi-664657b9f5-rqpcf      1/1     Running   0                4s
jenkins-obi-664657b9f5-sgp68      1/1     Running   0                4s
my-app-674df799d7-gw66d           1/1     Running   0                64m
rss-site-5b686ddfbd-7x7r7         2/2     Running   10 (140m ago)    49d
rss-site-5b686ddfbd-8rwl2         2/2     Running   10 (140m ago)    49d
rss-site-5b686ddfbd-r6vpv         2/2     Running   10 (140m ago)    49d
rss-site-5b686ddfbd-vwl45         2/2     Running   10 (4h12m ago)   49d
rss-site-5b686ddfbd-wg868         2/2     Running   10 (4h12m ago)   49d
eze@eze-virtual-machine:~/jenkins-kub$ history
    1  docker build -t nodejs-api .
