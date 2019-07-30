# Request Helm Charts repository

This repository contains sources for all of Request Helm Charts, available at https://request-charts.storage.googleapis.com.

## How to use?

```
helm repo add request https://request-charts.storage.googleapis.com
helm repo update
helm install request/request-node
```

## Contributing

New Charts must have their own top-level folders, and a new job in [.circleci/config.yml](.circleci/config.yml).

The chart repository will be automatically when your PR is merged to master.
