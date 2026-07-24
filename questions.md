# Kubernetes Phase 1 Interview Questions & Answers

# 1. What is Kubernetes?

**Answer:**

Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, networking, management, and self-healing of containerized applications across a cluster of machines.

---

# 2. Why do we need Kubernetes?

**Answer:**

Kubernetes is used to efficiently manage multiple containerized applications running across multiple machines. It automates deployment, scaling, load balancing, self-healing, networking, and application management while reducing manual effort.

---

# 3. What is Container Orchestration?

**Answer:**

Container orchestration is the automated management of multiple containerized applications, including their deployment, scaling, networking, load balancing, monitoring, and recovery across one or more machines.

---

# 4. Can Docker replace Kubernetes?

**Answer:**

No.

Docker is responsible for building and running containers.

Kubernetes manages containers at scale by handling deployment, scaling, networking, self-healing, and load balancing.

Docker and Kubernetes solve different problems and are commonly used together.

---

# 5. Name some problems Kubernetes solves.

**Answer:**

- Automatic deployment
- Automatic scaling
- Self-healing
- Load balancing
- Rolling updates
- Rollback
- High availability
- Resource management

---

# 6. What is a Kubernetes Cluster?

**Answer:**

A Kubernetes Cluster is a collection of one or more machines (called Nodes) that work together to deploy, run, manage, and scale containerized applications.

A cluster consists of:

- Control Plane
- One or more Worker Nodes

---

# 7. What are the two major components of Kubernetes Architecture?

**Answer:**

- Control Plane
- Worker Nodes

---

# 8. What is the Control Plane?

**Answer:**

The Control Plane is the brain of Kubernetes.

It receives requests, manages the cluster, schedules workloads, maintains the desired state, monitors the cluster, and instructs Worker Nodes on where and how to run applications.

---

# 9. What is a Worker Node?

**Answer:**

A Worker Node is a machine that executes the workloads assigned by the Control Plane.

It runs:

- Pods
- Containers
- Applications

---

# 10. Where does a FastAPI application run in Kubernetes?

**Answer:**

FastAPI runs inside:

```
Worker Node
    ↓
Pod
    ↓
Container
    ↓
FastAPI Application
```

---

# 11. Does the Control Plane usually run applications?

**Answer:**

No.

The Control Plane manages the cluster and makes decisions.

Applications run on Worker Nodes.

---

# 12. High-Level Kubernetes Deployment Flow

```
You
 │
kubectl apply
 │
 ▼
Control Plane
 │
 ▼
Worker Node
 │
 ▼
Pod
 │
 ▼
Container
 │
 ▼
FastAPI
```

---

# API Server

# 13. What is the Kubernetes API Server?

**Answer:**

The Kubernetes API Server is the central communication hub and front end of the Kubernetes Control Plane.

It receives, validates, authenticates, authorizes, and processes all requests to the cluster while coordinating communication between Kubernetes components.

---

# 14. Why is the API Server called the front door of Kubernetes?

**Answer:**

Because every request to the Kubernetes cluster first passes through the API Server before reaching any other Kubernetes component.

---

# 15. Does every request pass through the API Server?

**Answer:**

Yes.

Every request passes through the API Server because it:

- Receives requests
- Validates requests
- Authenticates users
- Authorizes actions
- Stores desired state
- Coordinates communication

---

# 16. What are the responsibilities of the API Server?

**Answer:**

- Receive requests
- Validate requests
- Authenticate users
- Authorize actions
- Store desired state in etcd
- Coordinate communication between Kubernetes components

---

# 17. Does kubectl communicate directly with Worker Nodes?

**Answer:**

No.

kubectl communicates only with the API Server.

The API Server then communicates with other Kubernetes components.

---

# 18. Authentication vs Authorization

## Authentication

**Answer:**

Authentication verifies the identity of the user by checking credentials such as certificates, tokens, usernames, or passwords.

**Memory Trick**

Who are you?

---

## Authorization

**Answer:**

Authorization determines what actions an authenticated user is allowed to perform.

**Memory Trick**

What are you allowed to do?

---

# 19. Status Codes

| Status Code | Meaning |
|-------------|---------|
| 401 | Authentication failed |
| 403 | Authenticated but not authorized |

---

# Scheduler

# 20. What is the Kubernetes Scheduler?

**Answer:**

The Kubernetes Scheduler is a Control Plane component responsible for selecting the most appropriate Worker Node for newly created Pods based on available resources and scheduling constraints.

---

# 21. Does the Scheduler schedule Pods or Containers?

**Answer:**

The Scheduler schedules Pods.

Containers are created inside Pods after scheduling.

---

# 22. Does the Scheduler create Pods?

**Answer:**

No.

The Scheduler only selects the best Worker Node where the Pod should run.

Another Kubernetes component ensures the Pod is created.

---

# 23. What factors does the Scheduler consider?

**Answer:**

- Available CPU
- Available Memory
- Resource Requests
- Node Constraints
- Scheduling Rules

---

# 24. Does the Scheduler continuously monitor Pods?

**Answer:**

No.

The Scheduler mainly works when a new Pod needs placement.

Once a Pod is assigned to a Worker Node, its scheduling task is complete.

---

# 25. Why do we need the Scheduler?

**Answer:**

The Scheduler distributes Pods efficiently across Worker Nodes based on available resources and scheduling constraints.

This prevents resource congestion and improves cluster performance.

---

# Controller Manager

# 26. What is the Controller Manager?

**Answer:**

The Kubernetes Controller Manager is a Control Plane component that continuously monitors the cluster and ensures that the actual state matches the desired state by creating, updating, or recreating resources whenever necessary.

---

# 27. What is Desired State?

**Answer:**

Desired State is the configuration declared by the user in Kubernetes manifests (YAML), such as:

- Number of replicas
- Resource limits
- Application configuration

Example:

```yaml
replicas: 3
```

---

# 28. What is Actual State?

**Answer:**

Actual State is the real condition of the cluster at any given time.

Example:

Desired:

```
3 Pods
```

Actual:

```
2 Pods
```

---

# 29. What happens if one Pod crashes?

**Answer:**

The Controller Manager detects that the Actual State no longer matches the Desired State and automatically recreates the missing Pod.

---

# 30. Why do we need the Controller Manager?

**Answer:**

The Controller Manager provides self-healing by continuously monitoring the cluster and restoring the desired state, ensuring high availability and minimizing downtime.

---

# 31. Does the Controller Manager continuously monitor the cluster?

**Answer:**

Yes.

Unlike the Scheduler, the Controller Manager continuously watches the cluster for state changes.

---

# 32. Scheduler vs Controller Manager

| Scheduler | Controller Manager |
|------------|--------------------|
| Chooses the Worker Node | Ensures desired state |
| Works mainly for new Pods | Continuously monitors cluster |
| Decides where Pods run | Decides whether resources should exist |

---

# etcd

# 33. What is etcd?

**Answer:**

etcd is a distributed, highly available key-value database used by Kubernetes to store the entire cluster state and configuration.

It acts as the source of truth for the Kubernetes cluster.

---

# 34. Why is etcd called the Source of Truth?

**Answer:**

Because it stores the desired state and configuration of the Kubernetes cluster.

All Kubernetes components rely on etcd to determine the current and desired state.

---

# 35. What type of database is etcd?

**Answer:**

A distributed key-value database.

---

# 36. What is stored in etcd?

