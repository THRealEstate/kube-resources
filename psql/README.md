# PSQL Helm Chart

The deployment of this helm chart begins with the general process of initializing a helm tiller pod / service. After which the steps are to bind the persistent volumes to a persistent volume claim. This helm chart binds the PSQL service to an external IP that is established via GCP or in minikube. To access within the cluster, it is can be accessed via port 5432 at the following DNS: `psql-postgresql.default.svc.cluster.local`

```bash
> ls
Chart.yaml  README.md   templates   values.yaml
> helm init
> helm install --name psql \
  --set postgresUser=my-user,postgresPassword=secretpassword,postgresDatabase=thre
```

To access externally you must first enable TCP/IP connection to our unique host.

You will therefore update the pg_hba.conf to include the following line:

```conf
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust
# Our unique host
host    all             all             <OUR_HOST_ADDRESS>      trust
```

At this point in time you may now connect to `psql` with the following:

```bash
> psql -H "192.168.99.100" -p 30231 -d thre -U my-user
Password for user my-user:
psql (9.5.4, server 9.6.2)
Type "help" for help.

thre=#
```
