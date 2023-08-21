# Build the current dockerfile

`docker build -t {username}/{container name}`

- -t: tag
- username: docker hub account

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

## what is Kubernetes Ingress?

##### is an API object that helps developers expose their applications and manage external access by providing http/s routing rules to the services within a Kubernetes cluster
