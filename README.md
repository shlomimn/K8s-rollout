# K8s-rollout

## Target
Show how to role back with kubectl

### Steps
* Create Deployment.yaml for a pod with image of nginx:1.7.9 with 2 replicasets
  * kubectl create deployment my-deploy --image nginx:1.7.9 --replicas=2 --port=80 -o yaml --dry-run > deployment.yaml
  * kubectl apply -f deployment.yaml
