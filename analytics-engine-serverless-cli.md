---

copyright:
  years: 2015, 2025
lastupdated: "2023-02-15"

subcollection: AnalyticsEngine

keywords: Analytics Engine CLI, Analytics Engine command line , Analytics Engine terminal, Analytics Engine shell, Spark, Spark CLI

content-type: cli-docs

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# {{site.data.keyword.iae_full_notm}} CLI plug-in for serverless instances
{: #CLI_analytics_engine}

The {{site.data.keyword.iae_full_notm}} v3 CLI provides command line options to interact with `Standard Serverless Spark` instances. You can manage instances and Spark applications with the CLI. For more information about serverless Analytics Engine instances, see [Architecture and concepts in serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts).

To run {{site.data.keyword.iae_full_notm}} v3 CLI commands, use `ibmcloud analytics-engine-v3` or `ibmcloud ae-v3`.
{: tip}

## Prerequisites
{: #prerequisites}

Download and install the {{site.data.keyword.Bluemix_notm}} CLI on your local system. See [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli) to install the CLI.

## Install the {{site.data.keyword.iae_full_notm}} v3 CLI
{: #installation}

Install the {{site.data.keyword.iae_full_notm}} v3 CLI by running the following command:
```sh
ibmcloud plugin install analytics-engine-v3
```
{: pre}

The following help CLI command shows that the v3 CLI supports CLI commands for:
- [Setting a target instance](#analytics-engine-v3-target-cli)
- [Instance management](#analytics-engine-v3-instance-cli)
- [Spark application management](#analytics-engine-v3-spark-app-cli)
- [Log forwarding configuration](#analytics-engine-v3-log-forwarding-config-cli)
- [Spark history server management](#analytics-engine-v3-history-server-cli)

```sh
$ ibmcloud ae-v3 --help

NAME:
  analytics-engine-v3 - Manage serverless Spark instances and run applications.

USAGE:
  ibmcloud analytics-engine-v3 [command] [options]

ALIASES:
  analytics-engine-v3, ae-v3

COMMANDS:
  history-server          Commands for HistoryServer resource.
  instance                Commands for Instance resource.
  log-forwarding-config   Commands for LogForwardingConfig resource.
  spark-app               Commands for SparkApp resource.
  target                  All commands run after setting a target will be run on the target instance

OPTIONS:
  -h, --help                Show help
  -j, --jmes-query string   Provide a JMESPath query to customize output.
      --output string       Choose an output format - can be 'json', 'yaml', or 'table'. (default "table")
  -q, --quiet               Suppresses verbose messages.
  -v, --version             version for analytics-engine-v3

Use "ibmcloud analytics-engine-v3 service-command --help" for more information about a command.
```

## Set a target instance
{: #analytics-engine-v3-target-cli}

The following command allows you to set a target instance against which all subsequent CLI commands are run.

Note: If the instance ID is passed explicitly in any v3 CLI commands, it will override the instance ID that is set as the target.

```sh
ibmcloud analytics-engine-v3 target [--instance-id INSTANCE-ID] [--unset-instance]
```

### Command options
{: #command-for-target}

-i, --instance-id
:   The identifier of the target Analytics Engine instance. All subsequent commands are run against the target instance.

--unset-instance
:   Revoke the target instance setting


### Examples
{: #example-for-target}

The following example shows you how to set a target instance:

```sh
$ ibmcloud ae-v3 target --instance-id d2189b-b729-61f-adc0-987001da52e3
...

Target instance d0f582f7-cbf4-4c64-bf7f-72efd21c9f32
Subsequent commands will be run against the target instance
```
{: pre}

The following example shows you how to revoke a target instance:

```sh
$ ibmcloud ae-v3 target --unset-instance
...

Target instance d0f582f7-cbf4-4c64-bf7f-72efd21c9f32 revoked
No target instance is set
```
{: pre}

## Instance management commands
{: #analytics-engine-v3-instance-cli}

Use this command for Instance management.

```sh
ibmcloud analytics-engine-v3 instance --help
```

### `ibmcloud analytics-engine-v3 instance show`
{: #analytics-engine-v3-cli-instance-show-command}

Retrieve the details of a single Analytics Engine instance.

```sh
ibmcloud analytics-engine-v3 instance show [--id ID]
```


#### Command options
{: #analytics-engine-v3-instance-show-cli-options}

`-i`, `--id` (string)
:   GUID of the Analytics Engine service instance to retrieve. When specified, it overrides the target instance ID if it was set. Required if target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-instance-show-examples}

The following example shows how to use the `show` command without an instance ID. The requested operation is performed on the target instance.
```sh
ibmcloud analytics-engine-v3 instance show --output json
```
{: pre}

The following example shows how to use the `show` command with an instance ID that overrides the target instance ID if defined:
```sh
ibmcloud analytics-engine-v3 instance show \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-show-cli-output}

The output is as follows:

```json
{
  "id" : "dc0e9889-eab2-4t9e-9441-566209499546",
  "state" : "active",
  "state_change_time" : "2021-04-21T04:24:01Z",
  "default_runtime" : {
    "spark_version" : "3.3"
  },
  "instance_home" : {
    "guid" : "30d5c4a7-9fb7-4712-9039-a79417dec87b",
    "provider" : "ibm-cos",
    "type" : "objectstore",
    "region" : "us-south",
    "endpoint" : "s3.direct.us-south.cloud-object-storage.appdomain.cloud",
    "bucket" : "ae-bucket-do-not-delete-dc0e9889-eab2-4t9e-9441-566209499546",
    "hmac_access_key" : "eH****g=",
    "hmac_secret_key" : "4d********76"
  },
  "default_config" : {
    "spark.driver.memory" : "4g",
    "spark.driver.cores" : 1
  }
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance state`
{: #analytics-engine-v3-cli-instance-state-command}

Retrieve the state of a single Analytics Engine instance.

```sh
ibmcloud analytics-engine-v3 instance state [--id ID]
```


#### Command options
{: #analytics-engine-v3-instance-state-cli-options}

`-i`, `--id` (string)
:   GUID of the Analytics Engine service instance to retrieve state. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-instance-state-examples}

```sh
ibmcloud analytics-engine-v3 instance state --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance state \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-state-cli-output}



```json
{
  "id" : "dc0e9889-eab2-4t9e-9441-566209499546",
  "state" : "active"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance home-set`
{: #analytics-engine-v3-cli-instance-home-set-command}

Provide the details of the Cloud Object Storage instance to associate with the Analytics Engine instance and use as 'instance home' if 'instance home' has not already been set.

Note: You can set 'instance home' again if the instance is in 'instance_home_creation_failure' state.

```sh
ibmcloud analytics-engine-v3 instance home-set [--id ID] [--instance-home-id INSTANCE-HOME-ID] [--endpoint ENDPOINT] [--hmac-access-key HMAC-ACCESS-KEY] [--hmac-secret-key HMAC-SECRET-KEY]
```


#### Command options
{: #analytics-engine-v3-instance-home-set-cli-options}

`-i`, `--id` (string)
:   The ID of the Analytics Engine instance for which 'instance home' is to be set. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--instance-home-id` (string)
:   UUID of the instance home storage instance.

`--endpoint` (string)
:   Endpoint to access the Cloud Object Storage instance.

    The default value is `s3.direct.us-south.cloud-object-storage.appdomain.cloud`.

`--hmac-access-key` (string)
:   Cloud Object Storage access key. Required.

`--hmac-secret-key` (string)
:   Cloud Object Storage secret key. Required.

#### Examples
{: #analytics-engine-v3-instance-home-set-examples}

```sh
ibmcloud analytics-engine-v3 instance home-set \
    --hmac-access-key=821**********0ae \
    --hmac-secret-key=03e****************4fc3 \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance home-set \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --instance-home-id=701549e6-ab7e-43f2-8b7e-742698c53db8 \
    --hmac-access-key=821**********0ae \
    --hmac-secret-key=03e****************4fc3 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-home-set-cli-output}

The output is as follows:
```json
{
  "instance_id" : "701549e6-ab7e-43f2-8b7e-742698c53db8",
  "region" : "us-south",
  "endpoint" : "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud",
  "hmac_access_key" : "b9**********4b",
  "hmac_secret_key" : "fa******************8a"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance default-configs`
{: #analytics-engine-v3-cli-instance-default-configs-command}

Get the default Spark configuration properties that will be applied to all applications of the instance.

```sh
ibmcloud analytics-engine-v3 instance default-configs [--id ID]
```


#### Command options
{: #analytics-engine-v3-instance-default-configs-cli-options}

`-i`, `--id` (string)
:   The ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-instance-default-configs-examples}

```sh
ibmcloud analytics-engine-v3 instance default-configs --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance default-configs \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-default-configs-cli-output}

The output is as follows:
```json
{
  "spark.driver.memory" : "4G",
  "spark.driver.cores" : "1"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance default-configs-replace`
{: #analytics-engine-v3-cli-instance-default-configs-replace-command}

Replace the default Spark configuration properties that will be applied to all applications of the instance.

```sh
ibmcloud analytics-engine-v3 instance default-configs-replace [--id ID] --body BODY
```


#### Command options
{: #analytics-engine-v3-instance-default-configs-replace-cli-options}

`-i`, `--id` (string)
:   The ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--body` (map[string]string)
:   Spark configuration properties to replace existing instance default Spark configurations. Required.

#### Examples
{: #analytics-engine-v3-instance-default-configs-replace-examples}

```sh
ibmcloud analytics-engine-v3 instance default-configs-replace \
    --body='{"spark.driver.memory" : "8G", "spark.driver.cores" : "2"}' \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance default-configs-replace \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --body='{"spark.driver.memory" : "8G", "spark.driver.cores" : "2"}' \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-default-configs-replace-cli-output}

The output is as follows:
```json
{
  "spark.driver.memory" : "8G",
  "spark.driver.cores" : "2"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance default-configs-update`
{: #analytics-engine-v3-cli-instance-default-configs-update-command}

Update the default Spark configuration properties that will be applied to all applications of the instance.

```sh
ibmcloud analytics-engine-v3 instance default-configs-update [--id ID] --body BODY
```


#### Command options
{: #analytics-engine-v3-instance-default-configs-update-cli-options}

`-i`, `--id` (string)
:   The ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--body` (generic map)
:   Spark configuration properties to be updated. Properties will be merged with existing configuration properties. Set a property value to `null` in order to unset it. Required.

#### Examples
{: #analytics-engine-v3-instance-default-configs-update-examples}

```sh
ibmcloud analytics-engine-v3 instance default-configs-update \
    --body='{"ae.spark.history-server.cores" : "1", "ae.spark.history-server.memory" : "4G"}' \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance default-configs-update \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --body='{"ae.spark.history-server.cores" : "1", "ae.spark.history-server.memory" : "4G"}' \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-default-configs-update-cli-output}

The output is as follows:
```json
{
  "ae.spark.history-server.cores" : "1",
  "ae.spark.history-server.memory" : "4G",
  "spark.driver.memory" : "8G",
  "spark.driver.cores" : "2"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance default-runtime`
{: #analytics-engine-v3-cli-instance-default-runtime-command}

Get the default runtime environment on which all workloads of the instance will run.

```sh
ibmcloud analytics-engine-v3 instance default-runtime [--id ID]
```


#### Command options
{: #analytics-engine-v3-instance-default-runtime-cli-options}

`-i`, `--id` (string)
:   The ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-instance-default-runtime-examples}

```sh
ibmcloud analytics-engine-v3 instance default-runtime --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance default-runtime \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-default-runtime-cli-output}

The output is as follows:
```json
{
  "spark_version" : "3.3"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance default-runtime-replace`
{: #analytics-engine-v3-cli-instance-default-runtime-replace-command}

Replace the default runtime environment on which all workloads of the instance will run.

```sh
ibmcloud analytics-engine-v3 instance default-runtime-replace [--id ID] --runtime-spark-version RUNTIME-SPARK-VERSION
```


#### Command options
{: #analytics-engine-v3-instance-default-runtime-replace-cli-options}

`-i`, `--id` (string)
:   The ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--runtime-spark-version` (string)
:   Spark version of the runtime environment. Required.

    Allowable values are: `3.1`, `3.3`, `3.4`.

#### Examples
{: #analytics-engine-v3-instance-default-runtime-replace-examples}

```sh
ibmcloud analytics-engine-v3 instance default-runtime-replace \
    --runtime-spark-version=3.3 \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance default-runtime-replace \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --runtime-spark-version=3.3 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-default-runtime-replace-cli-output}

The output is as follows:
```json
{
  "spark_version" : "3.3"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance current-resource-consumption`
{: #analytics-engine-v3-cli-instance-current-resource-consumption-command}

Provides the total memory and virtual processor cores allocated to all the applications running in the service instance at this point in time. When auto-scaled applications are running, the resources allotted changes over time, based on the applications's demands.

Note: The consumption is not an indication of actual resource consumption by Spark processes. It is the sum of resources allocated to the currently running applications at the time of application submission.

```sh
ibmcloud analytics-engine-v3 instance current-resource-consumption [--id ID]
```


#### Command options
{: #analytics-engine-v3-instance-current-resource-consumption-cli-options}

`-i`, `--id` (string)
:   ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-instance-current-resource-consumption-examples}

```sh
ibmcloud analytics-engine-v3 instance current-resource-consumption --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance current-resource-consumption \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-current-resource-consumption-cli-output}

The output is as follows:
```json
{
  "cores" : "2",
  "memory" : "8G"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 instance resource-consumption-limits`
{: #analytics-engine-v3-cli-instance-resource-consumption-limits-command}

Returns the maximum total memory and virtual processor cores allocated across all the applications running in the service instance at any point in time.

```sh
ibmcloud analytics-engine-v3 instance resource-consumption-limits [--id ID]
```


#### Command options
{: #analytics-engine-v3-instance-resource-consumption-limits-cli-options}

`-i`, `--id` (string)
:   ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-instance-resource-consumption-limits-examples}

```sh
ibmcloud analytics-engine-v3 instance resource-consumption-limits --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 instance resource-consumption-limits \
    --id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-instance-resource-consumption-limits-cli-output}

The output is as follows:
```json
{
  "max_cores" : "150",
  "max_memory" : "600G"
}
```
{: screen}

## Spark application commands
{: #analytics-engine-v3-spark-app-cli}

With the Spark application CLI commands, you can perform the following operations:

-	[Submit a Spark application](#analytics-engine-v3-cli-spark-app-submit-command)
-	[List all applications submitted](#analytics-engine-v3-cli-spark-app-list-command)
-	[Retrieve the details of a Spark application](#analytics-engine-v3-cli-spark-app-show-command)
- [Show the status of a submitted application](#analytics-engine-v3-cli-spark-app-status-command)
-	[Stop a submitted Spark application](#analytics-engine-v3-cli-spark-app-stop-command)

```sh
ibmcloud analytics-engine-v3 spark-app --help
```

### `ibmcloud analytics-engine-v3 spark-app submit`
{: #analytics-engine-v3-cli-spark-app-submit-command}

Deploys a Spark application on a given serverless Spark instance.

```sh
ibmcloud analytics-engine-v3 spark-app submit [--instance-id INSTANCE-ID] [--app APP] [--runtime RUNTIME] [--jars JARS] [--packages PACKAGES] [--repositories REPOSITORIES] [--files FILES] [--archives ARCHIVES] [--name NAME] [--class CLASS] [--arg ARG] [--conf CONF] [--env ENV]
```


#### Command options
{: #analytics-engine-v3-spark-app-submit-cli-options}

`-i`, `--instance-id` (string)
:   The identifier of the Analytics Engine instance associated with the Spark application(s). When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--app` (string)
:   Path of the application to run. Required.

`--runtime` ([`Runtime`](#cli-runtime-example-schema))
:   Runtime enviroment for applications and other workloads.

`--jars` (string)
:   Path of the jar files containing the application.

`--packages` (string)
:   Package names.

`--repositories` (string)
:   Repositories names.

`--files` (string)
:   File names.

`--archives` (string)
:   Archive Names.

`--name` (string)
:   Name of the application.

`--class` (string)
:   Entry point for a Spark application bundled as a '.jar' file. This is applicable only for Java or Scala applications.

    The value must match the regular expression `/([\\p{L}_$][\\p{L}\\p{N}_$]*\\.)*[\\p{L}_$][\\p{L}\\p{N}_$]*/`.

`--arg` ([]string)
:   An array of arguments to be passed to the application.

`--conf` (generic map)
:   Application configurations to override the value specified at instance level. See [Spark environment variables](https://spark.apache.org/docs/latest/configuration.html#available-properties) for a list of the supported variables.

`--env` (generic map)
:   Application environment configurations to use. See [Spark environment variables](https://spark.apache.org/docs/latest/configuration.html#environment-variables) for a list of the supported variables.

#### Examples
{: #analytics-engine-v3-spark-app-submit-examples}

```sh
ibmcloud analytics-engine-v3 spark-app submit \
    --arg "/opt/ibm/spark/examples/src/main/resources/people.txt" \
    --app "/opt/ibm/spark/examples/src/main/python/wordcount.py" \
    --runtime='{"spark_version": "3.3"}' \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 spark-app submit \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --arg "/opt/ibm/spark/examples/src/main/resources/people.txt" \
    --app "/opt/ibm/spark/examples/src/main/python/wordcount.py" \
    --runtime='{"spark_version": "3.3"}' \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-spark-app-submit-cli-output}

The output is as follows:
```json
{
  "id" : "87e63712-a823-4aa1-9f6e-7291d4e5a113",
  "state" : "accepted"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 spark-app list`
{: #analytics-engine-v3-cli-spark-app-list-command}

Returns a list of all Spark application submitted to the specified Analytics Engine instance. The result can be filtered by specifying query parameters. The number of applications returned can be limited by specifying the `limit` query parameter. Use `start` together with `limit` to fetch the next or previous page of results.

```sh
ibmcloud analytics-engine-v3 spark-app list [--instance-id INSTANCE-ID] [--state STATE] [--limit LIMIT] [--start START]
```


#### Command options
{: #analytics-engine-v3-spark-app-list-cli-options}

`-i`, `--instance-id` (string)
:   The identifier of the Analytics Engine instance associated with the Spark application(s). Required.

    The value must match regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--state` ([]string)
:   List of Spark application states that will be used to filter the response.

    Allowable list items are: `finished`, `running`, `failed`, `accepted`, `stopped`, `auto_terminated`, `ops_terminated`.

`--limit` (int64)
:   Number of application entries to be included in the response.

    The maximum value is `1000`. The minimum value is `1`.

`--start` (string)
:   Token used to fetch the next or the previous page of the applications list.

`--all-pages` (bool)
:   Invoke multiple requests to display all pages of the collection for spark-app-list.

#### Examples
{: #analytics-engine-v3-spark-app-list-examples}

```sh
ibmcloud analytics-engine-v3 spark-app list \
    --state=finished \
    --limit=25
```
{: pre}

```sh
ibmcloud analytics-engine-v3 spark-app list \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --state=finished \
    --limit=25
```
{: pre}

#### Example output
{: #analytics-engine-v3-spark-app-list-cli-output}

The output is as follows:
```json
{
  "applications" : [ {
    "id" : "db933645-0b68-4dcb-80d8-7b71a6c8e542",
    "spark_application_id" : "app-20211009103247-0000",
    "spark_application_name" : "PythonWordCount",
    "state" : "finished",
    "submission_time" : "2021-04-21T04:23:50Z",
    "start_time" : "2021-04-21T04:24:01Z",
    "end_time" : "2021-04-21T04:25:18Z",
    "finish_time" : "2021-04-21T04:25:18Z",
    "auto_termination_time" : "2021-04-24T04:24:01Z"
  }, {
    "id" : "a2a2b23f-0929-4c49-9cc0-bd4c2bf953d9",
    "spark_application_id" : "app-20211009103147-0000",
    "spark_application_name" : "PythonWordCount",
    "state" : "finished",
    "submission_time" : "2021-04-21T04:21:50Z",
    "start_time" : "2021-04-21T04:22:01Z",
    "end_time" : "2021-04-21T04:23:18Z",
    "finish_time" : "2021-04-21T04:23:18Z",
    "auto_termination_time" : "2021-04-24T04:22:01Z"
  } ],
  "first" : {
    "href" : "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/e64c907a-e82f-46fd-addc-ccfafbd28b09/spark_applications?limit=25"
  },
  "next" : {
    "href" : "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/e64c907a-e82f-46fd-addc-ccfafbd28b09/spark_applications?limit=25&start=QiwyMDIyLTA5LTI2IDA4OjE4OjU2LjE4MiwxMDAyYTVlZC0xZWU4LTQwZWItOWUyNC00OTMyNTcxZjgzYzE",
    "start" : "QiwyMDIyLTA5LTI2IDA4OjE4OjU2LjE4MiwxMDAyYTVlZC0xZWU4LTQwZWItOWUyNC00OTMyNTcxZjgzYzE"
  },
  "limit": 25
}
```
{: screen}

### `ibmcloud analytics-engine-v3 spark-app show`
{: #analytics-engine-v3-cli-spark-app-show-command}

Gets the details of a given Spark application.

```sh
ibmcloud analytics-engine-v3 spark-app show [--instance-id INSTANCE-ID] --app-id APP-ID
```


#### Command options
{: #analytics-engine-v3-spark-app-show-cli-options}

`-i`, `--instance-id` (string)
:   Identifier of the instance to which the application belongs. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--app-id` (string)
:   Identifier of the application for which details are requested. Required.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-spark-app-show-examples}

```sh
ibmcloud analytics-engine-v3 spark-app show \
    --app-id=ff48cc19-0e7e-4627-aac6-0b4ad080397b \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 spark-app show \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --app-id=ff48cc19-0e7e-4627-aac6-0b4ad080397b \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-spark-app-show-cli-output}

The output is as follows:
```json
{
  "application_details" : {
    "application" : "/opt/ibm/spark/examples/src/main/python/wordcount.py",
    "arguments" : [ "/opt/ibm/spark/examples/src/main/resources/people.txt" ],
    "runtime": {
      "spark_version": "3.3"
    }
  },
  "id" : "a9a6f328-56d8-4923-8042-97652fff2af3",
  "spark_application_id" : "app-20210908112356-0000",
  "spark_application_name" : "PythonWordCount",
  "state" : "finished",
  "submission_time" : "2021-04-21T04:23:55Z",
  "start_time" : "2021-04-21T04:24:01Z",
  "end_time" : "2021-04-21T04:28:15Z",
  "finish_time" : "2021-04-21T04:28:15Z",
  "auto_termination_time" : "2021-04-24T04:24:01Z"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 spark-app stop`
{: #analytics-engine-v3-cli-spark-app-stop-command}

Stops a running application identified by the app_id identifier. This is an idempotent operation. Performs no action if the requested application is already stopped or completed.

```sh
ibmcloud analytics-engine-v3 spark-app stop [--instance-id INSTANCE-ID] --app-id APP-ID
```


#### Command options
{: #analytics-engine-v3-spark-app-stop-cli-options}

`-i`, `--instance-id` (string)
:   Identifier of the instance to which the application belongs. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--app-id` (string)
:   Identifier of the application that needs to be stopped. Required.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-spark-app-stop-examples}

```sh
ibmcloud analytics-engine-v3 spark-app stop \
    --app-id=ff48cc19-0e7e-4627-aac6-0b4ad080397b
```
{: pre}

```sh
ibmcloud analytics-engine-v3 spark-app stop \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --app-id=ff48cc19-0e7e-4627-aac6-0b4ad080397b
```
{: pre}

### `ibmcloud analytics-engine-v3 spark-app status`
{: #analytics-engine-v3-cli-spark-app-status-command}

Returns the status of the given Spark application.

```sh
ibmcloud analytics-engine-v3 spark-app status [--instance-id INSTANCE-ID] --app-id APP-ID
```


#### Command options
{: #analytics-engine-v3-spark-app-status-cli-options}

`-i`, `--instance-id` (string)
:   Identifier of the instance to which the applications belongs. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--app-id` (string)
:   Identifier of the application for which details are requested. Required.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-spark-app-status-examples}

```sh
ibmcloud analytics-engine-v3 spark-app status \
    --app-id=ff48cc19-0e7e-4627-aac6-0b4ad080397b \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 spark-app status \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --app-id=ff48cc19-0e7e-4627-aac6-0b4ad080397b \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-spark-app-status-cli-output}

The output is as follows:
```json
{
  "id" : "9da32aaf-df69-4e61-bdb8-1b2772c0f677",
  "state" : "finished",
  "start_time" : "2021-04-21T04:24:01Z",
  "end_time" : "2021-04-21T04:28:15Z",
  "finish_time" : "2021-04-21T04:28:15Z",
  "auto_termination_time" : "2021-04-24T04:24:01Z"
}
```
{: screen}

## Log forwarding configuration
{: #analytics-engine-v3-log-forwarding-config-cli}

Use this command to enable or disable forwarding of logs from a serverless instance to an IBM Log Analysis service and to view the status of the log forwarding configuration.

```sh
ibmcloud analytics-engine-v3 log-forwarding-config --help
```


### `ibmcloud analytics-engine-v3 log-forwarding-config replace`
{: #analytics-engine-v3-cli-log-forwarding-config-replace-command}

Modify the configuration for forwarding logs from the Analytics Engine instance to IBM Log Analysis server. Use this endpoint to enable or disable log forwarding.

```sh
ibmcloud analytics-engine-v3 log-forwarding-config replace [--instance-id INSTANCE-ID] --enabled ENABLED [--sources SOURCES] [--tags TAGS]
```


#### Command options
{: #analytics-engine-v3-log-forwarding-config-replace-cli-options}

`-i`, `--instance-id` (string)
:   ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--enabled` (bool)
:   Enable or disable log forwarding. Required.

`--sources` ([]string)
:   List of sources of logs that will be forwarded. By default, only 'spark-driver' logs are forwarded.

`--tags` ([]string)
:   List of tags to be applied to the logs being forwarded. They can be used to filter the logs in the IBM Log Analysis server.

#### Examples
{: #analytics-engine-v3-log-forwarding-config-replace-examples}

```sh
ibmcloud analytics-engine-v3 log-forwarding-config replace \
    --enabled=true \
    --sources=spark-driver,spark-executor \
    --tags=<tag_1>,<tag_2>,<tag_n> \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 log-forwarding-config replace \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --enabled=true \
    --sources=spark-driver,spark-executor \
    --tags=<tag_1>,<tag_2>,<tag_n> \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-log-forwarding-config-replace-cli-output}

The output is as follows:
```json
{
  "sources" : [ "spark-driver", "spark-executor" ],
  "log_server" : {
    "type" : "ibm-log-analysis"
  },
  "enabled" : true
}
```
{: screen}

### `ibmcloud analytics-engine-v3 log-forwarding-config show`
{: #analytics-engine-v3-cli-log-forwarding-config-show-command}

Retrieve the log forwarding configuration of the Analytics Engine instance.

```sh
ibmcloud analytics-engine-v3 log-forwarding-config show [--instance-id INSTANCE-ID]
```


#### Command options
{: #analytics-engine-v3-log-forwarding-config-show-cli-options}

`-i`, `--instance-id` (string)
:   ID of the Analytics Engine instance. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-log-forwarding-config-show-examples}

```sh
ibmcloud analytics-engine-v3 log-forwarding-config show --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 log-forwarding-config show \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-log-forwarding-config-show-cli-output}

The output is as follows:
```json
{
  "sources" : [ "spark-driver", "spark-executor" ],
  "log_server" : {
    "type" : "ibm-log-analysis"
  },
  "enabled" : true
}
```
{: screen}

## Spark history server
{: #analytics-engine-v3-history-server-cli}

Use these commands to start or stop the Spark history server of your instance.

```sh
ibmcloud analytics-engine-v3 history-server --help
```


### `ibmcloud analytics-engine-v3 history-server start`
{: #analytics-engine-v3-cli-history-server-start-command}

Start the Spark history server for the given Analytics Engine instance.

```sh
ibmcloud analytics-engine-v3 history-server start [--instance-id INSTANCE-ID]
```


#### Command options
{: #analytics-engine-v3-history-server-start-cli-options}

`-i`, `--instance-id` (string)
:   The ID of the Analytics Engine instance to which the Spark history server belongs. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-history-server-start-examples}

```sh
ibmcloud analytics-engine-v3 history-server start --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 history-server start \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-history-server-start-cli-output}

The output is as follows:
```json
{
  "state" : "started",
  "cores" : "1",
  "memory" : "4G",
  "start_time" : "2022-02-21T07:37:47Z",
  "auto_termination_time" : "2022-02-24T07:37:47Z"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 history-server show`
{: #analytics-engine-v3-cli-history-server-show-command}

Get the details of the Spark history server of the given Analytics Engine instance.

```sh
ibmcloud analytics-engine-v3 history-server show [--instance-id INSTANCE-ID]
```


#### Command options
{: #analytics-engine-v3-history-server-show-cli-options}

`-i`, `--instance-id` (string)
:   The ID of the Analytics Engine instance to which the Spark history server belongs. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-history-server-show-examples}

```sh
ibmcloud analytics-engine-v3 history-server show --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 history-server show \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-history-server-show-cli-output}

The output is as follows:
```json
{
  "state" : "started",
  "cores" : "1",
  "memory" : "4G",
  "start_time" : "2022-02-21T07:37:47Z",
  "auto_termination_time" : "2022-02-24T07:37:47Z"
}
```
{: screen}

### `ibmcloud analytics-engine-v3 history-server stop`
{: #analytics-engine-v3-cli-history-server-stop-command}

Stop the Spark history server of the given Analytics Engine instance.

```sh
ibmcloud analytics-engine-v3 history-server stop [--instance-id INSTANCE-ID]
```


#### Command options
{: #analytics-engine-v3-history-server-stop-cli-options}

`-i`, `--instance-id` (string)
:   The ID of the Analytics Engine instance to which the Spark history server belongs. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-history-server-stop-examples}

```sh
ibmcloud analytics-engine-v3 history-server stop --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 history-server stop \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

## Log configuration
{: #analytics-engine-v3-log-config-cli}

Note: Deprecated. Use equivalent commands from the [log-forwarding-config](#analytics-engine-v3-log-forwarding-config-cli) command group instead.

```sh
ibmcloud analytics-engine-v3 log-config --help
```


### `ibmcloud analytics-engine-v3 log-config update`
{: #analytics-engine-v3-cli-log-config-update-command}

Enable or disable log forwarding from IBM Analytics Engine to IBM Log Analysis server.

Note: Deprecated. Use `replace` from the `log-forwarding-config` command group instead.

```sh
ibmcloud analytics-engine-v3 log-config update [--instance-id INSTANCE-ID] [--enable ENABLE]
```


#### Command options
{: #analytics-engine-v3-log-config-update-cli-options}

`-i`, `--instance-id` (string)
:   GUID of the instance details for which log forwarding is to be configured. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

`--enable` (bool)
:   Enable or disable log forwarding. Required.

#### Examples
{: #analytics-engine-v3-log-config-update-examples}

```sh
ibmcloud analytics-engine-v3 log-config update \
    --enable=true \
    --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 log-config update \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --enable=true \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-log-config-update-cli-output}

The output is as follows:
```json
{
  "components" : [ "spark-driver", "spark-executor" ],
  "log_server" : {
    "type" : "ibm-log-analysis"
  },
  "enable" : true
}
```
{: screen}

### `ibmcloud analytics-engine-v3 log-config show`
{: #analytics-engine-v3-cli-log-config-show-command}

Retrieve the logging configuration of a given Analytics Engine instance.

Note: Deprecated. Use `show` from the `log-forwarding-config` command group instead.

```sh
ibmcloud analytics-engine-v3 log-config show [--instance-id INSTANCE-ID]
```


#### Command options
{: #analytics-engine-v3-log-config-show-cli-options}

`-i`, `--instance-id` (string)
:   GUID of the Analytics Engine service instance to retrieve log configuration. When specified, it overrides the target instance ID if it was set. Required, if the target instance was not set.

    The value must match the regular expression `/\u0008[0-9a-f]{8}\u0008-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-\u0008[0-9a-f]{12}\u0008/`.

#### Examples
{: #analytics-engine-v3-log-config-show-examples}

```sh
ibmcloud analytics-engine-v3 log-config show --output json
```
{: pre}

```sh
ibmcloud analytics-engine-v3 log-config show \
    --instance-id=e64c907a-e82f-46fd-addc-ccfafbd28b09 \
    --output json
```
{: pre}

#### Example output
{: #analytics-engine-v3-log-config-show-cli-output}

The output is as follows:
```json
{
  "components" : [ "spark-driver", "spark-executor" ],
  "log_server" : {
    "type" : "ibm-log-analysis"
  },
  "enable" : true
}
```
{: screen}


## Schema examples
{: #analytics-engine-v3-schema-examples}

The following schema example represents the data required to specify command option. These example model the data structure and include placeholder values for the expected value type. When you run a command, replace these values with the values that apply to your environment as appropriate.

### Runtime
{: #cli-runtime-example-schema}

The following example shows the format of the Runtime object.

```json
{
  "spark_version" : "3.3"
}
```
{: screen}
