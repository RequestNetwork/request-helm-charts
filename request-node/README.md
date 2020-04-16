# Request Node 0.5.5 Helm Chart

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

| Parameter                   | Description                                           | Default                       |
| --------------------------- | ----------------------------------------------------- | ----------------------------- |
| `nodeEnv.mnemonic`          | The mnemonic of a wallet for gas fees                 | **Required value**            |
| `nodeEnv.web3ProviderUrl`   | The URL of the Web3 provider (infura for instance)    | **Required value**            |
| `nodeEnv.networkId`         | The ethereum network (0 custom, 1 mainnet, 4 rinkeby) | **Required value**            |
| `nodeEnv.logLevel`          | The node log level (ERROR, WARN, INFO or DEBUG)       | `DEBUG`                       |
| `replicaCount`              | The amount of replicas to run                         | `1`                           |
| `nodeImage.image`           | The docker image for the Request Node                 | `requestnetwork/request-node` |
| `nodeImage.tag`             | The version tag for the Request Node image            | `0.5.5`                       |
| `nodeImage.pullPolicy`      | Request Node image pull policy                        | `Always`                      |
| `ipfs.image.image`          | The docker image for the dedicated IPFS server        | `requestnetwork/request-ipfs` |
| `ipfs.image.tag`            | The version tag for the dedicated IPFS server image   | `0.3.4`                       |
| `ipfs.image.pullPolicy`     | Dedicated IPFS server image pull policy               | `Always`                      |
| `ipfs.swarm.loadBalancerIP` | The IPFS swarm load balancer IP (optional)            | ``                            |
| `ipfs.swarm.externalIP`     | The IPFS swarm load balancer external IP (optional)   | ``                            |
| `ipfs.identity.peerId`      | The IPFS node PeerID (optional)                       | ``                            |
| `ipfs.identity.privateKey`  | The IPFS node Private Key (optional)                  | ``                            |

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
