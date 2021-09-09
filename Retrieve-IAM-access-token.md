---

copyright:
  years: 2017, 2019
lastupdated: "2018-11-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving IAM access tokens
{: #retrieve-iam-token}

Before you can call an {{site.data.keyword.iae_full_notm}} API, you must first have created an IAM bearer token. Each token is valid only for one hour, and after a token expires, you must create a new one if you want to continue using the API. The recommended method to retrieve a token programmatically is to create an API key for your {{site.data.keyword.Bluemix_short}} identification and then use the IAM token API to exchange that key for a token.

You can create a token in two ways:

- [By using the {{site.data.keyword.Bluemix_notm}} REST API](#create-token-with-ibm-cloud-rest-api)
- [By using the {{site.data.keyword.Bluemix_short}} CLI](#create-token-with-ibm-cloud-cli).

## Create a token using the {{site.data.keyword.Bluemix_notm}} REST API
{: #create-token-with-ibm-cloud-rest-api}

To create a token in {{site.data.keyword.Bluemix_notm}}:

1. Sign in to {{site.data.keyword.Bluemix_notm}} and select **Manage>Security>Platform API Keys**.
2. Create an API key for your own personal identity, copy the key value, and save it in a secure place. After you leave the page, you will no longer be able to access this value.
3. Use the API key you created with the IAM identity token API to generate an IAM token. For example:
    ```
    curl -X POST \
    'https://iam.cloud.ibm.com/identity/token' \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=<your iam api key>'
    ```
    For details on the API syntax, see [IAM identity token API](https://cloud.ibm.com/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

    This is a sample of what is returned:
    ```
    {
    "access_token": "eyJraWQiOiIyMDE3MDgwOS0wMDowMDowMCIsImFsZyI6…",
    "refresh_token": "zmRTQFKhASUdF76Av6IUzi9dtB7ip8F2XV5fNgoRQ0mbQgD5XCeWkQhjlJ1dZi8K…",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1505865282
    }
    ```
4. Use the value of the `access_token` property for your {{site.data.keyword.iae_full_notm}} API calls. Set the `access_token`  value as the authorization header parameter for requests to the {{site.data.keyword.iae_full_notm}} APIs. The format is `Authorization: Bearer <access_token_value>`.

    For example:
    ```
    Authorization: Bearer eyJraWQiOiIyMDE3MDgwOS0wMDowMDowMCIsImFsZyI6IlJTMjU2In0...
    ```

## Create a token using the {{site.data.keyword.Bluemix_notm}} CLI
{: #create-token-with-ibm-cloud-cli}

Before you can create a token by using the {{site.data.keyword.Bluemix_notm}} CLI, check that you have met the following prerequisites:

1. You have a valid IBMid.
2. You have downloaded and installed the [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

To create a token using {{site.data.keyword.Bluemix_notm}} CLI:

1. Log in to the {{site.data.keyword.Bluemix_notm}} CLI:

    ```
    ibmcloud api https://cloud.ibm.com
    ibmcloud login <enter your credentials>
    ```

    If you have multiple {{site.data.keyword.Bluemix_notm}} accounts, you'll be asked to choose an account for the current session. Also, you'll need to choose an organization and space in {{site.data.keyword.Bluemix_notm}}.
2. Fetch the IAM access token:
    ```
    ibmcloud iam oauth-tokens
    ```
    The output returns the IAM token.

    **Note:** When you use the token, remove `Bearer` from the value of the token that you pass in API calls.
