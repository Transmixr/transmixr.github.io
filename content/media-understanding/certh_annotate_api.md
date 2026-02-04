---
title: CERTH Video Analysis API Specification
weight: 5
indent: true
---

**Table of Contents**

[Overview](#overview)

[Video Analysis Process](#video-analysis-process)

[360-Degree Video Analysis](#360-degree-video-analysis)

[Image Analysis Process](#image-analysis-process)

[Text Analysis Process](#text-analysis-process)

[API description for the analysis of videos](#api-description-for-the-analysis-of-videos)

[Example: Video analysis request](#example-video-analysis-request)

[API description for the analysis of 360-degrees videos](#api-description-for-the-analysis-of-360-degrees-videos)

[Example: 360-degrees video analysis request](#example-360-degrees-video-analysis-request)

[API description for the analysis of images](#api-description-for-the-analysis-of-images)

[Example: image collection – zip file request](#example-image-collection-–-zip-file-request)

[Example: image collection – individual URLs request](#example-image-collection-–-individual-urls-request)

[Checking the Status of an Analysis](#checking-the-status-of-an-analysis)

[Example: Checking the status of a video analysis](#example-checking-the-status-of-a-video-analysis)

[Retrieving the results](#retrieving-the-results)

[Example: Retrieving the results of a video analysis](#example-retrieving-the-results-of-a-video-analysis)

[Example: Retrieving the results of an image collection analysis](#example-retrieving-the-results-of-an-image-collection-analysis)

[API description for the analysis of text](#api-description-for-the-analysis-of-text)

# Overview

In case of issues or questions, please do not hesitate to contact us via:

|       | Names | Emails |
| :---- | :---- | :----- |
| **CERTH** | Ioannis Kontostathis <br> Damianos Galanopoulos <br> Vasileios Mezaris | [ioankont@iti.gr](mailto:ioankont@iti.gr) <br> [dgalanop@iti.gr](mailto:dgalanop@iti.gr) <br> [bmezaris@iti.gr](mailto:bmezaris@iti.gr) |

The **CERTH Video Analysis API** is responsible for the extraction of metadata from videos, images and text. It generates structured and meaningful descriptions (also known as semantic metadata) that help systems interpret and analyze multimedia content. The API communicates with the **weblyzard’s API** through secure HTTPS requests. This connection enables the exchange of data and supports advanced content understanding. To access our API, please contact us to receive the required user key parameter, which is essential for authentication.

# Video Analysis Process

The analysis of videos is conducted at three temporal levels: **scene**, **shot**, and the whole **video**. Each level focuses on a different granularity of content segmentation and understanding.

## Temporal Segmentation

* **Shots:** Continuous segments recorded by a single camera without interruption. They are the smallest temporal units in video segmentation.  
* **Scenes:** Larger segments that consist of one or more shots. They are both semantically and temporally coherent, often aligning with meaningful parts of the video's narrative.

### Analysis by Level

#### Video-Level Analysis

* **Visual event detection** – Identifies major activities throughout the entire video.  
* **Sentiment analysis** – Analyzes the emotional content expressed in the video.  
* **Video summarization** – Generates a condensed version of the video by selecting the most important segments.  
* **Scene segmentation** – Automatically divides the video into semantically meaningful scenes.

#### Scene-Level Analysis

* **Shot segmentation** – Detects and separates individual shots within a scene.  
* **Keyframe extraction** – Selects representative frames that summarize the main content of each scene.

#### Shot-Level Analysis

* **Keyframe extraction** – Identifies important frames that best represent the content of a shot.  
* **Visual concept detection** – Recognizes visual elements such as objects, scenes, or activities.  
* **Sentiment analysis** – Evaluates emotional content at a finer (shot) level.  
* **Cross-modal signature extraction** – Extracts features that link visual content with other modalities such as text, for multimodal analysis and retrieval.

## 360-Degree Video Analysis

For panoramic 360-degree videos, the API:

* Detects the most important regions across the entire view.  
* Converts the spherical video into a traditional 2D format, including the salient events.  
* Applies standard video analysis techniques, as described above.

# Image Analysis Process

The CERTH Video Analysis API also supports the analysis of individual images. The following methods (also described above) are available:

* **Visual concept detection**  
* **Sentiment analysis**  
* **Cross-modal signature extraction** 

# Text Analysis Process

The CERTH Video Analysis API supports analysis of short text inputs, such as textual queries or captions. Generates feature representations of the text that can be compared or aligned with visual data, enabling multimodal content understanding and retrieval.

# API description for the analysis of videos

To request the analysis of a video, submit a POST call to the following endpoint: **https://transmixr-idt.iti.gr/video-annotation**. 

The URL of the video and the user key must be included as parameters in the body of the request in JSON format. You can use tools like Postman to easily create and send the request. The service supports videos from various online platforms and social media such as YouTube, Facebook, Twitter, Vimeo etc. as well as custom URLs. The reply to the request is a JSON document that includes a short message and, if the request was valid, an item id that uniquely identifies the video and is later used to retrieve the status and the results. 

| Path | https://transmixr-idt.iti.gr/video-annotation |  |
| :---- | :---- | :---- |
| Method | POST |  |
| Parameters: | *\<video\_url\>* the URL of the video to be processed *\<user\_key\>* a unique 32-digits access key that allows access to the service |  |
| Returns: | JSON file |  |
| Status codes | 200, 403, 404, 409 |  |
| Attributes: |  |  |
| *Message* | *A short response message. It can be one of the following:* |  |
|  | **Status Code** | **Message** |
|  | **200** | **The REST call has been received. Please check the status of the analysis via the appropriate REST call** *Explanation: The request was valid and video processing has started*  |
|  | **403** | **Not valid user key** *Explanation: The user\_key used in the parameters is not valid* |
|  | **403** | **Limit exceeded. Try again later** *Explanation: The maximum total video duration sent for processing the last 10 hours was exceeded* |
|  | **404** | **The video URL is broken** *Explanation: The content of the video\_url parameter is not valid* |
|  | **404** | **XX : Error. No such mode** *Explanation: The service allows specific analysis mode, such as “video-annotation”. This error message informs that the mode requested does not exist.* |
|  | **404** | **Bad request** *Explanation: The request was not correctly formed* |
|  | **409** | **Video is currently being processed** *Explanation: The request sent is currently being processed* |
|  | **409** | **Video already in queue** *Explanation: The video has already been sent and is waiting in the queue* |
| ***\<item\_id\>*** | A unique identifier for the video sent. It is used later to retrieve the status and the results |  |

**Table 1: Overview of the video annotation API endpoint, including request method, parameters, and response status codes with explanations.**

## Example: Video analysis request

In this example, we submit a request to analyze the following YouTube video:  
**https://www.youtube.com/watch?v=RzGS74FsGG0**

Upon successful submission, the API responds with a confirmation message indicating that the request has been received. The response also includes an \<item\_id\>, which serves as a unique identifier for the analysis request.

You will use this \<item\_id\> in subsequent requests to:

* Check the **status** of the analysis  
* Retrieve the **final results** once processing is complete

| Request | POST call to: https://transmixr-idt.iti.gr/video-annotation Body: {“video\_url”:”https://www.youtube.com/watch?v=RzGS74FsGG0”,  “user\_key”: “Z0g\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*”} |  |
| :---- | :---- | ----- |
| **Reply** | Status code: 200 {     *"message"*: "The REST call has been received. Please check the status of the analysis via the appropriate REST call",     "*video\_id*": "0f025ca87779280a9c9806018d2fd4fd",     "*item\_id*": "0f025ca87779280a9c9806018d2fd4fd" } |  |

**Table 2: Example of a POST request to submit a video from YouTube for analysis, and the corresponding successful response.**

# API description for the analysis of 360-degrees videos
To request the processing of a 360-degree video, submit a POST call to https://transmixr-idt.iti.gr/video-annotation. 

The URL of the 360-degrees video, the user key, and an additional parameter indicating that the video is 360 degrees must be included as parameters in the request body in JSON format. The supported video platforms are YouTube, Vimeo and custom URLs. The reply to the request is a JSON document that includes a short message and, if the request was valid, an item id that uniquely identifies the video and is later used to retrieve the status and the results, as a normal 2D-video.

## Example: 360-Degrees video analysis request

In this example, we submit a request to analyze the following YouTube video:  
**https://www.youtube.com/watch?v=sPyAQQklc1s**

Upon successful submission, the API responds with a confirmation message indicating that the request has been received. The response also includes an \<item\_id\>, which serves as a unique identifier for the analysis request.

You will use this \<item\_id\> in subsequent requests to:

* Check the **status** of the analysis  
* Retrieve the **final results** once processing is complete

| Request | POST call to: https://transmixr-idt.iti.gr/video-annotation Body: {“video\_url”:”https://www.youtube.com/watch?v=sPyAQQklc1s”,  “user\_key”: “Z0g\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*”, ”360_video”:”True”} |  |
| :---- | :---- | ----- |
| **Reply** | Status code: 200 {     *"message"*: "The REST call has been received. Please check the status of the analysis via the appropriate REST call",     "*video\_id*": "ar025ca87779280a9c9806018d2fd4fd",     "*item\_id*": "ar025ca87779280a9c9806018d2fd4fd" } |  |

**Table 3: Example of a POST request to submit a 360-degrees video from YouTube for analysis, and the corresponding successful response.**

# API description for the analysis of images

To submit an image or a collection of images for analysis, send a POST request to the following endpoint:

**https://transmixr-idt.iti.gr/image-annotation**.

The image / image collection can be either a zip file containing one or more images or individual image URLs that can be downloaded. In the case of the zip file, the URL of the zip file should be provided in the “zip\_url” parameter. In the case of individual image URLs, a list of the URLs should be provided in the “image\_urls” parameter. In both cases the image collection can consist of between 1 and 1000 images.

| Path | https://transmixr-idt.iti.gr/image-annotation |  |
| :---- | :---- | :---- |
| Method | POST |  |
| Parameters: | \<zip\_url\> *the URL of the image collection (zip file) to be processed (only one of the zip\_url and image\_urls parameters should be used) \<images\_url\>\_ a list of URLs of the images to be processed (only one of the zip\_url and image\_urls parameters should be used) \<user\_key) a unique 32-digits access key that allows access to the service* |  |
| Returns: | JSON file |  |
| Status codes | 200, 403, 404, 409 |  |
| Attributes: |  |  |
| *Message* | *A short response message. It can be one of the following:* |  |
|  | **Status Code** | **Message** |
|  | **200** | **The REST call has been received. Please check the status of the analysis via the appropriate REST call** *Explanation: The request was valid and image processing has started* |
|  | **403** | **Not valid user key** *Explanation: The user\_key used in the parameters is not valid* |
|  | **404** | **XX : Error. No such mode** *Explanation: The service allows specific analysis mode, such as “video-annotation”. This error message informs that the mode requested does not exist.* |
|  | **404** | **Bad request** *Explanation: The request was not correctly formed* |
|  | **409** | **Image collection is currently being processed** *Explanation: The request sent is currently being processed* |
|  | **409** | **Image collection already in queue** *Explanation: The image collection has already been sent and is waiting in the queue* |
| ***\<item\_id\>*** | *A unique identifier for the image collection sent. It is used later to retrieve the status and the results* |  |

**Table 4: Overview of the image annotation API endpoint, including request method, parameters, and response status codes with explanations.**

## Example: image collection – zip file request

In this example we request the analysis of an image collection in a zip file.

| Request | POST https://transmixr-idt.iti.gr/image-annotation Body:  {“zip\_url”: ”https://example.com/collection.zip”,  “user\_key”: “Z0g\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*”} |  |
| :---- | :---- | ----- |
| **Reply** | Status code: 200 {   “*message*”: “The REST call has been received. Please check the status of the analysis via the appropriate REST call”,   “*item\_id*”:  “716797cdedd2d4f577f3503d09a8” } |  |

**Table 5: Example of a POST request to submit an image collection (ZIP file) for analysis, and the corresponding successful response.**

## Example: image collection – individual URLs request {#example:-image-collection-–-individual-urls-request}

In this example we request the analysis of an image collection in individual URLs. The collection consists of 3 images.

| Request | POST https://transmixr-idt.iti.gr/image-annotation Body: {“image\_urls”: \[”https://example.com/image100.jpg”,                                                “https://example.com/image101.jpg”,                             ”https://example.com/image102.jpg”\],  “user\_key”: “Z0g\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*”} |  |
| :---- | :---- | ----- |
| **Reply** | Status code: 200 {   “message”: “The REST call has been received. Please check the status of the analysis via the appropriate REST call”, “item\_id”:  “716797cdedd2d4f577f3503d09a8” } |  |

**Table 6: Example of a POST request to submit individual image URLs for analysis, and the corresponding successful response.**

# Checking the Status of an Analysis

The second step in the workflow is to check the status of the submitted video analysis. This should be done periodically, as the analysis may take some time to complete. Some of the status messages are temporary and some are final. You should continue polling the status endpoint until a final status is returned. The Table 7 below lists all possible status messages and clearly distinguishes between temporary and final statuses. To issue a request for the status we submit a **GET** call to **https://transmixr-idt.iti.gr/status/\<item\_id\>**, where \<item\_id \> is the item\_id previously retrieved.

| Path | https://transmixr-idt.iti.gr/status/\<item\_id\> |  |
| :---- | :---- | :---- |
| Method | GET |  |
| Returns: | JSON file |  |
| Status codes | 200, 404 |  |
| Attributes: |  |  |
| *Message* | The status of the analysis. It can be one of the following: |  |
|  | **Status Code** | **Message** |
|  | **200** | **VIDEO\_WAITING\_IN\_QUEUE** *Explanation:  The video is waiting to be processed in the queue Type: Temporary* |
|  | **200** | **ITEM\_WAITING\_IN\_QUEUE** *Explanation: The image collection is waiting to be processed in the queue Type: Temporary* |
|  | **200** | **VIDEO\_DOWNLOAD\_STARTED** *Explanation:  Downloading of the video has been initiated Type: Temporary* |
|  | **200** | **IMAGE\_COLLECTION\_DOWNLOAD\_STARTED** *Explanation:  Downloading of the image collection has been initiated Type: Temporary* |
|  | **200** | **VIDEO\_DOWNLOAD\_FAILED** *Explanation: Video downloading has failed Type: Final* |
|  | **200** | **VIDEO\_DOWNLOAD\_TIMEOUT** *Explanation: The video was taking too long to download so downloading was cancelled Type: Final* |
|  | **200** | **MAX\_VIDEO\_DURATION\_EXCEEDED** *Explanation: The is an 1 hour limit to the duration of the video that can be submitted Type: Final* |
|  | **200** | **VIDEO\_ANALYSIS\_STARTED** *Explanation: The analysis of the video has started Type: Temporary* |
|  | **200** | **IMAGE\_COLLECTION\_ANALYSIS\_STARTED** *Explanation: The image collection analysis has started Type: Temporary* |
|  | **200** | **VIDEO\_ ANALYSIS\_COMPLETED** *Explanation: The analysis of the video has completed successfully Type: Final* |
|  | **200** | **IMAGE\_COLLECTION\_ANALYSIS\_COMPLETED** *Explanation: The image collection analysis has completed successfully Type: Final* |
|  | **200** | **VIDEO\_ANALYSIS\_FAILED** *Explanation: The analysis of the video has failed  Type: Final* |
|  | **200** | **IMAGE\_COLLECTION\_ANALYSIS\_FAILED** *Explanation: The image collection analysis has failed Type: Final* |
|  | **400** | **Wrong file name or status file does no longer exist** *Explanation: No status of the requested item\_id exists* |
| ***\<item\_id\>*** | *A unique identifier for the video sent. It is used later to retrieve the status and the results* |  |

**Table 7: Detailed summary of the analysis status check API, showing the request method, required parameters, and possible response status codes along with their explanations.**

## Example: Checking the status of a video analysis

In this example we request the status of the previously submitted video. The reply informs us that the analysis has started.

| Request | GET https://transmixr-idt.iti.gr:/status/716797cdedd2d4f577f3503d09a8 |  |
| :---- | :---- | ----- |
| **Reply** | Status code: 200 {   “status”: “VIDEO\_ANALYSIS\_STARTED”  } |  |

**Table 8: Example of a GET request to check the status of a video analysis, along with the corresponding successful response.**

# Retrieving the results

The final step of the workflow is the retrieval of the results. Once the results have been created, they can be retrieved for 48 hours. After this point they are no longer available on our server. To get the result you should issue a **GET** call to **https://transmixr-idt.iti.gr/result/\<item\_id\>**, where \<item\_id\> is the identifier of your video/image collection.

Please note that the returned JSON includes only the URLs for the produced video summarization, video thumbnails, and the shot keyframes and must be downloaded independently if needed.

| Path | https://transmixr-idt.iti.gr/result/\<item\_id\> |  |
| :---- | :---- | ----- |
| Method | GET |  |
| **Returns** | **JSON FILE** |  |
| **Status code** | **200, 404** |  |

**Table 9: Overview of the analysis results retrieval endpoint, including request path, method, return type, and possible status codes**

| Reply | Status code: 200 {"expires\_at": \<local\_expiration\_date\>,  "framerate": \<video\_frame\_rate\>,  "generated\_at": \<local\_creation\_date\>,  "generated\_by": "http://transmixr-idt.iti.gr",  "scenes": \[          {		 "scene\_id": \<scene\_1\_id\>, "begintime": \<begintime\>,               "endtime": \<endtime\>,               "keyframes": \[                 {                     "time": \<keyframe\_1\_time\>,                     "url": \<keyframe\_1\_url\>                 }, |
| :---- | :---- |
|  |                 {                     "time": \<keyframe\_2\_time\>,                     "url": \<keyframe\_2\_url\>                 },                 ...                 {                     "time": \<keyframe\_5\_time\>,                     "url": \<keyframe\_5\_url\>                 }\], "shots": \[                 {                     "shot\_id": \<shot\_1\_id\>,                     "begintime": \<shot\_1\_begintime\>,      "endtime":  \<shot\_1\_endtime\>,                     "concepts": {                         \<concept\_1\_id\>: \<concept\_1\_score\>,                         \<concept\_2\_id\>: \<concept\_2\_score\>,            ...                         \<concept\_30\_id\>: \<concept\_30\_score\>,                     },                     "keyframes": \[                         {                             "time": \<keyframe\_1\_time\>                             "url": \<keyframe\_1\_url\>                         },                         {                             "time": \<keyframe\_2\_time\>                             "url": \<keyframe\_2\_url\>                         },                         {                             "time": \<keyframe\_3\_time\>                             "url": \<keyframe\_3\_url\>                         }\],                     "sentiment": \[                         \<sentiment\_id\>, \<sentiment\_score\>                     \],                                          "signature": \[                         \<signature\_1\_score\>,                         \<signature\_2\_score\>,           ...          \<signature\_2048\_score\>\] }, \]     },     \],  "events": {  \<event\_1\_id\>: \<event\_1\_score\>,  \<event\_2\_id\>: \<event\_2\_score\>,  ...  \<event\_10\_id\>: \<event\_10\_score\>,                   },  "sentiment": \[         \<sentiment\_id\>,  \<sentiment\_score\> \],  "summary": \<video\_summary\_url\> "thumbnails": \[  \<thumb\_url\_1\>,  \<thumb\_url\_2\>,  \<thumb\_url\_3\>,  \<thumb\_url\_4\>,  \<thumb\_url\_5\>      \],      "version": "v1.1"  } |

**Table 10: Structure of the JSON response returned by the analysis results endpoint.**

## Example: Retrieving the results of a video analysis

In this example, the results of a video analysis are retrieved. The returned JSON document contains multiple layers of information:

* **Top-level fields** include general metadata such as the video’s frame rate (framerate), generation timestamp (generated\_at), and analysis summary URL (summary).  
* The scenes array contains the list of detected scenes. For each scene, the following attributes are provided:  
  * scene\_id: A unique identifier for the scene.  
  * begintime and endtime: The start and end time of the scene, in seconds.  
  * keyframes: A list of keyframes with their timestamps (time) and URLs (url).  
  * shots: A list of shots contained within the scene.

* For each **shot**, the JSON includes:  
  * shot\_id: A unique identifier for the shot.  
  * begintime and endtime: Start and end time of the shot.  
  * keyframes: A list of up to three keyframes, each with a time and url.  
  * concepts: The top 30 visual concepts detected in the shot, each associated with a confidence score (the higher the score, the more relevant the concept).  
  * sentiment: A pair indicating sentiment — a label (positive or negative) and a numerical score between 0 and 1 (higher values indicate more positive sentiment).  
  * signature: A list of floating-point values representing a visual descriptor of the shot, based on the middle keyframe.

* At the video level:  
  * sentiment: Overall sentiment for the video, in the same binary \+ numerical form.  
  * events: A set of predicted visual events, each associated with a confidence score.  
  * thumbnails: A list of URLs pointing to representative thumbnails from the video.

| Request | GET https://transmixr-idt.iti.gr/result/716797cdedd2d4f577f3503d09a8 |
| :---- | :---- |
| **Reply** | Status code: 200 {    "expires\_at": "2020-07-15 10:51:13.304437",     "framerate": 25.000,     "generated\_at": "2020-07-01 10:51:13.304426",     "generated\_by": "https://transmixr-idt.iti.gr",     "scenes": \[         {             "scene\_id": "Sc1",             "begintime": 0.040,             "endtime": 53.800,             "keyframes": \[                 {                     "time": 9.000,                     "url": "https://transmixr-idt.iti.gr/keyframe/716797cdedd2d4f577f3503/shot4\_1"                 },                 …             \],              "shots": \[                 {                     "shot\_id": "Sh1",                     "begintime": 0.040,                     "endtime": 1.400,                     "concepts": {                         "Walking": 0.004,                         "Smartphone": 0.005,                         …                      },                      "keyframes": \[                         {                             "time": 0.360,                             "url": "https://transmixr-idt.iti.gr/keyframe/716797cdedd2d4f577f3503/shot1\_1"                         },                         …                      \],                     "sentiment": {                         "positive",  "0.866345"                     },                     "signature": \[                         0.05675,                         0.00458,                         …                     \]                 },                 …            \],         },         …      \],      "sentiment": {            "negative", "0.1745"      }     "events": {         "Cleaning windows": \-8.943937301635742,         "Discus throw": \-6.931890487670898,         … }, "summary": "https://transmixr-idt.iti.gr:443/summary/716797cdedd2d4f577f3503", "thumbnails": \[  " https://transmixr-idt.iti.gr: 443/thumbnail/716797cdedd2d4f577f3503/1", " https://transmixr-idt.iti.gr: 443/thumbnail/716797cdedd2d4f577f3503/2", …  " https://transmixr-idt.iti.gr: 443/thumbnail/716797cdedd2d4f577f3503/5",      \], },     "version": "v1.1" } |
|  | Status code: 200 {    "expires\_at": "2020-07-15 10:51:13.304437",     "framerate": 25.000,     "generated\_at": "2020-07-01 10:51:13.304426",     "generated\_by": "https://transmixr-idt.iti.gr",     "scenes": \[         {             "scene\_id": "Sc1",             "begintime": 0.040,             "endtime": 53.800,             "keyframes": \[                 {                     "time": 9.000,                     "url": "https://transmixr-idt.iti.gr/keyframe/716797cdedd2d4f577f3503/shot4\_1"                 },                 …             \],              "shots": \[                 {                     "shot\_id": "Sh1",                     "begintime": 0.040,                     "endtime": 1.400,                     "concepts": {                         "Walking": 0.004,                         "Smartphone": 0.005,                         …                      },                      "keyframes": \[                         {                             "time": 0.360,                             "url": "https://transmixr-idt.iti.gr/keyframe/716797cdedd2d4f577f3503/shot1\_1"                         },                         …                      \],                     "sentiment": {                         "positive",  "0.866345"                     },                     "signature": \[                         0.05675,                         0.00458,                         …                     \]                 },                 …            \],         },         …      \],      "sentiment": {            "negative", "0.1745"      }     "events": {         "Cleaning windows": \-8.943937301635742,         "Discus throw": \-6.931890487670898,         … }, "summary": "https://transmixr-idt.iti.gr:443/summary/716797cdedd2d4f577f3503", "thumbnails": \[  " https://transmixr-idt.iti.gr: 443/thumbnail/716797cdedd2d4f577f3503/1", " https://transmixr-idt.iti.gr: 443/thumbnail/716797cdedd2d4f577f3503/2", …  " https://transmixr-idt.iti.gr: 443/thumbnail/716797cdedd2d4f577f3503/5",      \], },     "version": "v1.1" } |

**Table 11: Example of a JSON response returned by a GET request for video analysis results.**

## Example: Retrieving the results of an image collection analysis

This example demonstrates how to retrieve the results of an **image collection analysis**. The returned JSON document includes a list of analyzed images, each identified by its file name (e.g., "img1003.jpg"). For each image, the following information is provided:

* **concepts** – A list of the top 30 visual concepts detected.  
* **sentiment** – The sentiment associated with the image.  
* **signature** – The cross-modal signature.  
* **dimensions** – The width and height of the image.  
* **analysis** – A boolean indicating whether the image was successfully analyzed.

If an image could not be analyzed—for example, due to a download failure—the analysis field will be set to false, and the concepts and sentiment fields will **not** be included.

The format and content of the results are the same as those returned for video analysis.

| Request | GET https://transmixr-idt.iti.gr/result/716797cdedd2d4f577f3503d09a8 |
| :---- | :---- |
| **Reply** | Status code: 200 {     "img1003.jpg": {         "analysis": "True",         "concepts": {             "yt8m\_top30": {                 "Animal": 0.010635197162628174,                 "Boat": 0.9999825954437256,                 …             }         },         "sentiment": {             "negative",             "0,078937"         },         "dimensions": "150x200",         "signature": \[             0.10345,             0.02353,             …         \]     },     … } |

**Table 12: Example of a JSON response returned by a GET request for image collection analysis results.**

# API description for the analysis of text

Text processing consists of signature extraction only. In contrast to the image/video analysis, which is asynchronous, the text signature extraction is synchronous. This means that the processing result (the signature vector) is instantly returned to the processing request.

| Path | https://transmixr-idt.iti.gr/text-sign-extr |  |  |
| :---- | :---- | :---- | ----- |
| Method | **POST** |  |  |
| **Parameters: *user\_key Text*** |  *a unique 32-digits access key that allows access to the service The text that will be processed* |  |  |
| **Returns** | **JSON FILE** |  |  |
|  | **status code** | **data** |  |
|  | **200** | **{“signature”: \<signature vector\>}** *Explanation: The returned signature vector, which is a list of 2048 floating point numbers* |  |
|  | **403** | **{“message”: “Not valid user\_key.”}** *Explanation: The user\_key used in the parameters is not valid* |  |
|  | **404** | **{“message”: “Bad request”}** *Explanation: The request was not correctly formed* |  |

**Table 13: Overview of the signature extraction API endpoint, including request details, parameters, and possible response status codes with explanations.**

| Request | POST https://transmixr-idt.iti.gr/text-sign-extr Body:  {“text”: “A group of people crossing a forest”, “user\_key”: “Z0g\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*”} |
| :---- | :---- |
| **Reply** | Status code: 200 {   “signature”: \[0.00345, 0.04586, 0.01345, …\] } |

**Table 14: Example of a POST request for text-based signature extraction and the corresponding successful response.**  
