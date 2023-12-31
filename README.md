# Keycloak Helm Chart

Helm Chart for Keycloak by intension

* [Parameters](#parameters)
  * [Common parameters](#common-parameters)
  * [Keycloak parameters](#keycloak-parameters)
  * [Keycloak deployment parameters](#keycloak-deployment-parameters)
  * [Database parameters](#database-parameters)
  * [Keycloak exposure parameters](#keycloak-exposure-parameters)
* [Post install jobs](#post-install-jobs)
* [Supported versions](#supported-versions)

## Parameters

### Common parameters

| Name               | Description                                                             | Value |
| ------------------ | ----------------------------------------------------------------------- | ----- |
| `imagePullSecrets` | Image pull secrets (e.g. when using an image from a private repository) | `[]`  |
| `nameOverride`     | Override the name                                                       | `""`  |
| `fullnameOverride` | Override the fully qualified domain                                     | `""`  |

### Keycloak parameters

| Name                | Description                                                                                  | Value                       |
| ------------------- | -------------------------------------------------------------------------------------------- | --------------------------- |
| `image.repository`  | The image repository for the Keycloak container                                              | `quay.io/keycloak/keycloak` |
| `image.pullPolicy`  | Pull policy of the image used for Keycloak                                                   | `IfNotPresent`              |
| `image.tag`         | Overrides the image tag whose default is the chart appVersion.                               | `22.0.1`                    |
| `image.digest`      | Overrides the image tag with a specific digest                                               | `""`                        |
| `http.relativePath` | For compatibility reason this could be set to "/auth"                                        | `""`                        |
| `proxy.enabled`     | Enable proxy address forwarding if Keycloak is behind a reverse proxy                        | `false`                     |
| `proxy.mode`        | Specify the proxy mode. Can be "none", "edge", "reencrypt" or "passthrough"                  | `""`                        |
| `metrics.enabled`   | Enable Keycloak's metrics endpoint                                                           | `true`                      |
| `health.enabled`    | Enable Keycloak's helath endpoint. This also enables startup, readiness and liveness probes. | `true`                      |
| `command`           | Override the default entrypoint of the Keycloak container                                    | `[]`                        |
| `args`              | Override the default args for the Keycloak container                                         | `[]`                        |
| `extraEnv`          | Extra environment variables for Keycloak                                                     | `""`                        |
| `extraEnvFrom`      | Get extra environment variables for Keycloak from ConfigMap or Secret                        | `""`                        |

### Keycloak deployment parameters

| Name                                            | Description                                                                                                                       | Value           |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| `replicaCount`                                  | Number of replicas in the StatefulSet                                                                                             | `1`             |
| `updateStrategy`                                | Update strategy of the StatefulSet                                                                                                | `RollingUpdate` |
| `restartPolicy`                                 | Restart policy of the StatefulSet                                                                                                 | `Always`        |
| `podManagementPolicy`                           | Control how pods are created during initial scale up, replacement or when scaling down. Either "OrderedReady" or "Parallel".      | `OrderedReady`  |
| `livenessProbe`                                 | Liveness probe configuration                                                                                                      | `""`            |
| `readinessProbe`                                | Readiness probe configuration                                                                                                     | `""`            |
| `startupProbe`                                  | Startup probe configuration                                                                                                       | `""`            |
| `statefulSet.annotations`                       | Annotations to add to the StatefulSet                                                                                             | `{}`            |
| `statefulSet.labels`                            | Labels to add to the StatefulSet                                                                                                  | `{}`            |
| `podAnnotations`                                | Annotations to add to the Pod                                                                                                     | `{}`            |
| `podLabels`                                     | Labels to add to the Pod                                                                                                          | `{}`            |
| `podSecurityContext`                            | Pod wide SecurityContext. Defines security options the container shold be run with.                                               | `{}`            |
| `securityContext`                               | Security context for the main container.                                                                                          | `{}`            |
| `nodeSelector`                                  | Specify the node for the pod to be scheduled on.                                                                                  | `{}`            |
| `affinity`                                      | Affinity for pod assignment                                                                                                       | `{}`            |
| `tolerations`                                   | Tolerations for pod assignment                                                                                                    | `[]`            |
| `priorityClassName`                             | If specified, indicates the pod's priority.                                                                                       | `""`            |
| `terminationGracePeriodSeconds`                 | Termination grace period in seconds for Keycloak shutdown. In clusters with a large cache Infinispan might need time to rebalance | `60`            |
| `hostAliases`                                   | Mapped IPs and hostnames that will be injected into Pod's hosts files                                                             | `[]`            |
| `extraPorts`                                    | Extra ports for the pod                                                                                                           | `[]`            |
| `topologySpreadConstraints`                     | Describe how a group of pods can spread across topology domains.                                                                  | `[]`            |
| `enableServiceLinks`                            | Indicate whether information about serfices should be injected into pod's environment variables                                   | `true`          |
| `extraVolumes`                                  | Extra volumes, e.g. for additional themes                                                                                         | `""`            |
| `extraVolumeMounts`                             | Extra volume mounts, e.g. for additional themes                                                                                   | `""`            |
| `resources`                                     | The source limits for Keycloak                                                                                                    | `{}`            |
| `podDisruptionBudget`                           | Pod disruption budget                                                                                                             | `{}`            |
| `podDisruptionBudgetAnnotations`                | Annotations for the pod disruption budget                                                                                         | `{}`            |
| `podDisruptionBudgetLabels`                     | Labels for the pod disruption budget                                                                                              | `{}`            |
| `serviceAccount.create`                         | Specifies whether a service account should be created                                                                             | `true`          |
| `serviceAccount.annotations`                    | Annotations to add to the service account                                                                                         | `{}`            |
| `serviceAccount.labels`                         | Labels to add to the service account                                                                                              | `{}`            |
| `serviceAccount.name`                           | The name of the service account to use. If not set and create is true, a name is generated using the fullname template            | `""`            |
| `serviceAccount.automountServiceAccountToken`   | Automount API token for the service account                                                                                       | `true`          |
| `serviceAccount.rbac.enable`                    | Enable the creation of ClusterRole and ClusterRoleBinding for the service account to be able to get and list pods                 | `true`          |
| `serviceAccount.rbac.rules`                     | The rules for the ClusterRole                                                                                                     | `[]`            |
| `autoscaling.enabled`                           | Enable autoscaling for Keycloak                                                                                                   | `false`         |
| `autoscaling.minReplicas`                       | Minimum number of replicas to scale back                                                                                          | `1`             |
| `autoscaling.maxReplicas`                       | Maximum number of replicas to scale up                                                                                            | `100`           |
| `autoscaling.targetCPUUtilizationPercentage`    | Target CPU utilization percentage                                                                                                 | `80`            |
| `autoscaling.targetMemoryUtilizationPercentage` | Target Memory utilization percentage                                                                                              | `nil`           |
| `autoscaling.labels`                            | Labels for the HPA                                                                                                                | `{}`            |
| `postInstallJobs`                               | Define a list of jobs to be executed after the installation                                                                       | `[]`            |

### Database parameters

| Name                           | Description                                                                        | Value |
| ------------------------------ | ---------------------------------------------------------------------------------- | ----- |
| `database.existingSecret.name` | Use an existing secret for database password. This takes precedence over password. | `""`  |
| `database.existingSecret.key`  | The key for the existing secret                                                    | `""`  |
| `database.vendor`              | Can be dev-file, dev-mem, mariadb, mssql, mysql, oracle, postgres                  | `""`  |
| `database.hostname`            | Hostname of the database                                                           | `""`  |
| `database.port`                | Port of the database                                                               | `""`  |
| `database.database`            | The name of the database                                                           | `""`  |
| `database.username`            | Username of the database                                                           | `""`  |
| `database.password`            | Password of the database. Ignored if existing secret is set                        | `""`  |

### Keycloak exposure parameters

| Name                               | Description                                                                                                                                                                                                                                   | Value       |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `service.annotations`              | Annotations to add to the http service                                                                                                                                                                                                        | `{}`        |
| `service.headless.annotations`     | Annotations to add to the headless serivce                                                                                                                                                                                                    | `{}`        |
| `service.labels`                   | Labels to add to the http and the headless services                                                                                                                                                                                           | `{}`        |
| `service.type`                     | Specify the type of the service                                                                                                                                                                                                               | `ClusterIP` |
| `service.loadBalancerIP`           | Optional IP for service of type loadbalancer                                                                                                                                                                                                  | `""`        |
| `service.httpPort`                 | The http Port of the service                                                                                                                                                                                                                  | `80`        |
| `service.httpsPort`                | The https Port of the service                                                                                                                                                                                                                 | `443`       |
| `service.httpNodePort`             | The http Port of the service if its type is NodePort                                                                                                                                                                                          | `nil`       |
| `service.httpsNodePort`            | The https Port of the service if its type is NodePort                                                                                                                                                                                         | `nil`       |
| `service.extraPorts`               | Specify extra ports for the service                                                                                                                                                                                                           | `[]`        |
| `service.loadBalancerSourceRanges` | Restrict source ranges allowed to connect to the loadbalancer if service is of type loadbalancer                                                                                                                                              | `[]`        |
| `service.externalTrafficPolicy`    | If set to "Local", the proxy assumes that external load balancers will take care of balancing the service traffic between nodes and won't masquerade the client source IP. If set to "Cluster" the standard behavior of routing will be used. | `Cluster`   |
| `service.sessionAffinity`          | Session Affinity                                                                                                                                                                                                                              | `""`        |
| `service.sessionAffinityConfig`    | Session Affinity Config                                                                                                                                                                                                                       | `{}`        |
| `ingress.enabled`                  | Enable ingress                                                                                                                                                                                                                                | `false`     |
| `ingress.className`                | Set the ingerssClassName on the ingress record                                                                                                                                                                                                | `""`        |
| `ingress.annotations`              | Annotations for the ingress                                                                                                                                                                                                                   | `{}`        |
| `ingress.rules`                    | List of arbitrary paths to the host                                                                                                                                                                                                           | `[]`        |
| `ingress.tls`                      | List of TLS secrets                                                                                                                                                                                                                           | `[]`        |

### Realm creator parameters

This Helm chart provides the possibility to create a realm after Keycloak was deployed. This is done using [keycloak-config-cli](https://github.com/adorsys/keycloak-config-cli). All options from realmCreator.options are passed to this cli.

| Name                             | Description                                                                                                                                                                                                                                                   | Value                         |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| `realmCreator.realms`            | Specify how the realms should be created. It contains a dictionary mapping a string for enumeration and the string specifying how the realm should be created. See [keycloak-config-cli](https://github.com/adorsys/keycloak-config-cli) for more information | `{}`                          |
| `realmCreator.image.repository`  | The image for the realm creator                                                                                                                                                                                                                               | `adorsys/keycloak-config-cli` |
| `realmCreator.image.pullPolicy`  | Pull policy of the image used for the realm creator                                                                                                                                                                                                           | `IfNotPresent`                |
| `realmCreator.image.tag`         | The tag of the image for the realm creator                                                                                                                                                                                                                    | `5.8.0-22.0.0`                |
| `realmCreator.image.digest`      | Overrides the image tag with a specific digest                                                                                                                                                                                                                | `""`                          |
| `realmCreator.username`          | Specify the username to be used for the Keycloak API                                                                                                                                                                                                          | `admin`                       |
| `realmCreator.password`          | Password for the Keycloak user                                                                                                                                                                                                                                | `""`                          |
| `realmCreator.existingSecret`    | Use an existing secret for Keycloak admin password. This takes precedence over password.                                                                                                                                                                      | `""`                          |
| `realmCreator.existingSecretKey` |                                                                                                                                                                                                                                                               | `""`                          |

## Post install jobs

This chart offers the possibility to define jobs that run after the installation. To enforce that it runs after the pod is ready, either use the `--wait` option from `helm install` or define a init container that waits for pod to be ready.

## Supported versions

This chart supports Keycloak version >= 17.0.0 using the Quarkus distribution.
