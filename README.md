# Kubernetes – Pod Deployment and Service Exposure (NGINX)

This repository contains a complete implementation of an NGINX application deployed using Kubernetes Pod and exposed externally through a NodePort Service. All operations were executed on a local Minikube Kubernetes cluster, following standard Kubernetes object specifications.

## 1. Cluster Initialization

Start Kubernetes cluster:

minikube start


Verify node availability:

kubectl get nodes


Expected Output:

Single control-plane node in Ready state.

## 2. Pod Deployment Using YAML
Pod Manifest (pod.yaml)
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx


Apply configuration:

kubectl apply -f pod.yaml


Verify deployment:

kubectl get pods
kubectl get pods -o wide


Pod transitions:

ContainerCreating → Running

A ClusterIP (10.x.x.x) is assigned internally

## 3. Internal Networking Validation

Kubernetes Pod IPs are internal and accessible only from within the cluster.

Access Minikube node:

minikube ssh


From inside:

curl <POD-IP>


Expected:

Default NGINX HTML welcome page.

Exit node:

exit

## 4. Service Creation (NodePort)

A Service provides a stable network endpoint for the Pod.

Expose the Pod:

kubectl expose pod nginx --type=NodePort --port=80


Check service details:

kubectl get svc


Example output:

nginx   NodePort   10.102.93.120   <none>   80:31554/TCP


Meaning:

Service listens internally on port 80

External port assigned = 31554 (random within 30000–32767)

## 5. External Application Access

Open the service externally:

minikube service nginx


This will launch the NGINX welcome page in the browser using the allocated NodePort.

## 6. Kubernetes Pod-to-Service Architecture

Below is the logical architecture of how the Pod and Service interact inside the Kubernetes cluster:

<img width="1024" height="1024" alt="k8s" src="https://github.com/user-attachments/assets/daffa29f-7d63-46ec-bfc3-050efb0d1f52" />


Key Flow:

Browser request hits NodePort (external port 31xxx)

Service maps request to Pod using selector app: nginx

Pod forwards request to container (nginx)

Response returns back via the same path

This is the standard Kubernetes networking flow.

## 7. Operational Commands Used
Pod operations:
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod nginx
kubectl delete pod nginx
kubectl logs nginx

Service operations:
kubectl get svc
kubectl describe svc nginx
kubectl delete svc nginx

Cluster interactions:
minikube ssh
curl <POD-IP>
minikube service nginx

## 8. Important Behaviour Observed

Pod IPs are ephemeral and internal-only

Service ensures stable connectivity independent of Pod restarts

NodePort exposes service outside the cluster

Labels are essential for Service → Pod mapping

Minikube replicates real Kubernetes networking behaviour

## 9. Repository Structure
.
├── pod.yaml
├── service-nodeport.yaml   (optional future)
└── README.md

## 10. Conclusion

This implementation demonstrates the complete workflow of deploying, validating, and exposing a Kubernetes Pod using Minikube. It showcases Pod lifecycle, cluster networking, label-based selection, and NodePort-based external access—reflecting standard Kubernetes operational patterns used in real-world deployments.

