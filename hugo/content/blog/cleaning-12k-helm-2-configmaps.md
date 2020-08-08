---
draft: false
date: 2020-07-27T12:51:00-07:00
title: "Cleaning 12k Helm 2 Configmaps"
description: "Remember to clean your Helm 2 Configmaps by setting the TILLER_HISTORY_MAX"
image: "images/featured-post/garbage-collection.jpg"
type: "featured"

categories:
  - "DevOps"
tags:
  - "Kubernetes"
  - "Helm"
  - "Garbage Collection"
---

---

__If you've ran Helm in a development k8s cluster for over a year and weren't aware of `TILLER_HISTORY_MAX`, you're not alone 🤣🤣__

### Introduction

I'm Ryan and I help build and maintain the DevOps infrastructure at [Calm](https://calm.com) for the past 1.5 years. We run Kubernetes in AWS EKS and deploy via [Helmfile](https://github.com/roboll/helmfile).

Here's a quick post about a Helm 2 issue we ran into with our development k8s cluster.

### 'Twas a Normal Day Until..

Last week, everything was running smoothly until we started noticing failing deployments and complaints from developers. The initial obsevation in the logs was Helmfile hitting deployment timeouts and rolling back.

Digging deeper.. we checked Tiller logs and noticed error logs mentioning timeouts of `GET` & `POST` to the `api/v1/namespaces/kube-system/configmaps` endpoint:

```bash
Error: Get https://172.20.0.1:443/api/v1/namespaces/kube-system/configmaps?labelSelector=OWNER%!D(MISSING)TILLER: read tcp 10.1.123.123:48172->172.20.0.1:443: read: connection timed out
```

We also noticed `helm ls` command would fail, generating additional errors like this ^^.

However, `kubectl get cm` commands would succeed.

We figured these symptoms were a bit odd, so we shouldertapped our friendly neighborhood AWS TAM. We wanted to gather additional insight into the control plane and guidance on parsing the k8s api logs in AWS Cloudwatch. We just wanted to rule out any deep seeded issues going on. 👍

### Google to the Rescue

Of course we Googled the error and stumbled on [an open Helm issue](https://github.com/helm/helm/issues/2332) regarding this.

Lo and behold, we accumulated over 12k configmaps in `kube-system`:

```bash
-> kubectl -n kube-system get cm | wc -l
   12766
```

You'd think 10s of MB worth of configmaps wouldn't cause issues.. but hey 🤷‍♂️

### Resolution

From the Helm issue above, I [grabbed and modified an existing script](https://github.com/helm/helm/issues/2332#issuecomment-336565784) to clean up old Helm configmaps older than the last 10 versions:

{{< gist rms1000watt 626063b036c5ce81581160237a6a8475 >}}

After running this, the cluster began to behave properly once again. 😅

### Here We Are in the Future

**How can we prevent this from happening in the future?**

> "Migrate to Helm 3"

Lol, yes of course. It's backlogged and we'll get to it. 🤣

**How can we prevent this from happening in the _near_ future?**

The quick and dirty solution that we _should have already known about_ is to update the `tiller-deploy` k8s deployment to include the environment variable:

```yaml
TILLER_HISTORY_MAX=10
```

Which grants us automatic garbage collection!

### Contact & Hiring

If you've enjoyed reading this, please reach out; I'd love to hear what you're working on.

[_Please checkout Calm's career page for all available job openings_](https://boards.greenhouse.io/calm)

---