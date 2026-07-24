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