---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-07"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Script based customization
{: #cust-script}

If you want to customize your instance by adding libraries to a library set that you fetch from sources other than through the `conda` or `pip` repositories, you should use script based customization.

With script based customization, you create a Python script using the module naming convention expected by {{site.data.keyword.iae_full_notm}}. Also, you need to implement a Python function that acts as the executable entry point to your script. In this script, you can add your own logic for downloading the libraries and placing them in a predesignated directory so that they become part of the library set and get stored for later consumption in your application.

## Creating a library set using script based customization
{: #lib-set-script-cust}

Perform these steps to create a library set using script based customization:
1. Create a Python file named `customization_scripts.py`. {{site.data.keyword.iae_short}}'s customization component looks for a Python module with this name.
1. In your `customization_scripts.py`, implement a function called `customize(<install_path>, <params>)`, where `<install_path>` is the location where you want to download your libraries to (you can nest directories), and `<params>` is a list of the parameters required by the customization script.
1. Store the `customization_scripts.py` file in {{site.data.keyword.cos_full_notm}} or in GitHub.
1. Pass the location of the `customization_scrpts.py` file to the customization Spark application through the `--py-files`  parameter.

   The `"arguments"` section in the Spark application submission payload must contain a `"library_set"` section with details, like `"action"` and `"name"` as shown in the following sample payload.

   Example of the payload:
   ```
   {
     "application_details": {
     "application": "/opt/ibm/customization-scripts/customize_instance_app.py",
      "arguments": ["{\"library_set\":{\"action\":\"add\",\"name\":\"customize_integration_custom_lib\",\"script\":{\"source\":\"py_files\",\"params\":[\"https://s3.private.<CHANGEME_REGION>.cloud-object-storage.appdomain.cloud\",\"<CHANGEME_BUCKET_NAME>\",\"libcalc.so\",\"<CHANGEME_ACCESS_KEY>\",\"<CHANGEME_SECRET_KEY>\"]}}}"],
      "py-files": "cos://<CHANGEME_BUCKET_NAME>.dev-cos/customization_script.py",
          "conf": {
            "spark.hadoop.fs.cos.dev-cos.endpoint":"https://s3.private.<CHANGEME_REGION>.cloud-object-storage.appdomain.cloud",
            "spark.hadoop.fs.cos.dev-cos.access.key":"<CHANGEME_ACCESS_KEY>",
            "spark.hadoop.fs.cos.dev-cos.secret.key":"<CHANGEME_SECRET_KEY>"
       }
     }
   }
   ```
   {: codeblock}

   Example of customization_script.py:
   ```
   import cos_utils
   import os

   def customize(install_path, params):
     print ("inside base install_misc_package(), override this to implement your own implementation.")
     for param in params:
      print(param)
      endpoint = params[0]
      bucket_name = params[1]
      so_file_name = params[2]
      access_key = params[3]
      secret_key = params[4]
      cos = cos_utils.get_cos_object(access_key, secret_key, endpoint)

      retCode = cos_utils.download_file(so_file_name, cos, bucket_name, "{}/{}".format(install_path, so_file_name))
      if (retCode != 0):
          print("non-zero return code while downloading file    {}".format(str(retCode)))
         sys.exit(retCode)
        else:
          print("Successfully downloaded file...")
   ```
   {: codeblock}

## Using the library set created using script based customization
{: #script-cust-using}

When you use a library set that you created using a script, you need to include the path of the library in certain environment variables so that the library set can be accessed by your Spark application.

For example, if your custom library is a `.so` file, you need to add `"EXTRA_LD_LIBRARY_PATH"` to the `"env"` section of the Spark  application submit call, with the value `/home/spark/shared/user-libs/<libraryset_name>/custom/<subdir_if_applicable>`. This  prepends `"EXTRA_LD_LIBRARY_PATH"` to `"LD_LIBRARY_PATH"`, ensuring that this is the first path to the `.so` file that is searched.
