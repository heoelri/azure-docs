---
# Mandatory fields. See more on aka.ms/skyeye/meta.
title: Transforms and Jobs in Media Services
: Azure Media Services
description: Transforms describe the rules for processing your videos in Azure Media Services.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 03/22/2021
ms.author: inhenkel
---

# Transforms and Jobs in Media Services

This topic gives details about [Transforms](/rest/api/media/transforms) and [Jobs](/rest/api/media/jobs) and explains the relationship between these entities.

## Overview

### Transforms/Jobs workflow

The following diagram shows transforms/jobs workflow:

![Transforms and jobs workflow in Azure Media Services](./media/encoding/transforms-jobs.png)

#### Typical workflow

1. Create a Transform.
2. Submit Jobs under that Transform.
3. List Transforms.
4. Delete a Transform, if you aren't planning to use it in the future.

#### Example

Suppose you wanted to extract the first frame of all your videos as a thumbnail image–the steps you would take are:

1. Define the recipe, or the rule for processing your videos: "use the first frame of the video as the thumbnail".
2. For each video, you would tell the service:
    1. Where to find that video.
    2. Where to write the output thumbnail image.

A **Transform** helps you create the recipe once (Step 1), and submit Jobs using that recipe (Step 2).

> [!NOTE]
> Properties of **Transform** and **Job** of the Datetime type are always in UTC format.

## Transforms

Use **Transforms** to configure common tasks for encoding or analyzing videos. Each **Transform** describes a recipe or a workflow of tasks for processing your video or audio files. A single Transform can apply more than one rule. For example, a Transform could specify that each video be encoded into an MP4 file at a given bitrate, and that a thumbnail image be generated from the first frame of the video. You would add one TransformOutput entry for each rule that you want to include in your Transform. You use presets to tell the Transform how the input media files should be processed.

### Viewing schema

In Media Services v3, presets are strongly typed entities in the API itself. You can find the "schema" definition for these objects in [Open API Specification (or Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01). You can also view the preset definitions (like **StandardEncoderPreset**) in the [REST API](/rest/api/media/transforms/createorupdate#standardencoderpreset), [.NET SDK](/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset), or other Media Services v3 SDK reference documentation.

### Creating Transforms

You can create Transforms using REST, CLI, or any of the published SDKs. The Media Services v3 API is driven by Azure Resource Manager, so you can also use Resource Manager templates to create and deploy Transforms in your Media Services account. Azure role-based access control can be used to lock down access to Transforms.

### Updating Transforms

If you need to update your [Transform](/rest/api/media/transforms), use the **Update** operation. It's intended for making changes to the description, or the priorities of the underlying TransformOutputs. It's recommended that such updates be done when all in-progress jobs have completed. If you intend to rewrite the recipe, you need to create a new Transform.

### Transform object diagram

The following diagram shows the **Transform** object and the objects it references, including the derivation relationships. The gray arrows show a type that the Job references and the green arrows show class derivation relationships.

Select the image to view it full size.  

[![Diagram showing the Transform object and the objects it references, including the class derivation relationships between the objects.](./media/api-diagrams/transform-small.png)](./media/api-diagrams/transform-large.png#lightbox)

## Jobs

A **Job** is the actual request to Media Services to apply the **Transform** to a given input video or audio content. Once the Transform has been created, you can submit jobs using Media Services APIs, or any of the published SDKs. The **Job** specifies information like the location of the input video and the location for the output. You can specify the location of your input video using: HTTPS URLs, SAS URLs, or [Assets](/rest/api/media/assets).  

### Job input from HTTPS

Use [job input from HTTPS](job-input-from-http-how-to.md) if your content is already accessible via a URL and you don't need to store the source file in Azure (for example, import from S3). This method is also suitable if you have the content in Azure Blob storage but have no need for the file to be in an Asset. Currently, this method only supports a single file for input.

### Asset as Job input

Use [Asset as job input](job-input-from-local-file-how-to.md) if the input content is already in an Asset or the content is stored in local file. It's also a good option if you plan to publish the input asset for streaming or download (say you want to publish the mp4 for download but also want to do speech to text or face detection). This method supports multi-file assets (for example, MBR streaming sets that were encoded locally).

### Checking Job progress

The progress and state of jobs can be obtained by monitoring events with Event Grid. For more information, see [Monitor events using EventGrid](monitoring/job-state-events-cli-how-to.md).

### Updating Jobs

The Update operation on the [Job](/rest/api/media/jobs) entity can be used to modify the *description* and the *priority* properties after the job has been submitted. A change to the *priority* property is effective only if the job is still in a queued state. If the job has begun processing, or has finished, changing priority has no effect.

### Job object diagram

The following diagram shows the **Job** object and the objects it references including the derivation relationships.

Click the image to view it full size.  

[![Diagram showing the Job object and the objects it references, including the class derivation relationships between the objects.](./media/api-diagrams/job-small.png)](./media/api-diagrams/job-large.png#lightbox)

## Ask questions, give feedback, get updates

Check out the [Azure Media Services community](media-services-community.md) article to see different ways you can ask questions, give feedback, and get updates about Media Services.

## See also

* [Error codes](/rest/api/media/jobs/get#joberrorcode)
* [Filtering, ordering, paging of Media Services entities](filter-order-page-entities-how-to.md)

## Next steps

- Before you start developing, review [Developing with Media Services v3 APIs](media-services-apis-overview.md) (includes information on accessing APIs, naming conventions, etc.)
- Check out these tutorials:

    - [Tutorial: Encode a remote file based on URL and stream the video](stream-files-tutorial-with-rest.md)
    - [Tutorial: Upload, encode, and stream videos](stream-files-tutorial-with-api.md)
    - [Tutorial: Analyze videos with Media Services v3](analyze-videos-tutorial.md)
