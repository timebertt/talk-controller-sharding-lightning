# Towards Horizontally Scalable Kubernetes Controllers (Lightning Talk)

[![Netlify Status](https://api.netlify.com/api/v1/badges/e06fa269-6329-42d3-9f49-9356ee0cc204/deploy-status)](https://app.netlify.com/sites/talk-controller-sharding-lightning/deploys)

Take me to the [slides](https://talks.timebertt.dev/controller-sharding-lightning/)!

## About

This is a lightning talk by [@timebertt](https://github.com/timebertt) at [Cloud Native Rejekts 2023](https://cloud-native.rejekts.io/) in Amsterdam ([event schedule](https://cfp.cloud-native.rejekts.io/cloud-native-rejekts-eu-amsterdam-2023/schedule/)).

You can find the full project belonging to this talk here: [kubernetes-controller-sharding](https://github.com/timebertt/kubernetes-controller-sharding).

### Abstract

Kubernetes controllers use a leader election mechanism to determine a single active instance out of multiple controller replicas to prevent uncoordinated and conflicting actions.
Only the current leader performs reconciliations, other instances are on standby.
Reconciliations cannot be distributed among multiple controller instances, hence Kubernetes controllers cannot be scaled horizontally.
But how are your operators supposed to handle thousands of API objects?

As large-scale cluster and operator deployments are getting more widespread, there is an unanswered ask to overcome this scalability limitation.
This talk takes you on a journey towards horizontally scalable Kubernetes controllers by introducing a design for Kubernetes controller sharding.

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
