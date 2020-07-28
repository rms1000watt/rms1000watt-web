---
title: "Cleaning 12k Helm 2 Configmaps"
date: 2020-07-27T12:51:00-07:00
draft: false

# post thumb
image: "images/featured-post/garbage-collection.jpg"

# meta description
description: "this is meta description"

# taxonomies
categories:
  - "DevOps"
tags:
  - "Kubernetes"
  - "Helm"
  - "Garbage Collection"

# post type
type: "featured"
---

## tl;dr

Here's a quick script to clean up helm 2 configmaps if `TILLER_HISTORY_MAX=0`.

{{< gist rms1000watt 626063b036c5ce81581160237a6a8475 >}}