**Answer:**

- Pods
- Deployments
- Services
- ConfigMaps
- Secrets (encrypted)
- Replica Count
- Node Information
- Cluster Configuration

---

# 37. Do Scheduler and Controller Manager communicate directly with etcd?

**Answer:**

No.

They communicate with the API Server.

The API Server is the only component that directly interacts with etcd.

---

# 38. Why is etcd distributed?

**Answer:**

etcd is distributed to provide:

- Fault tolerance
- High availability
- Reduced downtime
- Data redundancy
- Better reliability

---

# 39. Complete Kubernetes Deployment Flow

```
You
 │
kubectl apply
 │
 ▼
API Server
 │
Receives Request
Validates
Authenticates
Authorizes
 │
 ▼
Stores Desired State
 │
 ▼
etcd
 │
 ▼
Scheduler
 │
Chooses Worker Node
 │
 ▼
Controller Manager
 │
Ensures Desired State
 │
 ▼
Worker Node
 │
 ▼
Pod
 │
 ▼
Container
 │
 ▼
FastAPI Application
```

---

# 40. Easy Memory Tricks

## Kubernetes

Docker → Runs Containers

Kubernetes → Manages Containers

---

## Architecture

Control Plane → Brain

Worker Node → Hands

---

## API Server

Everything

↓

API Server

↓

Everything Else

---

## Scheduler

Where should the Pod run?

---

## Controller Manager

Should the Pod exist?

---

## etcd

Application

↓

PostgreSQL

--------------------

Kubernetes

↓

etcd

# Kubernetes Phase 2 Interview Questions & Answers
## Topics Covered
- kubelet
- Pods
- ReplicaSets
- Deployments
- Services
- Ingress

---

# kubelet

## 1. What is kubelet?

**Answer:**

kubelet is an agent that runs on every Worker Node. It communicates with the API Server, manages Pods assigned to its node, monitors them, and reports the node's status back to the Control Plane.

---

## 2. Where does kubelet run?

**Answer:**

kubelet runs on every Worker Node.

---

## 3. Does every Worker Node have a kubelet?

**Answer:**

Yes. Every Worker Node runs one kubelet.

---

## 4. What are the responsibilities of kubelet?

**Answer:**

- Receives Pod assignments
- Creates and manages Pods
- Monitors Pods and containers
- Reports node and Pod status back to the API Server

---

## 5. Does kubelet decide where Pods run?

**Answer:**

No.

The Scheduler selects the best Worker Node.

kubelet only manages Pods assigned to its Worker Node.

---

## 6. Does kubelet communicate directly with etcd?

**Answer:**

No.

kubelet communicates only with the API Server.

The API Server communicates with etcd.

---

## 7. Difference between Scheduler and kubelet

| Scheduler | kubelet |
|------------|----------|
| Runs in Control Plane | Runs on Worker Node |
| Chooses Worker Node | Creates and manages Pods |
| Works during scheduling | Continuously monitors Pods |

---

# Pods

## 8. What is a Pod?

**Answer:**

A Pod is the smallest deployable unit in Kubernetes that encapsulates one or more containers sharing the same network, storage, and lifecycle.

---

## 9. Why doesn't Kubernetes deploy containers directly?

**Answer:**

Managing networking, storage, scaling, communication, and lifecycle for individual containers would be complex.

Kubernetes wraps containers inside Pods to simplify management.

---

## 10. What is the smallest deployable unit in Kubernetes?

**Answer:**

Pod.

---

## 11. Can a Pod contain multiple containers?

**Answer:**

Yes.

Example:

- FastAPI Container
- Logging Container (Sidecar)

---

## 12. What resources are shared inside a Pod?

**Answer:**

- IP Address
- Port Space
- Storage (Volumes)
- Lifecycle

---

## 13. What does "ephemeral" mean?

**Answer:**

Pods are temporary.

If a Pod crashes or is deleted, Kubernetes creates a new Pod instead of repairing the old one.

---

## 14. If a Pod crashes, what happens?

**Answer:**

The Controller Manager detects the mismatch between the desired and actual state.

The ReplicaSet creates a new Pod.

---

## 15. Can Kubernetes run a container without a Pod?

**Answer:**

No.

Kubernetes always runs containers inside Pods.

---

## 16. Who owns the IP address?

**Answer:**

The Pod owns the IP address.

Containers inside the Pod share the same IP.

---

# ReplicaSets

## 17. What is a ReplicaSet?

**Answer:**

A ReplicaSet is a Kubernetes object that maintains a specified number of identical Pod replicas by automatically creating or deleting Pods whenever necessary.

---

## 18. What does a ReplicaSet manage?

**Answer:**

ReplicaSets manage Pods.

---

## 19. If replicas = 5 and one Pod crashes, what happens?

**Answer:**

ReplicaSet detects only four Pods are running and automatically creates one new Pod.

---

## 20. If replicas = 3 but four Pods exist, what happens?

**Answer:**

ReplicaSet deletes one Pod to match the desired number.

---

## 21. Does ReplicaSet perform rolling updates?

**Answer:**

No.

Rolling Updates are handled by Deployments.

---

## 22. Why do we need ReplicaSets?

**Answer:**

- Maintain desired number of Pods
- Fault tolerance
- High availability
- Automatically replace failed Pods

---

## 23. Difference between Pod and ReplicaSet

| Pod | ReplicaSet |
|------|------------|
| Smallest deployable unit | Kubernetes object |
| Runs containers | Maintains desired number of Pods |

---

## 24. Can a ReplicaSet exist with zero Pods?

**Answer:**

Yes.

Example:

```yaml
replicas: 0
```

---

## 25. Should we create ReplicaSets directly?

**Answer:**

Technically yes.

Practically no.

Deployments create and manage ReplicaSets automatically.

---

# Deployments

## 26. What is a Deployment?

**Answer:**

A Deployment is a Kubernetes object that manages ReplicaSets and provides declarative updates through rolling updates, rollbacks, scaling, and version management.

---

## 27. Does Deployment manage Pods directly?

**Answer:**

No.

Deployment manages ReplicaSets.

ReplicaSets manage Pods.

---

## 28. Explain the Deployment hierarchy.

```text
Deployment
      ↓
ReplicaSet
      ↓
Pod
      ↓
Container
```

---

## 29. What is a Rolling Update?

**Answer:**

A Rolling Update gradually replaces old Pods with new Pods without causing downtime.

---

## 30. What is a Rollback?

**Answer:**

Rollback restores the application to a previous stable version if the new deployment fails.

---

## 31. Why do we use Deployments instead of ReplicaSets?

**Answer:**

Deployments provide:

- Rolling Updates
- Rollbacks
- Version History
- Scaling
- ReplicaSet Management

---

## 32. Suppose Version 2 has bugs. What should you do?

**Answer:**

Rollback to the previous stable version using the Deployment.

---

## 33. Does Deployment create Pods?

**Answer:**

Indirectly.

Deployment creates ReplicaSets.

ReplicaSets create Pods.

---

## 34. Can a Deployment exist without a ReplicaSet?

**Answer:**

No.

Deployments always manage one or more ReplicaSets.

---

# Services

## 35. What is a Service?

**Answer:**

A Service is a Kubernetes object that provides a stable network endpoint and load balances traffic to a group of Pods.

---

## 36. Why do we need Services?

**Answer:**

Pods are ephemeral and their IP addresses change.

Services provide:

