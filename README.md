Pod creation using YAML
- Pod lifecycle (`ContainerCreating → Running`)
- How Pods get an internal ClusterIP
- Why Pod IP is not accessible from outside the cluster
- How to access Pod internally using `minikube ssh`
- Importance of labels in Kubernetes
- Creating NodePort service to expose the Pod

---

## Commands Used

```bash
minikube start
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod nginx
minikube ssh
curl <pod-ip>
kubectl delete pod nginx
kubectl apply -f pod-with-labels.yaml
kubectl expose pod nginx --type=NodePort --port=80
minikube service nginx
