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