- Stable IP
- Stable DNS
- Load balancing
- Reliable communication

---

## 37. Why can't users communicate directly with Pods?

**Answer:**

Pods are temporary.

If they are recreated, their IP addresses change.

Users communicate with Services instead.

---

## 38. Responsibilities of Services

**Answer:**

- Stable endpoint
- Stable DNS
- Load balancing
- Service discovery
- Access to Pods

---

## 39. What is Load Balancing?

**Answer:**

Load balancing distributes incoming requests across multiple healthy Pods.

---

## 40. Name the four Service types.

**Answer:**

- ClusterIP
- NodePort
- LoadBalancer
- ExternalName

---

## 41. Which Service type is the default?

**Answer:**

ClusterIP.

---

## 42. Which Service type is commonly used in cloud providers?

**Answer:**

LoadBalancer.

---

## 43. Does a Service create Pods?

**Answer:**

No.

A Service only routes traffic to Pods.

---

## 44. Does a Service have its own IP?

**Answer:**

Yes.

A Service has a stable virtual IP (ClusterIP).

---

# Ingress

## 45. What is an Ingress?

**Answer:**

Ingress is a Kubernetes object that manages external HTTP/HTTPS access to Services by routing traffic based on hostnames and URL paths through a single entry point.

---

## 46. What does Ingress route traffic to?

**Answer:**

Ingress routes traffic to Services.

---

## 47. Why doesn't Ingress communicate directly with Pods?

**Answer:**

Pods are ephemeral.

Services provide stable endpoints.

Ingress routes traffic to Services.

---

## 48. What is path-based routing?

**Answer:**

Routing requests based on URL paths.

Example:

```
company.com/ → Frontend

company.com/api → Backend

company.com/admin → Admin
```

---

## 49. What is host-based routing?

**Answer:**

Routing requests based on hostnames.

Example:

```
api.company.com → API

admin.company.com → Admin

shop.company.com → Shopping App
```

---

## 50. Why do we need Ingress?

**Answer:**

- External HTTP/HTTPS access
- URL routing
- Hostname routing
- HTTPS termination
- Reverse proxy
- Single public entry point

---

## 51. Difference between Service and Ingress

| Service | Ingress |
|----------|----------|
| Connects to Pods | Connects to Services |
| Stable endpoint | Routes external traffic |
| Load balances Pods | Routes requests |
| Internal or external networking | HTTP/HTTPS entry point |

---

## 52. Can one Ingress expose multiple Services?

**Answer:**

Yes.

Ingress routes traffic to multiple Services using:

- URL paths
- Hostnames

---

## 53. Does Ingress work by itself?

**Answer:**

No.

An Ingress Controller (such as NGINX Ingress Controller or Traefik) is required to implement the routing rules.

---

# Complete Kubernetes Flow

```text
Internet
     │
     ▼
Ingress
     │
     ▼
Service
     │
     ▼
Deployment
     │
     ▼
ReplicaSet
     │
     ▼
Pod
     │
     ▼
Container
     │
     ▼
Application

----------------------------

API Server
     │
     ├── Scheduler
     ├── Controller Manager
     └── etcd
              │
              ▼
          kubelet
              │
              ▼
         Worker Node
```

# Easy Memory Tricks

```
Deployment
    ↓
Manages Versions

ReplicaSet
    ↓
Manages Pods

Pod
    ↓
Runs Containers

Service
    ↓
Provides Stable Networking

Ingress
    ↓
Routes External Traffic

Scheduler
    ↓
Where?

kubelet
    ↓
Run it.

Controller Manager
    ↓
Keep it healthy.
```
# Kubernetes Phase 3 Interview Questions & Answers
## Topics Covered
- ConfigMaps
- Secrets

---

# ConfigMaps

## 1. What is a ConfigMap?

**Answer:**

A ConfigMap is a Kubernetes object used to store non-sensitive configuration data as key-value pairs, allowing applications to separate configuration from application code.

---

## 2. Why do we need ConfigMaps?

**Answer:**

ConfigMaps separate configuration from application code so the same application image can be used across development, testing, and production without rebuilding Docker images.

---

## 3. What problem do ConfigMaps solve?

**Answer:**

Without ConfigMaps, every configuration change requires modifying the application code, rebuilding the Docker image, and redeploying the application.

ConfigMaps eliminate this by storing configuration separately.

---

## 4. What type of data should be stored in ConfigMaps?

**Answer:**

Only non-sensitive configuration data.

Examples:

- Database Host
- Database Port
- API URL
- Debug Mode
- Model Name
- Feature Flags
- Log Level
- Environment Variables

---

## 5. Can passwords be stored in ConfigMaps?

**Answer:**

No.

Passwords, API Keys, JWT Secrets, Tokens, and certificates should never be stored inside ConfigMaps because ConfigMaps are stored as plain text.

---

## 6. Are ConfigMaps encrypted?

**Answer:**

No.

ConfigMaps are stored as plain text.

---

## 7. What does "separating configuration from code" mean?

**Answer:**

Application logic remains unchanged while environment-specific configuration is stored in Kubernetes ConfigMaps.

This allows the same application image to work in development, testing, and production using different configurations.

---

## 8. Why is separating configuration from code useful?

**Answer:**

- No image rebuild
- Easier maintenance
- Better portability
- Environment-specific configuration
- Simpler deployments

---

## 9. Can multiple Pods use the same ConfigMap?

**Answer:**

Yes.

A single ConfigMap can be shared by multiple Pods.

---

## 10. Can ConfigMaps be updated?

**Answer:**

Yes.

Configurations can be updated without changing application code or rebuilding Docker images.

---

## 11. Give five examples of ConfigMap values.

**Answer:**

- DATABASE_HOST
- DATABASE_PORT
- DEBUG
- MODEL_NAME
- LOG_LEVEL
- API_URL
- FEATURE_FLAG

---

## 12. What are the responsibilities of ConfigMaps?

**Answer:**

- Store configuration
- Separate configuration from code
- Share configuration across Pods
- Environment-specific settings
- Simplify deployments

---

## 13. Does ConfigMap contain application logic?

**Answer:**

No.

It stores only configuration.

---

## 14. Does ConfigMap store sensitive information?

**Answer:**

No.

Sensitive information belongs in Kubernetes Secrets.

---

## 15. Can the same Docker image use different ConfigMaps?

**Answer:**

Yes.

That is one of the main advantages of ConfigMaps.

---

## 16. Difference between application code and ConfigMap.

| Application Code | ConfigMap |
|------------------|-----------|
| Business logic | Configuration |
| Same across environments | Changes across environments |
| Stored inside image | Stored in Kubernetes |

---

# Secrets

## 17. What is a Secret?

**Answer:**

A Secret is a Kubernetes object used to securely store and manage sensitive information such as passwords, API keys, JWT secrets, OAuth tokens, certificates, and credentials.

---

## 18. Why do we need Secrets?

**Answer:**

Secrets separate sensitive information from application code and ConfigMaps, improving security and simplifying credential management.

---

## 19. What problem do Secrets solve?

**Answer:**

Secrets prevent sensitive information from being hardcoded into source code, Docker images, or ConfigMaps.

---

## 20. What type of information belongs in Secrets?

**Answer:**

Examples:

- Database Username
- Database Password
- API Keys
- JWT Secrets
- OAuth Tokens
- TLS Certificates
- SSH Keys
- Cloud Credentials

