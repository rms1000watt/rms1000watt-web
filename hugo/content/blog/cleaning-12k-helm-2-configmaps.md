---
draft: false
date: 2020-07-27T12:51:00-07:00
title: "Cleaning 12k Helm 2 Configmaps"
description: "this is meta description"
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
If you've ran helm in a development cluster for over a year and wasn't aware of `TILLER_HISTORY_MAX`, you're not alone 🤣🤣

### Observation

At https://calm.com in one of our `dev` clusters, we accumulated around 12k configmaps related to helm versions

```bash
-> kubectl -n kube-system get cm | wc -l
   12766
```

### Problem

At this point, `helm ls` would start to fail. We noticed Tiller logs timing out to `GET` the `configmap` endpoint

```bash
Error: Get https://172.20.0.1:443/api/v1/namespaces/kube-system/configmaps?labelSelector=OWNER%!D(MISSING)TILLER: read tcp 10.1.123.123:48172->172.20.0.1:443: read: connection timed out
```

### Code

So, after some Googling, we saw other people hit similar issues and we should purge Helm configmaps. Here's the script we used to purge old Helm configmaps:

{{< gist rms1000watt 626063b036c5ce81581160237a6a8475 >}}

### Future

Yeah, we need to just migrate to Helm 3. Until then, we updated the `tiller-deploy` deployments to:

```yaml
TILLER_HISTORY_MAX=10
```

Which grants us automatic garbage collection!

---