# Request Node Helm Chart

This chart allows you to deploy a Request Network node, with it's own IPFS server, on a Kubernetes cluster.

## StatefulSet Details

- https://kubernetes.io/docs/concepts/abstractions/controllers/statefulsets/

## StatefulSet Caveats

- https://kubernetes.io/docs/concepts/abstractions/controllers/statefulsets/#limitations

## Chart Details

This chart implements a full [Request Node](https://github.com/RequestNetwork/requestNetwork/tree/development/packages/request-node) with it's own IPFS server using Kubernetes StatefulSets.

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm repo add request https://request-charts.storage.googleapis.com
helm repo update
$ helm install --name my-release request/request-node
```

## Configuration

The following table lists the configurable parameters of the Request Node chart and their default values.

| Parameter                           | Description                                                                                                            | Default                       |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| `nodeEnv.mnemonic`                  | The mnemonic of a wallet for gas fees                                                                                  | **Required value**            |
| `nodeEnv.web3ProviderUrl`           | The URL of the Web3 provider (infura for instance)                                                                     | **Required value**            |
| `nodeEnv.networkId`                 | The ethereum network (0 custom, 1 mainnet, 4 rinkeby)                                                                  | **Required value**            |
| `nodeEnv.logLevel`                  | The node log level (ERROR, WARN, INFO or DEBUG)                                                                        | `DEBUG`                       |
| `replicaCount`                      | The amount of replicas to run                                                                                          | `1`                           |
| `nodeImage.image`                   | The docker image for the Request Node                                                                                  | `requestnetwork/request-node` |
| `nodeImage.tag`                     | The version tag for the Request Node image                                                                             | `0.5.5`                       |
| `nodeImage.pullPolicy`              | Request Node image pull policy                                                                                         | `Always`                      |
| `ipfs.image.image`                  | The docker image for the dedicated IPFS server                                                                         | `requestnetwork/request-ipfs` |
| `ipfs.image.tag`                    | The version tag for the dedicated IPFS server image                                                                    | `v0.4.23`                     |
| `ipfs.image.pullPolicy`             | Dedicated IPFS server image pull policy                                                                                | `Always`                      |
| `ipfs.swarm.port`                   | Port to access the IPFS swarm                                                                                          | `4001`                        |
| `ipfs.swarm.externalIP`             | Swarm address to announce to the network (optional). Usually should be the same as `ipfs.swarm.service.loadBalancerIP` | `null`                        |
| `ipfs.swarm.service.enabled`        | Whether to enable the load service to access to IPFS swarm                                                             | `true`                        |
| `ipfs.swarm.service.type`           | The service type to access the IPFS swarm                                                                              | `LoadBalancer`                |
| `ipfs.swarm.service.loadBalancerIP` | Static IP address used by the load balancer (optional)                                                                 | `null`                        |
| `ipfs.identity.peerId`              | The IPFS node PeerID (optional)                                                                                        | `null`                        |
| `ipfs.identity.privateKey`          | The IPFS node Private Key (optional)                                                                                   | `null`                        |
| `ipfs.extraEnvs`                    | Additional environment variables to pass down to the IPFS node (optional)                                              | `{}`                          |


Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml request/request-node
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Cleanup orphaned Persistent Volumes

Deleting a StatefulSet will not delete associated Persistent Volumes.

Do the following after deleting the chart release to clean up orphaned Persistent Volumes.

```console
$ kubectl delete pvc -l release=my-release
```

## Scaling

Scaling should be managed by `helm upgrade`, which is the recommended way. Example:

```
$ helm upgrade --set cluster.replicas=4 my-release request/request-node
```
