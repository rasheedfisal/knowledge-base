# Build the current dockerfile

`docker build -t {username}/{container name} {directory path}`

- -t: tag
- username: docker hub account
- directory path: `.` for current directory

# Run docker container

`docker run -p {extrnal port}:{internal port} -d {username}/{container name}`

- -p: port
- -d: detached

# Show running containers

`docker ps`

# Stop docker container

`docker stop {container id}`

# Start docker container

`docker start {container id}`

# Push docker container to docker hub

###### \*to push to docker from vs code you first had to login to docker using:

`docker login`

###### \*you will be prompt to enter username then password

`docker push {username}/{container name}`

# Kubernetes Commands

## Get Version

`kubectl version`

## Apply/Create the Deployment File

`kubectl apply -f {file name}.yaml`

- -f: file name

## Get the Deployments

`kubectl get deployments`

## Get the Pods

`kubectl get pods`

## delete the Pods

`kubectl delete pod [pod_name]`

## Delete Deployment File

`kubectl delete deployment {deployment name}`

## Get Services

`kubectl get services`

## Force Kubernetes to Re-Deploy Container with Latest Changes

`kubectl rollout restart deployment {deployment name}`

## Get Namespace

`kubectl get namespace`

## Get pod for that Particular Namespace

`kubectl get pods --namespace={namespace name}`

## short hand for namespace example

`kubectl get services -n ingress-nginx`

- -n: namespace

## what is Kubernetes Ingress?

##### is an API object that helps developers expose their applications and manage external access by providing http/s routing rules to the services within a Kubernetes cluster

## Nginx Ingress Controller

1- go to [kubernetes nginx](https://kubernetes.github.io/ingress-nginx/deploy/).
2- copy ingress controller link `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml` note that version may be different

## Debug Nginx Ingress Logs

`kubectl -n ingress-nginx logs -l app.kubernetes.io/instance=ingress-nginx`

## Kubernetes StorageClass
`kubectl get storageclass`
## Get Persistent Volume Claim
`kubectl get pvc`
## Create Secrets in Kubernetes
`kubectl create secret generic [secret name] --from-literal=[secret key]="[secret value]"`
ex: `kubectl create secret generic mssql --from-literal=SA_PASSWORD="pa55w0rd!"`

## TroubleShooting
`kubectl describe pod`
`kubectl describe pod [podname] > [file name].txt`
