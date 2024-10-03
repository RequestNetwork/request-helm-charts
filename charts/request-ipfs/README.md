# Request IPFS Helm Chart

This chart allows you to deploy a dedicated Request IPFS node, on a Kubernetes cluster.

## StatefulSet Details

- https://kubernetes.io/docs/concepts/abstractions/controllers/statefulsets/

## StatefulSet Caveats

- https://kubernetes.io/docs/concepts/abstractions/controllers/statefulsets/#limitations

## Chart Details

This chart implements a dedicated Request IPFS server using Kubernetes StatefulSets.

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm repo add request https://request-charts.storage.googleapis.com
helm repo update
$ helm install --name my-release request/request-ipfs
```

## Configuration

The following table lists the configurable parameters of the Request IPFS chart and their default values.

| Parameter                      | Description                                                                                                       | Default                       |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------|-------------------------------|
| `replicaCount`                 | The amount of replicas to run                                                                                     | `1`                           |
| `image.image`                  | The docker image for the dedicated IPFS node                                                                      | `requestnetwork/request-ipfs` |
| `image.tag`                    | The version tag for the dedicated IPFS node image                                                                 | `v0.4.23`                     |
| `image.pullPolicy`             | Dedicated IPFS node image pull policy                                                                             | `Always`                      |
| `swarm.externalIP`             | Swarm address to announce to the network (optional). Usually should be the same as `swarm.service.loadBalancerIP` | `null`                        |
| `swarm.service.enabled`        | Whether to enable the load service to access to IPFS swarm                                                        | `true`                        |
| `swarm.service.type`           | The service type to access the IPFS swarm                                                                         | `LoadBalancer`                |
| `swarm.service.loadBalancerIP` | Static IP address used by the load balancer (optional)                                                            | `null`                        |
| `identity.peerId`              | The IPFS node PeerID (optional)                                                                                   | `null`                        |
| `identity.privateKey`          | The IPFS node Private Key (optional)                                                                              | `null`                        |
| `extraEnvs`                    | Additional environment variables to pass down to the IPFS node (optional)                                         | `{}`                          |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml request/request-ipfs
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
$ helm upgrade --set cluster.replicas=4 my-release request/request-ipfs
```