---

## 21. Can passwords be stored in ConfigMaps?

**Answer:**

No.

Passwords should always be stored inside Kubernetes Secrets.

---

## 22. Are Secrets encrypted by default?

**Answer:**

No.

Secrets are Base64 encoded by default.

Encryption at rest must be enabled separately.

---

## 23. What is Base64 encoding?

**Answer:**

Base64 is an encoding mechanism used to convert binary data into text.

It is **not** a security mechanism.

---

## 24. What is encryption?

**Answer:**

Encryption protects data using cryptographic algorithms and requires a decryption key.

Unlike Base64 encoding, encryption provides security.

---

## 25. Difference between Base64 encoding and encryption.

| Base64 Encoding | Encryption |
|-----------------|------------|
| Encoding | Security |
| Easily decoded | Requires decryption key |
| No protection | Protects data |

---

## 26. How can applications consume Secrets?

**Answer:**

Applications can consume Secrets as:

- Environment Variables
- Mounted Files (Volumes)

---

## 27. Why should passwords never be hardcoded?

**Answer:**

Because they become visible in:

- Source code
- Git repositories
- Docker images

This creates major security risks.

---

## 28. What are the responsibilities of Secrets?

**Answer:**

- Store sensitive data
- Separate secrets from application code
- Improve security
- Centralize credential management
- Simplify secret updates

---

## 29. Can multiple Pods use the same Secret?

**Answer:**

Yes.

Multiple Pods can use the same Secret.

---

## 30. Can Secrets be updated?

**Answer:**

Yes.

Secrets can be updated without changing application code.

---

## 31. Give five examples of Secret values.

**Answer:**

- DB_PASSWORD
- DB_USERNAME
- JWT_SECRET
- OPENAI_API_KEY
- AWS_ACCESS_KEY
- TLS Certificate
- OAuth Token

---

## 32. Difference between ConfigMap and Secret.

| ConfigMap | Secret |
|------------|--------|
| Non-sensitive data | Sensitive data |
| Plain text | Base64 encoded by default |
| Database host | Database password |
| Debug mode | JWT Secret |
| API URL | API Key |

---

## 33. Should API Keys be stored in Docker images?

**Answer:**

No.

They should be stored in Kubernetes Secrets.

---

## 34. Can Kubernetes decode a Secret?

**Answer:**

Yes.

Because Secrets are Base64 encoded, not encrypted.

---

## 35. Which should you use?

### Database Host

**ConfigMap**

---

### Database Password

**Secret**

---

### Debug Mode

**ConfigMap**

---

### JWT Secret

**Secret**

---

### API URL

**ConfigMap**

---

### API Key

**Secret**

---

# ConfigMap vs Secret Summary

| Feature | ConfigMap | Secret |
|----------|-----------|--------|
| Stores | Non-sensitive data | Sensitive data |
| Default Storage | Plain text | Base64 encoded |
| Passwords | ❌ No | ✅ Yes |
| API Keys | ❌ No | ✅ Yes |
| JWT Secret | ❌ No | ✅ Yes |
| Database Host | ✅ Yes | ❌ No |
| Debug Mode | ✅ Yes | ❌ No |

---

# Easy Memory Trick

```
Application
        │
        ▼

Logic
        │
────────┼────────

ConfigMap
        │
Public Configuration

────────┼────────

Secret
        │
Private Configuration
```
# Kubernetes Phase 4 Interview Questions & Answers
## Topics Covered
- Persistent Volumes (PV)
- Persistent Volume Claims (PVC)

---

# Persistent Volumes (PV)

## 1. What is a Persistent Volume (PV)?

**Answer:**

A Persistent Volume (PV) is a Kubernetes storage resource that provides persistent storage independent of the Pod lifecycle, allowing data to survive Pod recreation.

---

## 2. Why do we need Persistent Volumes?

**Answer:**

Pods are ephemeral.

If a Pod crashes or is recreated, its local storage is lost.

Persistent Volumes allow important application data to survive Pod failures.

---

## 3. What problem do Persistent Volumes solve?

**Answer:**

Persistent Volumes solve the problem of data loss caused by the temporary nature of Pods.

---

## 4. What happens to data stored inside a Pod if the Pod crashes?

**Answer:**

Data stored inside the Pod's local filesystem is lost because Pods are ephemeral.

---

## 5. Does a Persistent Volume disappear when a Pod is deleted?

**Answer:**

No.

Persistent Volumes are independent of the Pod lifecycle.

Deleting a Pod does not delete the Persistent Volume.

---

## 6. Who usually creates Persistent Volumes?

**Answer:**

Usually:

- Cluster Administrator
- Storage Administrator
- Cloud Provider (AWS, Azure, GCP)

---

## 7. Give five examples of data stored inside a Persistent Volume.

**Answer:**

- Database files
- ML Models
- Uploaded Images
- Videos
- Logs
- User Documents
- Backups

---

## 8. Can a Pod exist without a Persistent Volume?

**Answer:**

Yes.

Applications that do not need persistent storage can run without a PV.

---

## 9. Does a Persistent Volume belong to a Pod?

**Answer:**

No.

A Persistent Volume exists independently of Pods.

---

## 10. Why is a Persistent Volume called "Persistent"?

**Answer:**

Because the storage remains even if Pods are deleted or recreated.

---

## 11. Does a Persistent Volume store application data?

**Answer:**

Yes.

Persistent Volumes store the actual application data.

---

## 12. Can multiple Pods use a Persistent Volume?

**Answer:**

Yes.

Depending on the supported access mode.

---

## 13. Responsibilities of Persistent Volumes.

**Answer:**

- Provide persistent storage
- Store application data
- Survive Pod recreation
- Independent of Pod lifecycle
- Support reusable storage

---

## 14. Examples of applications requiring PVs.

**Answer:**

- PostgreSQL
- MySQL
- MongoDB
- File Storage
- ML Model Storage
- Logging Systems

---

## 15. Difference between Pod storage and Persistent Volume.

| Pod Storage | Persistent Volume |
|-------------|------------------|
| Temporary | Persistent |
| Lost when Pod dies | Survives Pod recreation |
| Inside Pod | Independent resource |

---

# Persistent Volume Claims (PVC)

## 16. What is a Persistent Volume Claim (PVC)?

**Answer:**

A Persistent Volume Claim (PVC) is a Kubernetes object that requests persistent storage from a Persistent Volume based on storage requirements such as capacity and access mode.

---

## 17. Why do we need PVC?

**Answer:**

PVC allows applications to request storage without knowing the actual storage implementation.

---

## 18. What problem do PVCs solve?

**Answer:**

PVC separates storage requests from storage implementation.

Applications simply request storage, and Kubernetes finds a matching Persistent Volume.

---

## 19. Does a PVC store data?

**Answer:**

No.

Persistent Volumes store data.

PVC only requests storage.

---

## 20. Does a Pod communicate directly with a Persistent Volume?

**Answer:**

No.

Flow:

```text
Pod
 ↓
PVC
 ↓
PV
```

---

## 21. What information does a PVC request?

**Answer:**

Typically:

- Storage Size
- Access Mode

---

## 22. Who usually creates PVCs?

**Answer:**

Application Developers or DevOps Engineers create PVCs inside Kubernetes manifests.

---

## 23. Who usually creates Persistent Volumes?

**Answer:**

Usually:

- Cluster Administrator
- Cloud Provider
- Storage Administrator

