---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-31"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Combining library sets
{: #combining-lib-sets}

You can install a Python packages via pip or conda, as well as download a file as part of the library set by combining these options when you create the library set and prepare the JSON file tp pass in the REST API call.

You can use the combination option as shown in the following example:

```json
{
	"application_details": {
		"application": "/opt/ibm/customization-scripts/customize_instance_app.py",
		"arguments": ["{\"library_set\":{\"action\":\"add\",\"name\":\"multi_library\",\"libraries\":{\"pip\":{\"python\":{\"packages\":[\"numpy\"]}}},\"script\":{\"source\":\"py_files\",\"params\":[\"script_params\"]}}}"],
		"py-files": "cos://<bucket>.dev-cos/jar/customization_script.py",
		"conf": {
			"spark.hadoop.fs.cos.dev-cos.endpoint":"https://s3.direct.us-south.cloud-object-storage.appdomain.cloud",
			"spark.hadoop.fs.cos.dev-cos.access.key":"<cos_access_key>",
			"spark.hadoop.fs.cos.dev-cos.secret.key":"<cos_secret_key>"
		}
	}
}
```
{: codeblock}


To increase readability, the unescaped JSON for the arguments would look as follows:

```json
{
    "library_set": {
        "action": "add",
        "name": "multi_library",
        "libraries": {
            "pip": {
                "python": {
                    "packages": [
                        "numpy"
                    ]
                }
            }
        },
        "script": {
            "source": "py_files",
            "params": [
                "script_params"
            ]
        }
    }
}
{: codeblock}
