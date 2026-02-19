
# MongoDB + Mongo Express on Kubernetes

This is a personal project documenting a small Kubernetes deployment with MongoDB and Mongo Express.  
Purpose: To practice deploying databases and web interfaces in Kubernetes and troubleshooting common issues.

---

## Overview

- Deployed MongoDB as a Deployment with a Secret for username/password.
- Exposed MongoDB via a ClusterIP service.
- Deployed Mongo Express as a Deployment with a ConfigMap for DB host and Secret references for credentials.
- Exposed Mongo Express via NodePort for local access.

---

## Repository Structure

- `manifests/` - All Kubernetes YAML files for deployments, services, secrets, and configmaps.
- `notes/` - Logs of errors, troubleshooting steps, and lessons learned.
- `screenshots/` - Optional images of errors, outputs, or UI.

---

## Setup Instructions

1. Clone the repository:

```bash
git clone <repo-url>
cd mongo-k8s-demo


# Mongo K8s Demo

A simple Kubernetes demo connecting **MongoDB** and **Mongo Express** using ConfigMaps, Secrets, Deployments, and Services. This repository documents the project from start to finish, including errors encountered, fixes, and commands used.

---

## **Project Overview**

This project demonstrates:

- Deploying a MongoDB database in Kubernetes.
- Deploying Mongo Express as a web interface to access MongoDB.
- Using **Secrets** for sensitive credentials.
- Using **ConfigMaps** for configuration (MongoDB host).
- Exposing services via NodePort for access from the host machine.
- Connecting deployments via environment variables.

---

## **File-by-File Explanation**

| File | Purpose |
|------|---------|
| `mongodb-deployment.yaml` | Deploys the MongoDB pod. |
| `mongodb-service.yaml` | Exposes MongoDB internally via a ClusterIP service on port 27017. |
| `mongodb-secret.yaml` | Stores MongoDB root username and password in base64. |
| `mongoexpress-configmap.yaml` | Stores the MongoDB host name for Mongo Express deployment. |
| `mongoexpress-deployment.yaml` | Deploys Mongo Express and injects secrets & ConfigMap values as environment variables. |
| `mongoexpress-service.yaml` | Exposes Mongo Express via NodePort on 8081 for access from host machine. |

---

## **Connectivity Diagram**

[MongoDB Pod] ---> (27017) ---> [MongoDB Service] ---> env variables ---> [Mongo Express Pod] ---> (8081) ---> [NodePort / Port-forward / Browser]


- Mongo Express connects to MongoDB using environment variables (`ME_CONFIG_MONGODB_SERVER`, `ME_CONFIG_MONGODB_ADMINUSERNAME`, `ME_CONFIG_MONGODB_ADMINPASSWORD`).

---

## **Environment Variables & Secrets Mapping**

| Variable | Source | Description |
|----------|--------|-------------|
| `ME_CONFIG_MONGODB_SERVER` | ConfigMap `mongoexpress-configmap` | MongoDB host for Mongo Express |
| `ME_CONFIG_MONGODB_ADMINUSERNAME` | Secret `mongo-secret` | MongoDB root username |
| `ME_CONFIG_MONGODB_ADMINPASSWORD` | Secret `mongo-secret` | MongoDB root password |
| `ME_CONFIG_MONGODB_ENABLE_ADMIN` | Deployment env | Enables admin access to all databases |

**Note:** Secrets are stored in base64 in `mongodb-secret.yaml`. Example (decode with `echo dXNlcmh3aDM= | base64 --decode`):

- `mongo-root-username`: `mongouser`  
- `mongo-root-password`: `password123`  

> Do **not commit real passwords** in public repositories.

---

## **Known Issues & Fixes**

| Error | Cause | Fix |
|-------|-------|-----|
| `spec.selector: field is immutable` | Attempting to change Deployment selector after creation | Delete deployment and re-apply with correct selector |
| `Waiting for mongo:27017... /dev/tcp/mongo/27017: Invalid argument` | Mongo container not ready or wrong service name | Ensure service selector matches pod labels, check ConfigMap |
| `port-forward timeout` | Service endpoints `<none>` or pod not ready | Verify pod status with `kubectl get pods -n abc`, ensure Service selector matches pod labels |

---

## **Version & Config Info**

- **MongoDB image:** `mongo:6`  
- **Mongo Express image:** `mongo-express:1.0.2`  
- **Namespace:** `abc`  
- **Service types:** MongoDB → `ClusterIP`, Mongo Express → `NodePort`  
- **Ports:** MongoDB → `27017`, Mongo Express → `8081:NodePort`  

---

## **Commands Cheat Sheet**

```bash
# Create namespace
kubectl create ns abc

# Apply all manifests
kubectl apply -f manifests/

# Check pods
kubectl get pods -n abc

# Check services
kubectl get svc -n abc

# Check endpoints
kubectl get endpoints -n abc

# View logs
kubectl logs <pod-name> -n abc

# Port forward Mongo Express to local host
kubectl port-forward svc/mongoexpress-service 8081:8081 -n abc
Summary
This project is a full end-to-end demo of connecting backend (MongoDB) and frontend (Mongo Express) in Kubernetes, with proper handling of secrets, config, services, and deployments. It documents issues encountered, fixes applied, and provides commands for easy re-deployment or learning reference.
