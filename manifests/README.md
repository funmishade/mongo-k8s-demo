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

