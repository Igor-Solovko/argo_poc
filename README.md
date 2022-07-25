# argo_poc
argo_R&amp;D

For demo, I`m going to use official nginx 'hello world' container

In k8s manifest File I create simple deployment with 2 replicas, service (If there is enough time - ingress)


### used command:

minikube start <br>
kubectl get pod <br>
kubectl get svc <br>
kubectl get deploy <br>
kubectl get rs <br>
kubectl apply -f application.yml <br>
kubectl delete -f application.yml <br>
kubectl create namespace argocd <br>
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/ manifests/install.yaml  <br>
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo  <br>

kubectl port-forward svc/argocd-server -n argocd 8080:443 & <br>

kubectl create namespace argo-events <br>
kubectl apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml <br>

kubectl --namespace argo-events apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/examples/eventbus/native.yaml <br>

kubectl --namespace argo-events apply --filename event-source.yml <br>
kubectl --namespace argo-events get eventsources <br>
kubectl --namespace argo-events get services <br>
kubectl --namespace argo-events get pods <br>
kubectl --namespace argo-events port-forward svc/webhook-eventsource-svc 12000:12000 & <br>
curl -X POST -H "Content-Type: application/json" -d '{"message":"nginxdemos/hello:latest"}' http://localhost:12000/devops-toolkit  <br>

kubectl --namespace argo-events apply -f sensor.yaml  <br>
kubectl --namespace argo-events logs --selector app=payload  <br>
kubectl --namespace argo-events delete pods --selector app=payload  <br>
