# The "Employee Manager" application

_Kubernetes, Minikube, Docker_

## How to start the Demo project with all services in the Minikube

Download repository if you don't have:
```
git clone https://github.com/Kv-062-DevOps/k8s-deploy.git
```
Open the folder **`k8s-deploy`** in commandline console and execute:
```
minikube start
 
kubectl apply -f db.yml
kubectl apply -f get.yml
kubectl apply -f post.yml
kubectl apply -f front.yml
 
```
Use command to get the address link:
```
minikube service front-srv --url
```
Open it in your web browser (for example, <http://172.17.0.2:30808>).  

---
To visit a Kubernetes WEB dashboard, use command 
```
minikube dashboard
```

If you want to see a raw data in the GET service, use:
```
minikube service get-srv -n demo --url
```
for example, link will be <http://172.17.0.2:30593>

If you want to see a raw data in the Backend service, get the address using command:
```
minikube service back-srv -n demo --url
```
and open this link in browser, adding a path `/list`, for example: <http://172.17.0.2:31486/list>

### Clear resources after work:
```
kubectl delete deployments back get post db front
kubectl delete services back-srv get-srv post-srv db-srv front-srv 
 
kubectl get pods
kubectl get services
kubectl get services
 
minikube stop
 
```
---
_https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request_