---

## 24. Can a PVC create a Persistent Volume?

**Answer:**

Normally no.

However, when Dynamic Provisioning is enabled through a StorageClass, creating a PVC can automatically provision a PV.

---

## 25. Difference between PV and PVC.

| PV | PVC |
|----|-----|
| Actual storage | Storage request |
| Stores data | Requests data storage |
| Created by admin/cloud | Created by developer |

---

## 26. Can one PVC use multiple Persistent Volumes?

**Answer:**

No.

A PVC binds to one suitable Persistent Volume.

---

## 27. Can multiple Pods use the same PVC?

**Answer:**

Yes.

Depending on the access mode supported by the Persistent Volume.

---

## 28. What happens if no matching Persistent Volume exists?

**Answer:**

The PVC remains in the Pending state until a suitable Persistent Volume becomes available.

---

## 29. What are the responsibilities of PVC?

**Answer:**

- Request storage
- Specify storage requirements
- Specify access mode
- Connect Pods to Persistent Volumes

---

## 30. Explain the complete storage flow.

```text
Application
      ↓
Pod
      ↓
Persistent Volume Claim (PVC)
      ↓
Persistent Volume (PV)
      ↓
Physical Storage
```

---

## 31. Explain the hotel analogy.

**Answer:**

Persistent Volume = Hotel Room

Persistent Volume Claim = Reservation

Pod = Customer

The customer does not directly choose a room.

Instead, the reservation system assigns a suitable room.

---

## 32. Why don't Pods directly use Persistent Volumes?

**Answer:**

Because Kubernetes abstracts storage using PVCs.

Applications only specify their storage requirements.

Kubernetes manages the actual storage allocation.

---

## 33. Can a Persistent Volume exist without a PVC?

**Answer:**

Yes.

A Persistent Volume can exist even if no application is currently using it.

---

## 34. Can a PVC exist without a Pod?

**Answer:**

Yes.

A PVC can exist before any Pod starts using it.

---

## 35. Does deleting a PVC always delete the Persistent Volume?

**Answer:**

Not necessarily.

It depends on the Persistent Volume's reclaim policy (Retain, Delete, etc.).

---

# PV vs PVC Summary

| Feature | PV | PVC |
|----------|----|-----|
| Purpose | Storage Resource | Storage Request |
| Stores Data | ✅ Yes | ❌ No |
| Created By | Admin / Cloud Provider | Developer |
| Independent | Yes | No |
| Used By | PVC | Pod |

---

# Easy Memory Trick

```
Persistent Volume
        ↓
Actual Storage

---------------------

Persistent Volume Claim
        ↓
Storage Request

---------------------

Pod
        ↓
Uses PVC
```

---

# Most Asked Interview Questions

### Does a Pod directly use a Persistent Volume?

**No.**

It uses a PVC.

---

### Does a PVC store data?

**No.**

The Persistent Volume stores the data.

---

### Who creates Persistent Volumes?

- Cluster Administrator
- Storage Administrator
- Cloud Provider

---

### Who creates PVCs?

Application Developers or DevOps Engineers.

---

### Why do we need Persistent Volumes?

To prevent data loss caused by Pod recreation.

---

### What happens if a Pod crashes?

The Pod is recreated.

The Persistent Volume remains.

The application can continue using the same data.
---
# Kubernetes Phase 5 Interview Questions & Answers
## Topics Covered
- Jobs
- CronJobs

---

# Jobs

## 1. What is a Job?

**Answer:**

A Job is a Kubernetes workload object that creates one or more Pods and ensures they successfully complete a one-time task.

---

## 2. Why do we need Jobs?

**Answer:**

Jobs are used to execute one-time tasks that should finish successfully instead of running continuously like a Deployment.

Examples include:

- Model training
- Batch inference
- Database migration
- Database backup
- Report generation
- Data preprocessing

---

## 3. What problem do Jobs solve?

**Answer:**

Jobs solve the problem of executing one-time tasks that should terminate after successful completion instead of running forever.

---

## 4. What happens after a Job completes successfully?

**Answer:**

The Job is marked as **Completed**, and the Pod exits after finishing the assigned task.

Unlike a Deployment, Kubernetes does not keep the Pod running.

---

## 5. What happens if a Job fails?

**Answer:**

The Job automatically creates another Pod and retries the task until it completes successfully (subject to the configured retry limit).

---

## 6. Does a Job run continuously?

**Answer:**

No.

A Job runs only until the assigned task completes successfully.

---

## 7. Does a Job create Pods?

**Answer:**

Yes.

A Job creates one or more Pods to execute the task.

---

## 8. Give five real-world use cases of Jobs.

**Answer:**

- ML model training
- Batch inference
- Database migration
- Database backup
- Report generation
- Data preprocessing
- File conversion

---

## 9. Can a Job create multiple Pods?

**Answer:**

Yes.

Depending on its configuration (parallelism and completions), a Job can create multiple Pods.

---

## 10. Does a Job restart after successful completion?

**Answer:**

No.

Once the Job completes successfully, it is marked as completed.

---

## 11. What happens if the Pod created by a Job crashes?

**Answer:**

The Job creates another Pod and retries the task until it succeeds or reaches its retry limit.

---

## 12. Difference between Deployment and Job.

| Deployment | Job |
|------------|-----|
| Runs continuously | Runs until completion |
| Long-running applications | One-time tasks |
| Maintains Pods | Stops after success |
| Web applications | Batch processing |

---

## 13. When should you use a Job instead of a Deployment?

**Answer:**

Whenever the workload should execute once and terminate.

Examples:

- Model training
- Database migration
- Data preprocessing
- Database backup

---

## 14. Can a Job be used for a FastAPI application?

**Answer:**

No.

FastAPI is a continuously running application and should use a Deployment.

---

## 15. Can a Job run forever?

**Answer:**

No.

If continuous execution is required, use a Deployment.

---

# CronJobs

## 16. What is a CronJob?

**Answer:**

A CronJob is a Kubernetes workload object that automatically creates Jobs according to a defined schedule using cron expressions.

---

## 17. Why do we need CronJobs?

**Answer:**

CronJobs automate recurring tasks so they do not need to be executed manually.

---

## 18. What problem do CronJobs solve?

**Answer:**

CronJobs automate periodic workloads such as backups, retraining, report generation, and maintenance tasks.

---

## 19. Does a CronJob create Pods directly?

**Answer:**

No.

Flow:

```text
CronJob
    ↓
Job
    ↓
Pod
    ↓
Container
    ↓
Task
```

---

## 20. Does a CronJob execute the task itself?

**Answer:**

No.

It creates a Job, and the Job executes the task.

---

## 21. What is a cron expression?

**Answer:**

A cron expression defines the schedule on which a CronJob executes.

Example:

```text
0 0 * * *
```

Meaning:

Every day at **12:00 AM**

---

## 22. Give five real-world use cases of CronJobs.

**Answer:**

- Nightly database backups
- Weekly ML model retraining
- Monthly billing reports
- Daily data preprocessing
- Cache cleanup
- Log cleanup
- Scheduled emails
- Database synchronization

---

## 23. Difference between Job and CronJob.

| Job | CronJob |
|------|----------|
| Runs once | Runs on a schedule |
| Manual execution | Automatic execution |
| One-time task | Recurring task |

---

## 24. Can a CronJob exist without a Job?

**Answer:**

Yes.

