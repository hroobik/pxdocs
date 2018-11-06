---
title: Upgrade Portworx on DCOS
weight: 2
linkTitle: Upgrade
---

This guide walks through upgrading Portworx deployed on DCOS through the framework available in the DCOS
catalog. If you are using a Portworx framework version older than 1.2*, refer [this guide to perform the
Portworx [upgrade](/install-with-other/dcos/operate-and-maintain/upgrade).

### Update the Portworx image from UI

The Portworx image to be used on each node is specified by the config variable `portworx image` under `node` section.
To upgrade to a newer version, you just need to update this field to the desired version. For example, if you want
to upgrade to v1.3.1.4 you would set this to `portworx/px-enterprise:1.3.1.4`.

![Portworx image option](/img/dcos-px-image-option2.png)

### Update the Portworx image using DCOS CLI

If you want to update Portworx using the DCOS CLI instead of the UI, you can perform the following steps:

```bash
dcos portworx describe | \
  jq '.node.portworx_image="portworx/px-enterprise:1.3.1.4"' > \
  new-options.json

dcos portworx update start --options=new-options.json
```

Now wait for the portworx install tasks to go to COMPLETE state on all the agents
```bash
dcos portworx --name=portworx update status
deploy (serial strategy) (COMPLETE)
└─ portworx-install (serial strategy) (COMPLETE)
   ├─ portworx-0:[install] (COMPLETE)
   ├─ portworx-1:[install] (COMPLETE)
   ├─ portworx-2:[install] (COMPLETE)
   ├─ portworx-3:[install] (COMPLETE)
   └─ portworx-4:[install] (COMPLETE)
```
At this point your Portworx cluster should be upgraded to the specified version.