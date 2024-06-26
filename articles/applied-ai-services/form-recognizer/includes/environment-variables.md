---
title: "Environment variables"
description: set environment variables
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/06/2020
ms.author: lajanuar
---

Using your key and endpoint from the resource you created, create two environment variables for authentication:

* `FORM_RECOGNIZER_KEY` - The resource key for authenticating your requests.
* `FORM_RECOGNIZER_ENDPOINT` - The resource endpoint for sending API requests. It will look like this: 
  * `https://<your-custom-subdomain>.cognitiveservices.azure.com`

>[!NOTE]
> The endpoints for resources created after July 1, 2019 use the custom subdomain format shown below. For more information and a complete list of regional endpoints, see [Custom subdomain names for Cognitive Services](../../cognitive-services-custom-subdomains.md). 

Use the following instructions to set environment variables on your operating system.

#### [Windows](#tab/windows)

```console
setx FORM_RECOGNIZER_KEY <replace-with-your-form-recognizer-key>
setx FORM_RECOGNIZER_ENDPOINT <replace-with-your-form-recognizer-endpoint>
```

After you add the environment variables, close and reopen the console window.

#### [Linux](#tab/linux)

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

After you add the environment variable, run `source ~/.bashrc` from your console window to make the changes effective.

#### [macOS](#tab/unix)

Edit your `.bash_profile`, and add the environment variable:

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

After you add the environment variables, run `source .bash_profile` from your console window to make the changes effective.
***