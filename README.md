# K8s-rollout

## Target
Show how to role back with kubectl

### Steps
* Create Deployment.yaml for a pod with image of nginx:1.7.9 with 2 replicasets
  * kubectl create deployment my-deploy --image nginx:1.7.9 --replicas=2 --port=80 -o yaml --dry-run > deployment.yaml
  * kubectl apply -f deployment.yaml

At this point 1 the following are created:

```
$ *kubectl get deployments.apps*
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   2/2     2            2           2m16s

$ *kubectl get replicasets.apps* 
NAME                      DESIRED   CURRENT   READY   AGE
nginx-deploy-5d59d67564   2         2         2       2m18s

$ *kubectl get pods*
NAME                            READY   STATUS    RESTARTS   AGE
nginx-deploy-5d59d67564-6cx9p   1/1     Running   0          2m21s
nginx-deploy-5d59d67564-zp9bk   1/1     Running   0          2m21s
$
```