A CronJob is a scheduler.

Whenever the scheduled time arrives, it creates a Job.

---

## 25. Does a CronJob continue existing after the Job finishes?

**Answer:**

Yes.

The CronJob remains active because it is responsible for scheduling future Jobs.

---

## 26. Can a CronJob be used for one-time execution?

**Answer:**

Technically yes, but it is not recommended.

For one-time execution, use a Job.

---

## 27. Give one example where you would use a CronJob instead of a Job.

**Answer:**

Weekly machine learning model retraining because it must execute automatically at regular intervals.

---

## 28. What are the responsibilities of a CronJob?

**Answer:**

- Schedule workloads
- Automatically create Jobs
- Automate recurring tasks
- Reduce manual intervention

---

## 29. Can a CronJob create multiple Jobs?

**Answer:**

Yes.

Every time the schedule is triggered, the CronJob creates a new Job.

---

## 30. What happens if a scheduled CronJob execution fails?

**Answer:**

The CronJob creates a Job.

If that Job fails, the Job handles retries according to its retry policy.

---

# Job vs CronJob Summary

| Feature | Job | CronJob |
|----------|------|----------|
| Purpose | One-time execution | Scheduled execution |
| Runs | Once | Repeatedly |
| Trigger | Manual | Automatic |
| Creates Pods | Yes | Indirectly (through Jobs) |
| Best Use | Batch processing | Periodic automation |

---

# Complete Workload Flow

```text
CronJob
      │
      ▼
Job
      │
      ▼
Pod
      │
      ▼
Container
      │
      ▼
Task Completed
```

---

# Easy Memory Trick

```
Deployment
      ↓
Always Running

------------------------

Job
      ↓
Run Once

------------------------

CronJob
      ↓
Run on Schedule
```

---

# Most Asked Interview Questions

### Does a Job create Pods?

**Yes.**

---

### Does a Job run continuously?

**No.**

---

### What happens after a Job succeeds?

The Job is marked as completed, and the Pod exits.

---

### Does a CronJob create Pods directly?

**No.**

CronJob → Job → Pod

---

### What does a CronJob create?

A Job.

---

### Which object is used for nightly database backups?

**CronJob**

---

### Which object is used for one-time database migration?

**Job**

---

### Which object is used for weekly ML model retraining?

**CronJob**

---

### Which object is used for one-time ML model training?

**Job**

---

### Which object should be used for a FastAPI application?

**Deployment**

---

### One-Line Revision

- **Deployment → Run forever**
- **Job → Run once**
- **CronJob → Run on schedule**
---
# Kubernetes Phase 6 & 7 Interview Questions & Answers

# Phase 6 — Resource Management

Topics Covered

- CPU Requests
- Memory Requests
- CPU Limits
- Memory Limits
- CPU Throttling
- OOMKilled

---

# CPU Requests

## 1. What is a CPU Request?

**Answer:**

A CPU Request is the minimum amount of CPU guaranteed to a Pod by Kubernetes. The Scheduler uses it to determine which worker node can run the Pod.

---

## 2. Why do we need CPU Requests?

**Answer:**

CPU Requests ensure that Pods are scheduled only on worker nodes with sufficient CPU resources, preventing resource over-allocation and guaranteeing minimum CPU for applications.

---

## 3. Which Kubernetes component uses CPU Requests?

**Answer:**

The **Scheduler**.

---

## 4. What happens if sufficient CPU is unavailable?

**Answer:**

The Pod remains in the **Pending** state until a worker node with enough available requested CPU becomes available.

---

## 5. Is CPU Request the maximum CPU a Pod can use?

**Answer:**

No.

It is the **minimum guaranteed CPU**.

---

## 6. What problems do CPU Requests solve?

**Answer:**

- Guarantee minimum CPU
- Help Scheduler place Pods
- Prevent over-allocation
- Improve resource planning

---

## 7. Give one real-world example.

**Answer:**

Suppose PostgreSQL requires at least **2 CPUs**.

We configure:

```
CPU Request = 2
```

Kubernetes guarantees that the Pod will only be scheduled on a worker node with at least 2 CPUs available.

---

# Memory Requests

## 8. What is a Memory Request?

**Answer:**

A Memory Request is the minimum amount of memory guaranteed to a Pod by Kubernetes. The Scheduler uses it while scheduling Pods.

---

## 9. Why do we need Memory Requests?

**Answer:**

Applications require different amounts of memory. Memory Requests ensure Pods are scheduled only on nodes with enough available memory.

---

## 10. Which Kubernetes component uses Memory Requests?

**Answer:**

The **Scheduler**.

---

## 11. What happens if enough memory is unavailable?

**Answer:**

The Pod remains in the **Pending** state.

---

## 12. Is Memory Request the maximum memory?

**Answer:**

No.

It is the **minimum guaranteed memory**.

---

## 13. What problems do Memory Requests solve?

**Answer:**

- Guarantee minimum memory
- Help Scheduler place Pods
- Prevent memory over-allocation
- Improve cluster utilization

---

## 14. Give one real-world example.

**Answer:**

Suppose PostgreSQL requires at least **4 GB RAM**.

We configure:

```
Memory Request = 4Gi
```

The Pod is scheduled only on a node that can guarantee at least 4 GB RAM.

---

# CPU Limits

## 15. What is a CPU Limit?

**Answer:**

A CPU Limit is the maximum amount of CPU a Pod is allowed to use.

---

## 16. Why do we need CPU Limits?

**Answer:**

CPU Limits prevent a single Pod from consuming all available CPU resources and affecting other Pods.

---

## 17. What happens if a Pod exceeds the CPU Limit?

**Answer:**

Kubernetes throttles the Pod's CPU usage to the configured limit.

---

## 18. What is CPU Throttling?

**Answer:**

CPU Throttling is the process of restricting a Pod's CPU usage when it attempts to exceed its configured CPU Limit.

---

## 19. Is the Pod killed when CPU Limit is exceeded?

**Answer:**

No.

The Pod continues running with reduced CPU usage.

---

## 20. Give one example.

**Answer:**

```
CPU Request = 1

CPU Limit = 2
```

The Pod is guaranteed 1 CPU but can use up to 2 CPUs.

If it tries to use 3 CPUs, Kubernetes throttles it back to 2 CPUs.

---

# Memory Limits

## 21. What is a Memory Limit?

**Answer:**

A Memory Limit is the maximum amount of memory a Pod is allowed to use.

---

## 22. Why do we need Memory Limits?

**Answer:**

Memory Limits prevent a single Pod from consuming all available memory on a worker node.

---

## 23. What happens if a Pod exceeds its Memory Limit?

**Answer:**

Kubernetes immediately terminates the Pod.

---

## 24. What is OOMKilled?

**Answer:**

OOMKilled (Out Of Memory Killed) occurs when a Pod exceeds its configured Memory Limit and Kubernetes terminates it to protect the worker node.

---

## 25. Why is CPU throttled but memory killed?

**Answer:**

CPU can be slowed down without affecting system stability.

Memory cannot.

Exhausting memory can destabilize the operating system, so Kubernetes terminates the Pod.

---

# Requests vs Limits

## 26. Difference between Requests and Limits.

| Requests | Limits |
|-----------|--------|
| Minimum guaranteed resources | Maximum allowed resources |
| Used by Scheduler | Enforced while running |
| Determines Pod placement | Restricts resource usage |

---

