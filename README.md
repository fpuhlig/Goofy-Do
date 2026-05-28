# Goofy-Do

Goofy-Do is a todo application with a React frontend, a Quarkus/Kotlin backend, PostgreSQL, Keycloak, and Kubernetes deployment manifests.

## Repository layout

```text
backend/        Quarkus/Kotlin backend
frontend/       React/Vite frontend
deployment/     Kubernetes manifests and Kustomize configuration
development/    Local development files
keycloak/       Keycloak Dockerfile and realm setup
scripts/        Utility scripts
docs/           Project documentation
evidence/       Verification evidence
```

## Requirements

The Makefile checks for these tools before starting the deployment:

- minikube
- kubectl
- openssl

Docker is required by the Minikube Docker driver used in the Makefile.

## Local Kubernetes setup

Start the local environment:

```bash
make up
```

`make up` runs these Makefile targets:

```text
check-prereqs -> cluster-start -> ns -> secrets -> tls-secret -> deploy -> wait-ready
```

The Kubernetes namespace is:

```text
goofydo
```

The local domain used by the Makefile is:

```text
goofydo.local
```

Set up local access:

```bash
make tunnel
```

On Linux, the Makefile prints the Minikube IP and the `/etc/hosts` command for `goofydo.local`.

## Makefile commands

```bash
make help       # List available targets
make up         # Start the local environment
make stop       # Stop the Minikube VM and keep data
make start      # Start the Minikube VM again
make suspend    # Scale all deployments to 0
make resume     # Scale all deployments back to 1
make undeploy   # Delete app resources and keep secrets/data
make destroy    # Delete namespace, secrets, and persistent data
make restart    # Run destroy and then up
make wait-ready # Wait for deployments to become ready
make tunnel     # Print or start local ingress access
make logs       # Show Kubernetes resources
```

## Kubernetes resources

`deployment/k8s/kustomization.yaml` includes:

- namespace
- PostgreSQL
- Keycloak
- backend
- frontend
- ingress
- database bootstrap job
- service accounts
- network policy

## Frontend

Frontend scripts from `frontend/package.json`:

```bash
cd frontend
npm run dev
npm run build
npm run lint
npm run preview
```

Main frontend dependencies include React, React Router, Vite, TypeScript, Keycloak JS, Tailwind CSS, and Lucide React.

## Backend

The backend uses Gradle with:

- Kotlin JVM
- Quarkus
- Java 21 target
- PostgreSQL JDBC
- Hibernate ORM and Panache
- Flyway
- OIDC and Keycloak authorization
- OpenAPI and Swagger UI

Backend development commands are documented in `backend/README.md`.

## Troubleshooting

Check pods and events:

```bash
kubectl get pods -n goofydo
kubectl get events -n goofydo --sort-by=.lastTimestamp
```

Check the Minikube IP:

```bash
minikube ip
```
