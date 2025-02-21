Deployments are the most popular way of running stateless apps on Kubernetes. They add self-healing, scaling, rollouts, and rollbacks.

## Deployment and ReplicaSets

We’ve repeatedly said that Deployments add self-healing, scaling, rollouts, and rollbacks. However, behind the scenes, it’s actually a different resource called a ReplicaSet that provides the self-healing and scaling.

You’ll sometimes hear people refer to *multi-dimensional autoscaling*. This is jargon for combining multiple scaling methods — scaling Pods and nodes, or scaling apps horizontally (adding more Pods) and vertically (adding more resources to existing Pods).

ReplicaSets are implemented as a background controller running in a reconciliation loop, ensuring the correct number of Pod replicas are always present. If there aren’t enough Pods, it adds more. If there are too many, it terminates some.

In a Deployment manifest file:

- **spec.revisionHistoryLimit** tells Kubernetes to keep the previous five ReplicaSets
so you can roll back to the last five versions. Keeping more gives you more rollback
options, but keeping too many can bloat the object and cause problems on large clusters
with lots of releases.

- **spec.progressDeadlineSeconds** tells Kubernetes to give each new replica a five-
minute start window before reporting the update as stalled. The counter is reset for
each replica, meaning each replica has its own five-minute window to come up properly
(progress).

Note: The terms rollout, release, zero-downtime update, and rolling update mean
the same thing, and we’ll use them interchangeably.

