---
reviewers:
- stclair
title: AppArmor
---

{% capture overview %}

{% assign for_k8s_version="v1.4" %}{% include feature-state-beta.md %}


AppArmor is a Linux kernel security module that supplements the standard Linux user and group based
permissions to confine programs to a limited set of resources. AppArmor can be configured for any
application to reduce its potential attack surface and provide greater in-depth defense. It is
configured through profiles tuned to whitelist the access needed by a specific program or container,
such as Linux capabilities, network access, file permissions, etc. Each profile can be run in either
*enforcing* mode, which blocks access to disallowed resources, or *complain* mode, which only reports
violations.
