[![CircleCI](https://circleci.com/gh/RequestNetwork/request-helm-charts.svg?style=svg&circle-token=7f672824b8febacaea69fa451b9944fd07454617)](https://circleci.com/gh/RequestNetwork/request-helm-charts)

# Request Helm Charts repository

This repository contains sources for all Request Helm Charts.
They use Docker images available at https://hub.docker.com/u/requestnetwork.

## How to use?

```
helm repo add request https://requestnetwork.github.io/request-helm-charts
helm repo update
helm install request/request-node
```

## Contributing

New Charts must have their own top-level folders, and a new job in [.circleci/config.yml](.circleci/config.yml).
