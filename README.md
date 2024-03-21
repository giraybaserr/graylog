# Graylog Deployment on Kubernetes

This guide provides a step-by-step process to deploy Graylog on Kubernetes and configure ingress for access.

## Prerequisites

Ensure MongoDB is deployed in your Kubernetes cluster. If you're using an external OpenSearch or Elasticsearch cluster, make sure it's accessible from your Kubernetes cluster. If the OpenSearch or Elasticsearch cluster is deployed within the same Kubernetes cluster, ensure it's located in the same `namespace` as your Graylog deployment for seamless connectivity.

## Creating a Namespace

Start by creating a dedicated namespace for Graylog:

```bash
kubectl create namespace "namespace"
```
## Deploying Graylog with Helm

Deploy Graylog into the namespace namespace, specifying the OpenSearch or Elasticsearch configuration:
```bash
helm repo add kongz https://charts.kongz.me
helm repo update

helm install graylog kongz/graylog \
  --namespace "namespace" \
  --set tags.install-opensearch=false \
  --set graylog.opensearch.hosts=http://elasticsearch-master-headless.namespace.svc.cluster.local:9200 \
  --set graylog.opensearch.version=7
```

## Applying Ingress Configuration
After Graylog is successfully deployed, apply the ingress configuration to access Graylog through a web browser:
```bash
kubectl apply -f graylog-ingress.yaml --namespace "namespace"
```
Make sure you have an graylog-ingress.yaml file prepared with the appropriate configuration for your ingress controller and domain.

## Verifying the Deployment

Check the status of your Graylog deployment:
```bash
helm status graylog --namespace "namespace"
```
