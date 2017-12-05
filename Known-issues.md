---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Known issues

List of known issues in the release:

| Category | Problem | Workaround |
|------------|-----------|-----------|
| Cluster Access | Launch of Ambari Console or SSH connectivity to newly created cluster may fail due to a SoftLayer DNS issue | Allow a few minutes prior to access.|
| UI | The Cluster Management user interface does not function well in Internet Explorer 11 | The Management user interface functions fine in Chrome, Safari and Firefox. Use these browsers to access the user interface. |
| Oozie | Oozie jobs fail because of the Oozie versions used with HDP. | Perform the steps in the following [workaround](./workaround-oozie-jobs.html). |
| Customization | The package-admin can only install packages in the centos repository.
| | OS packages that are installed through the package-admin will not persist after the host is rebooted. They need to be installed again, if the host machine is rebooted.
