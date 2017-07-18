---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Troubleshooting (JKG)

## Common Issues

**The notebook user interface launches, but the kernel remains in a busy state (filled circle) before executing any cell/code.**
 
 Because we don't have the lazy spark initialization in the cluster yet, the most likely cause is that you don't have any more available YARN containers on the cluster. Verify this by checking that the 'state' of your application is 'ACCEPTED' in the YARN RM UI. To fix this issue, shut down the existing running kernels or other YARN applications to free up resources.
