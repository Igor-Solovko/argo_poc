# argo_poc
argo_R&amp;D

For demo, I`m going to use official nginx 'hello world' container

In k8s manifest File I create simple deployment with 2 replicas, service (If there is enough time - ingress)


### used command:

minikube start



kubectl get pod
kubectl get svc
kubectl get deploy
kubectl get rs
kubectl apply -f application.yml
kubectl delete -f application.yml
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

kubectl port-forward svc/argocd-server -n argocd 8080:443

kubectl create namespace argo-events
kubectl apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml

kubectl --namespace argo-events apply --filename https://raw.githubusercontent.com/argoproj/argo-events/stable/examples/eventbus/native.yaml

kubectl --namespace argo-events apply --filename event-source.yml
kubectl --namespace argo-events get eventsources
kubectl --namespace argo-events get services
kubectl --namespace argo-events get pods
kubectl --namespace argo-events port-forward svc/webhook-eventsource-svc 12000:12000 &
curl -X POST -H "Content-Type: application/json" -d '{"message":"nginxdemos/hello:latest"}' http://localhost:12000/devops-toolkit

kubectl --namespace argo-events apply -f sensor.yaml


kubectl --namespace argo-events logs \
    --selector app=payload

kubectl --namespace argo-events \
    delete pods \
    --selector app=payload
