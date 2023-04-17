## The Problem

Kubernetes Controllers don't scale horizontally.

notes:

- large-scale controller deployments more widespread

---

## The Idea

Apply sharding patterns from distributed databases.

notes:

- proven since years

---

## A Controller

![Single Controller](../assets/single-controller.png)
<!-- .element: class="r-stretch" -->

---

## Multiple Controller Instances

![HA Controller](../assets/ha-controller.png)
<!-- .element: class="r-stretch" -->

notes:

- active-passive
- prevent conflicting actions
- fast failovers
- not horizontal scaling!
- limited by machine size
- vertical scaling only option

---

## Controller Sharding

![Sharded Controller](../assets/sharded-controller.png)
<!-- .element: class="r-stretch" -->

notes:

- partition objects using labels
- instances: label selector, all active
- sharder: watch leases (Bigtable/Chubby)
- sharder: consistent hashing (Cassandra/Dynamo)
- patterns from distributed databases applied to controllers
- (preventing concurrency: drain label)

---

## It's a match!

- Kubernetes controllers can be sharded!
- controller-native implementation
- controller-runtime

notes:

- controller-runtime