## 27. Difference between CPU Limit and Memory Limit.

| CPU Limit | Memory Limit |
|------------|--------------|
| Exceed → CPU Throttling | Exceed → OOMKilled |
| Pod survives | Pod is terminated |

---

## 28. Give one real-world example using Requests and Limits.

**Answer:**

```
CPU Request = 2

CPU Limit = 4

Memory Request = 4Gi

Memory Limit = 8Gi
```

Meaning:

- Guaranteed 2 CPUs and 4 GB RAM
- Can use up to 4 CPUs and 8 GB RAM
- Exceeding CPU Limit results in throttling
- Exceeding Memory Limit results in OOMKilled

---

# Phase 7 — Health Checks

Topics Covered

- Liveness Probe
- Readiness Probe
- Startup Probe

---

# Liveness Probe

## 29. What is a Liveness Probe?

**Answer:**

A Liveness Probe is a Kubernetes health check that determines whether an application is alive and functioning correctly.

---

## 30. Why do we need a Liveness Probe?

**Answer:**

Applications may become unresponsive because of:

- Deadlocks
- Infinite loops
- Frozen processes

Liveness Probes detect these issues and allow Kubernetes to restart the container.

---

## 31. What happens if the Liveness Probe fails?

**Answer:**

Kubernetes:

- Marks the container unhealthy
- Terminates it
- Starts a new container

---

## 32. Does Liveness Probe restart the container?

**Answer:**

Yes.

---

## 33. Can an application be running but unhealthy?

**Answer:**

Yes.

The process may still be running while the application is frozen or unresponsive.

---

## 34. What problems can Liveness Probe detect?

**Answer:**

- Deadlocks
- Infinite loops
- Frozen applications
- Unresponsive services

---

## 35. Does Liveness Probe stop traffic?

**Answer:**

No.

Traffic management is handled by the Readiness Probe.

---

## 36. Give one real-world example.

**Answer:**

Suppose a FastAPI application enters an infinite loop.

The process is still alive, but every request hangs.

The Liveness Probe detects the failure and Kubernetes restarts the container.

---

# Readiness Probe

## 37. What is a Readiness Probe?

**Answer:**

A Readiness Probe determines whether an application is ready to receive user traffic.

---

## 38. Why do we need a Readiness Probe?

**Answer:**

Applications often require initialization before serving users.

Examples:

- Database connection
- Loading ML models
- Cache initialization
- Configuration loading

---

## 39. What happens if the Readiness Probe fails?

**Answer:**

Kubernetes removes the Pod from the Service endpoints but keeps the container running.

---

## 40. Does Kubernetes restart the container?

**Answer:**

No.

---

## 41. Does the application continue running?

**Answer:**

Yes.

Only traffic is stopped.

---

## 42. What happens when the application becomes ready again?

**Answer:**

Kubernetes automatically adds the Pod back to the Service and traffic resumes.

---

## 43. Does Readiness Probe remove traffic?

**Answer:**

Yes.

---

## 44. Give one real-world example.

**Answer:**

Suppose a FastAPI application loads a 5 GB ML model during startup.

The container is running, but the model is still loading.

The Readiness Probe prevents user traffic until loading completes.

---

# Startup Probe

## 45. What is a Startup Probe?

**Answer:**

A Startup Probe determines whether an application has completed startup successfully.

Until it succeeds, Kubernetes does not run Liveness or Readiness probes.

---

## 46. Why do we need a Startup Probe?

**Answer:**

Some applications require a long startup time.

Without a Startup Probe, Liveness may repeatedly restart the application before startup completes.

---

## 47. What happens while the Startup Probe is running?

**Answer:**

- Startup Probe runs
- Liveness Probe is disabled
- Readiness Probe is disabled

---

## 48. What happens if the Startup Probe fails?

**Answer:**

Kubernetes marks the startup as failed, terminates the container, and starts a new one.

---

## 49. Does the Startup Probe run forever?

**Answer:**

No.

It only runs during application startup.

---

## 50. When do Liveness and Readiness Probes start?

**Answer:**

Only after the Startup Probe succeeds.

---

## 51. Which applications commonly require a Startup Probe?

**Answer:**

- Large LLMs
- Large ML models
- Spring Boot applications
- Large databases
- Cache restoration systems

---

## 52. Difference between Startup, Liveness, and Readiness Probes.

| Probe | Checks | Failure Result |
|--------|--------|----------------|
| Startup | Has the application finished starting? | Restart container |
| Liveness | Is the application alive? | Restart container |
| Readiness | Is the application ready for traffic? | Remove Pod from Service |

---

## 53. Give one real-world example.

**Answer:**

Suppose a FastAPI application loads a 20 GB LLM and takes 3 minutes to start.

Without a Startup Probe, the Liveness Probe may restart the application every 30 seconds.

Using a Startup Probe delays Liveness and Readiness until startup finishes, allowing the application to start successfully.

---

# Quick Revision

## Requests

- Minimum guaranteed resources
- Used by Scheduler

---

## Limits

- Maximum allowed resources
- CPU → Throttling
- Memory → OOMKilled

---

## Health Checks

### Startup Probe

Checks startup completion.

Failure → Restart container.

---

### Liveness Probe

Checks whether the application is alive.

Failure → Restart container.

---

### Readiness Probe

Checks whether the application is ready to receive traffic.

Failure → Remove Pod from Service.

---

# Most Asked Interview Questions

1. Difference between Requests and Limits.
2. CPU Throttling vs OOMKilled.
3. Difference between Liveness, Readiness, and Startup Probes.
4. Why does Kubernetes throttle CPU but kill Pods exceeding Memory Limits?
5. Can a container be running but not ready?
6. Can a container be running but unhealthy?
7. When should Startup Probe be used?
8. Which Kubernetes component uses Requests?
9. What happens when a Readiness Probe fails?
10. What happens when a Liveness Probe fails?
---
# Phase 10 & Phase 11 Interview Questions and Answers
# Scaling & Kubernetes Operations

---

# Horizontal Pod Autoscaler (HPA)

## Q1. What is Horizontal Pod Autoscaler (HPA)?

**Answer:**

Horizontal Pod Autoscaler (HPA) is a Kubernetes object that automatically increases or decreases the number of Pod replicas based on metrics such as CPU usage, memory usage, or custom metrics.

---

## Q2. Why do we need HPA?

**Answer:**

Application traffic constantly changes. During high traffic, more Pods are needed to handle requests, while during low traffic, excess Pods waste resources. HPA automatically adjusts the number of Pods to maintain performance and optimize resource usage.

---

## Q3. Does HPA increase CPU or Memory of a Pod?

**Answer:**

No.

HPA increases or decreases the number of Pods.

It does not change the CPU or Memory of existing Pods.

---

## Q4. What is Horizontal Scaling?

**Answer:**

Horizontal Scaling means increasing or decreasing the number of Pods.

Example:

3 Pods → 10 Pods

---

## Q5. What is Vertical Scaling?

**Answer:**

Vertical Scaling means increasing or decreasing the CPU or Memory allocated to a Pod instead of creating new Pods.

---

## Q6. Which Kubernetes object actually creates new Pods when HPA scales?

**Answer:**

HPA modifies the Deployment replica count.

Deployment updates the ReplicaSet.

ReplicaSet creates or removes Pods.

---

## Q7. Which metrics can HPA use?

**Answer:**

- CPU Usage
- Memory Usage
- Custom Metrics
- External Metrics

