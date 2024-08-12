# Keycloak KinD Setup

This is a small Keycloak server that is meant to be deployed with [KinD](https://kind.sigs.k8s.io/). It includes the complete setup for:

- Deploying a KinD cluster
- Adding Crossplane
- Installing the Keycloak Operator
- Building a Keycloak Server
- Configuring the Keycloak Server with a custom realm, role, and user

## Deploy

```sh
# create the KinD cluster
kind create cluster --config cluster.yaml

# Deploy Crossplane
kubectl create ns crossplane
helm install crossplane ./crossplane -n crossplane

# Deploy Keycloak Operator
kubectl apply -k ./keycloak-operator

# Deploy Keycloak Server
kubectl apply -k ./keycloak-server

# Deploy Keycloak Config
helm install keycloak-config ./keycloak-config -n keycloak

# Create a port forward to access the UI:
kubectl port-forward service/example-kc-service -n keycloak 8080:8080
```

The UI should now be accessible on `localhost:8080`, login with username `admin` and password `password`. A realm named `test-realm` should exist with a user and a client already configured.
