# argo_poc
argo_R&amp;D

For demo, I`m going to use official nginx 'hello world' container

In k8s manifest File I create simple deployment with 2 replicas, service (If there is enough time - ingress)


### used command:

kubectl -n argocd get secret argocd-initial-admin-secret
kubectl -n argocd get secret argocd-initial-admin-secret -o yaml
echo <argocd-initial-admin-secret> | base64 --decode
kubectl get pod
kubectl get svc
kubectl get deploy
kubectl get rs
kubectl apply -f application.yml
kubectl delete -f application.yml
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
brew install argocd
kubectl port-forward svc/argocd-server -n argocd 8080:443

kubectl create namespace argo-events
kubectl apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml

kubectl --namespace argo-events apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/examples/eventbus/native.yaml

kubectl --namespace argo-events apply --filename event-source.yml
kubectl --namespace argo-events get eventsources
kubectl --namespace argo-events get services
kubectl --namespace argo-events get pods
kubectl --namespace argo-events port-forward <EVENTSOURCE_POD_NAME> 12000:12000 &
curl -X POST -H "Content-Type: application/json" -d '{"message":"My first webhook"}' http://localhost:12000/devops