---

## Q8. What is Metrics Server?

**Answer:**

Metrics Server is a Kubernetes component that collects CPU and Memory usage from Pods and Nodes and provides those metrics to HPA.

---

## Q9. What are Min Replicas?

**Answer:**

Minimum number of Pods HPA is allowed to maintain regardless of traffic.

---

## Q10. What are Max Replicas?

**Answer:**

Maximum number of Pods HPA is allowed to create regardless of traffic.

---

## Q11. Real-world Example

**Answer:**

Suppose a FastAPI application normally runs with 3 Pods.

Traffic suddenly increases to 10,000 requests.

HPA detects high CPU usage.

It increases the Deployment replica count.

ReplicaSet creates more Pods.

When traffic decreases, HPA scales back to the minimum number of replicas.

---

# kubectl

## Q1. What is kubectl?

**Answer:**

kubectl is the official Kubernetes command-line tool used to communicate with and manage Kubernetes clusters through the API Server.

---

## Q2. Why do we need kubectl?

**Answer:**

It is used to create, update, delete, monitor, and debug Kubernetes resources.

---

## Q3. Does kubectl communicate directly with worker nodes?

**Answer:**

No.

kubectl sends requests to the API Server, which manages communication with the cluster.

---

## Q4. Which component receives kubectl requests?

**Answer:**

API Server.

---

## Q5. What is the purpose of `kubectl apply`?

**Answer:**

It creates a resource if it does not exist or updates it if it already exists.

---

## Q6. Difference between `kubectl create` and `kubectl apply`.

| kubectl create | kubectl apply |
|----------------|---------------|
| Creates new resources | Creates or updates resources |
| Fails if resource exists | Updates existing resources |

---

## Q7. What does `kubectl get pods` do?

**Answer:**

Displays all Pods in the current namespace.

---

## Q8. What does `kubectl get all` display?

**Answer:**

Displays common Kubernetes resources such as Pods, Deployments, ReplicaSets, and Services.

---

# kubectl logs

## Q1. What is `kubectl logs`?

**Answer:**

A Kubernetes command used to view logs of a running container for debugging and monitoring.

---

## Q2. Why do we need it?

**Answer:**

To inspect runtime errors such as Python exceptions, database connection failures, authentication failures, and model loading issues.

---

## Q3. What does `kubectl logs pod-name` do?

**Answer:**

Displays logs of the container running inside the specified Pod.

---

## Q4. What does `kubectl logs -f pod-name` do?

**Answer:**

Continuously streams logs in real time.

---

## Q5. What does `kubectl logs --previous pod-name` do?

**Answer:**

Displays logs of the previous container before it restarted.

---

## Q6. Why do we use `-c container-name`?

**Answer:**

To view logs of a specific container in a multi-container Pod.

---

# kubectl describe

## Q1. What is `kubectl describe`?

**Answer:**

A Kubernetes command used to display detailed information, configuration, status, conditions, and events of Kubernetes resources.

---

## Q2. Why do we need it?

**Answer:**

To diagnose scheduling failures, image pull failures, restart loops, and configuration issues.

---

## Q3. What information does it display?

**Answer:**

- Pod Name
- Namespace
- Node
- Labels
- IP Address
- Container Information
- Requests & Limits
- Conditions
- Events

---

## Q4. Which section is most useful?

**Answer:**

Events.

---

## Q5. Difference between `kubectl get` and `kubectl describe`.

| kubectl get | kubectl describe |
|--------------|------------------|
| Summary | Detailed information |
| Shows status | Shows configuration, conditions, and events |

---

## Q6. What does `FailedScheduling` mean?

**Answer:**

Scheduler could not place the Pod because of insufficient resources or scheduling constraints.

---

## Q7. What does `ImagePullBackOff` mean?

**Answer:**

Kubernetes failed to download the container image due to an incorrect image name or registry authentication issues.

---

# kubectl exec

## Q1. What is `kubectl exec`?

**Answer:**

A Kubernetes command used to execute commands inside a running container for debugging and troubleshooting.

---

## Q2. Why do we need it?

**Answer:**

To inspect files, verify models, check environment variables, and debug applications from inside the container.

---

## Q3. Does `kubectl exec` connect directly to the worker node?

**Answer:**

No.

It communicates through the API Server, which forwards the request to the kubelet.

---

## Q4. What does `kubectl exec -it pod-name -- /bin/bash` do?

**Answer:**

Opens an interactive Bash terminal inside the running container.

---

## Q5. What do `-i` and `-t` mean?

**Answer:**

- `-i` → Interactive mode
- `-t` → Allocate a terminal

---

## Q6. Why might `/bin/sh` be used instead of `/bin/bash`?

**Answer:**

Some container images do not include Bash but provide a shell through `/bin/sh`.

---

## Q7. How do you enter a specific container?

**Answer:**

```bash
kubectl exec -it pod-name -c container-name -- /bin/bash
```

---

## Q8. Difference between `kubectl logs` and `kubectl exec`.

| kubectl logs | kubectl exec |
|---------------|--------------|
| View logs | Execute commands inside container |
| Read-only | Interactive |

---

# kubectl rollout

## Q1. What is `kubectl rollout`?

**Answer:**

A Kubernetes command used to monitor Deployment updates, restart Deployments, view rollout history, and roll back to previous versions.

---

## Q2. Why do we need it?

**Answer:**

To safely manage application deployments with minimal downtime.

---

## Q3. What does `kubectl rollout status` do?

**Answer:**

Displays whether a Deployment has completed successfully.

---

## Q4. What does `kubectl rollout history` do?

**Answer:**

Displays the revision history of a Deployment.

---

## Q5. What does `kubectl rollout undo` do?

**Answer:**

Rolls back the Deployment to the previous stable revision.

---

## Q6. What is a rollout revision?

**Answer:**

A saved version of a Deployment created after every successful deployment.

---

## Q7. What does `kubectl rollout restart` do?

**Answer:**

Restarts all Pods managed by a Deployment without changing the image.

---

## Q8. Difference between `kubectl apply` and `kubectl rollout`.

| kubectl apply | kubectl rollout |
|----------------|-----------------|
| Creates or updates resources | Manages deployment after update |

---

# kubectl scale

## Q1. What is `kubectl scale`?

**Answer:**

A Kubernetes command used to manually increase or decrease the number of replicas of Deployments, ReplicaSets, or StatefulSets.

---

## Q2. Why do we need it?

**Answer:**

To immediately adjust the number of running Pods without modifying the Deployment YAML file.

---

## Q3. Does `kubectl scale` create Pods?

**Answer:**

No.

It changes the replica count.

Deployment and ReplicaSet create or remove Pods.

---

## Q4. Which Kubernetes object actually creates Pods?

**Answer:**

ReplicaSet.

---

## Q5. What does `kubectl scale deployment fastapi --replicas=10` do?

**Answer:**

Changes the Deployment replica count to 10, causing the ReplicaSet to create additional Pods until 10 replicas are running.

---

## Q6. Difference between `kubectl scale` and HPA.

| kubectl scale | HPA |
|----------------|-----|
| Manual | Automatic |
| User decides replicas | Metrics decide replicas |

---

## Q7. Real-world Example

**Answer:**

Before a product launch, manually scale a Deployment from 3 Pods to 20 Pods using `kubectl scale`. After traffic returns to normal, reduce it back to 3 Pods.