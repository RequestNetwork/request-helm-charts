> ### ℹ️ Note: the HELM chart repository URL has changed
> - from: https://request-charts.storage.googleapis.com
> - to: https://requestnetwork.github.io/request-helm-charts
> 
> Please migrate to the new URL, the legacy URL will soon be retired.

# Request Helm Charts repository

This repository contains sources for all Request Helm Charts.
They use Docker images available at https://hub.docker.com/u/requestnetwork.

## How to use?

```
helm repo add request https://requestnetwork.github.io/request-helm-charts
helm repo update
helm install request/request-node
```

## Documentation
- `request-ipfs` on [ArtifactHub](https://artifacthub.io/packages/helm/requestnetwork/request-ipfs)
- `request-node` on [ArtifactHub](https://artifacthub.io/packages/helm/requestnetwork/request-node)

## Contributing

New Charts must have their own top-level folders, and a new job in [.circleci/config.yml](.circleci/config.yml).
