# Towards Horizontally Scalable Kubernetes Controllers (Lightning Talk)

[![Netlify Status](https://api.netlify.com/api/v1/badges/e06fa269-6329-42d3-9f49-9356ee0cc204/deploy-status)](https://app.netlify.com/sites/talk-controller-sharding-lightning/deploys)

Take me to the [slides](https://talks.timebertt.dev/controller-sharding-lightning/)!

## About

This is a lightning talk by [@timebertt](https://github.com/timebertt) at [Cloud Native Rejekts 2023](https://cloud-native.rejekts.io/) in Amsterdam ([event schedule](https://cfp.cloud-native.rejekts.io/cloud-native-rejekts-eu-amsterdam-2023/schedule/)).

### TL;DR

Distribute reconciliation of Kubernetes objects across multiple controller instances.
Remove the limitation to have only one active replica (leader) per controller.

### Motivation

Typically, [Kubernetes controllers](https://kubernetes.io/docs/concepts/architecture/controller/) use a leader election mechanism to determine a *single* active controller instance (leader).
When deploying multiple instances of the same controller, there will only be one active instance at any given time, other instances will be on standby.
This is done to prevent controllers from performing uncoordinated and conflicting actions (reconciliations).

If the current leader goes down and loses leadership (e.g. network failure, rolling update) another instance takes over leadership and becomes the active instance.
Such a setup can be described as an "active-passive HA setup". It minimizes "controller downtime" and facilitates fast failovers.
However, it cannot be considered as "horizontal scaling" as work is not distributed among multiple instances.

This restriction imposes scalability limitations for Kubernetes controllers.
I.e., the rate of reconciliations, amount of objects, etc. is limited by the machine size that the active controller runs on and the network bandwidth it can use.
In contrast to usual stateless applications, one cannot increase the throughput of the system by adding more instances (scaling horizontally) but only by using bigger instances (scaling vertically).

This study project presents a design that allows distributing reconciliation of Kubernetes objects across multiple controller instances.
It applies proven sharding mechanisms used in distributed databases to Kubernetes controllers to overcome the restriction of having only one active replica per controller.
The sharding design is implemented in a generic way in my [fork](https://github.com/timebertt/controller-runtime/tree/sharding) of the Kubernetes [controller-runtime](https://github.com/kubernetes-sigs/controller-runtime) project.
The [webhosting-operator](#webhosting-operator) is implemented as an example operator that uses the sharding implementation for demonstration and evaluation.
These are the first steps toward horizontally scalable Kubernetes controllers.

## Presenting and Editing the Slides

Slides are built in Markdown using [reveal.js](https://revealjs.com/), packaged with [webpack](https://webpack.js.org/), and deployed with [netlify](https://www.netlify.com/).

### Prerequisites

Install a recent `node` version. Preferably, the one specified in [`.node-version`](./.node-version).

```bash
brew install node
```

### Present Locally

Perform a production build and serve the slides from the `dist` folder:

```bash
NODE_ENV=production npm run build
npm run serve
```

Important: Set `NODE_ENV=production` to yield the same build outputs as in production deploys to netlify.
If you don't set it, the QR will link to a local IP instead of the canonical URL, for example.

### Edit Locally

Run a dev server with hot-reload and open the slides in the browser:

```bash
npm start
```

Alternatively, use the preconfigured `start` run configuration for JetBrains IDEs.

Now, start editing the [content](./content) files.
When saving, slides are automatically rebuilt and refreshed in the browser.

> Note, that `npm start` doesn't write the output to `dist`.

### Build Locally

Run a full build and write output files to `dist`:

```bash
npm run build
```

Now, output files can be inspected in the `dist` folder.
Also, the slides can be served locally from the `dist` folder (no hot-reload):

```bash
npm run serve
```

Using the above will output non-minimized files.
Set `NODE_ENV=production` to enable minimization as it is done in netflify builds:

```bash
NODE_ENV=production npm run build
```

## Netlify Deploys

Netlify builds and publishes new commits to the `master` branch on https://talk-controller-sharding-lightning.netlify.app/.

https://github.com/timebertt/talks contains a [netlify proxy configuration](https://github.com/timebertt/talks/blob/master/netlify.toml) to make the slides available at https://talks.timebertt.dev/controller-sharding-lightning/.

The netlify site is configured to publish deploy previews for pull requests to the `master` branch and for pushes to arbitrary other branches.
