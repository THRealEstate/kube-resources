# Redis Helm Chart
The deployment of this helm chart begins with the general process of initializing
a helm tiller pod / service. After which the steps are to bind the persistent
volumes to a persistent volume claim. This helm chart binds the redis service to
an external IP that is established via GCP or in minikube can be found via
the `minikube service url`

```bash
> ls
README.md          deployments        persistant-volumes
> helm init
> helm install -n redis-pvs persistant-volumes
> helm install -n redis-pods deployments
> minikube service redis --url
 (HOST):(PORT)
> redis-cli -h HOST -p PORT
```
