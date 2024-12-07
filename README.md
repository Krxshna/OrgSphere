# OrgSphere - HRM based on Microservices Architecture

### Usage
1. Download and run **Minikube** using command: `minikube start --vm-driver=virtualbox --memory='4000mb'` 
2. Build Maven project with using command: `mvn clean install`
3. Build Docker images for each module using command, for example: `docker build -t piomin/employee:1.1 .`
4. Go to `/kubernetes` directory in repository
5. Apply all templates to Minikube using command: `kubectl apply -f <filename>.yaml`
6. Check status with `kubectl get pods`

## Architecture

Our sample microservices-based system consists of the following modules:
- **gateway-service** - a module that Spring Cloud Netflix Zuul for running Spring Boot application that acts as a proxy/gateway in our architecture.
- **employee-service** - a module containing the first of our sample microservices that allows to perform CRUD operation on Mongo repository of employees
- **department-service** - a module containing the second of our sample microservices that allows to perform CRUD operation on Mongo repository of departments. It communicates with employee-service. 
- **organization-service** - a module containing the third of our sample microservices that allows to perform CRUD operation on Mongo repository of organizations. It communicates with both employee-service and department-service.
- **admin-service** - a module containing embedded Spring Boot Admin Server used for monitoring Spring Boot microservices running on Kubernetes
The following picture illustrates the architecture described above including Kubernetes objects.

You can distribute applications across multiple namespaces and use Spring Cloud Kubernetes `DiscoveryClient` and `Ribbon` for inter-service communication.

<img src="https://piotrminkowski.files.wordpress.com/2019/12/microservices-with-spring-cloud-kubernetes-discovery.png" title="Architecture2" >

## Before you start
Go to the `k8s` directory. You will find there several YAML scripts you need to apply before running applications.
1. `privileges.yaml` - `Role` and `RoleBinding` for Spring Cloud Kubernetes to allow access Kubernetes API from pod
2. `mongo-secret.yaml` - credentials for MongoDB
3. `mongo-configmap.yaml` - user for MongoDB
4. `mongo-deployment.yaml` - `Deployment` for MongoDB
Just apply those scripts using `kubectl apply`.

You can easily deploy all applications using `skaffold dev` command.
