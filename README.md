# K8s-rollout

## Target
Show how to role back with kubectl

### Steps
* Create Deployment.yaml for a pod with image of nginx:1.7.9 with 2 replicasets
  * kubectl create deployment my-deploy --image nginx:1.7.9 --replicas=2 --port=80 -o yaml --dry-run > deployment.yaml
  * kubectl apply -f deployment.yaml --record

At this point the following are created:

```
$ *kubectl get deployments.apps*
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deploy   2/2     2            2           2m16s

$ *kubectl get replicasets.apps* 
NAME                      DESIRED   CURRENT   READY   AGE
nginx-deploy-5d59d67564   2         2         2       2m18s

$ *kubectl get pods*
NAME                            READY   STATUS    RESTARTS   AGE
nginx-deploy-5d59d67564-2zvpz   1/1     Running   0          2m21s
nginx-deploy-5d59d67564-8pqfz   1/1     Running   0          2m21s
$
```

* Now the image version is changed to nginx:latest
* kubectl apply -f deployment.yaml --record
  The old pods are terminated and the new pods take place.

```
$ kubectl get pod
NAME                            READY   STATUS        RESTARTS   AGE
nginx-deploy-5d59d67564-2zvpz   0/1     Terminating   0          64s
nginx-deploy-5d59d67564-8pqfz   0/1     Terminating   0          61s
nginx-deploy-75b69bd684-6jgp2   1/1     Running       0          11s
nginx-deploy-75b69bd684-mxggs   1/1     Running       0          9s
$
```

* Check rollout history

```
$ kubectl rollout history deployment
deployment.apps/nginx-deploy 
REVISION  CHANGE-CAUSE
1         kubectl apply --filename=deployment.yaml --record=true
2         kubectl apply --filename=deployment.yaml --record=true

$
```

* Now roll back to revision 2

```
$ kubectl rollout undo deployment nginx-deploy --to-revision=2
deployment.apps/nginx-deploy rolled back
$

$ kubectl get pod
NAME                            READY   STATUS              RESTARTS   AGE
nginx-deploy-5d59d67564-22cvb   0/1     ContainerCreating   0          3s
nginx-deploy-5d59d67564-sl5nf   1/1     Running             0          6s
nginx-deploy-75b69bd684-6jgp2   1/1     Running             0          2m59s
nginx-deploy-75b69bd684-mxggs   0/1     Terminating         0          2m57s
$
```

