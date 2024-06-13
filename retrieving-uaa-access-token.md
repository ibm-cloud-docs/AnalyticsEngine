---

copyright:
  years: 2017, 2019
lastupdated: "2017-11-02"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving UAA access tokens
{: #retrieve-uaa-token}

To retrieve an UAA access token:

1. Log in to Cloud Foundry CLI.
2. Run the command: `cf oauth-token`

	The output of this command is the UAA access token to pass to CLoud Foundry REST APIs for creating a service instance.

**Very Important:** You should not share this token with other users. Use this token only as the value for the  `authorization` request header in Cloud Foundry REST API calls.
