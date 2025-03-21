---
title: Analyze the results
weight: 4

### FIXED, DO NOT MODIFY
layout: learningpathall
---

## Identifying issues and opportunities

After running KubeArchInspect, you can examine the output to determine if the cluster image architectures are suitable for your needs. Each image running in the cluster appears on a separate line, including name, tag (version), and test result:

* A green tick (✅) indicates the image already supports arm64.
* A red cross (❌) indicates that arm64 support is not available.
* A blue up symbol (🆙) shows that arm64 support is included in a newer version.
* A red cross mark (🚫) is shown when an error occurs checking the image. This may indicate an error connecting to the image registry.

If you want to run an all Arm cluster, you need to use images which include arm64 support. 

## Addressing issues

The KubeArchInspect report provides valuable information for improving the cluster's performance and compatibility with the Arm architecture. 

There are several approaches you can take to address issues identified in the report:

* Upgrade images: If an image with an available arm64 version (🆙) is detected, consider upgrading to that version.  You can do this by modifying the deployment configuration and restarting the containers using the new image tag. 

* Find alternative images: For images with no available arm64 version (`❌`), look for alternative images that offer arm64 support. For example, instead of a specific image from the registry, try using a more general image like `busybox`, which supports multiple architectures, including arm64.

* Request Arm support: If there is no suitable alternative image available, you can contact the image developers or the Kubernetes community and request them to build and publish an arm64 version of the image.
