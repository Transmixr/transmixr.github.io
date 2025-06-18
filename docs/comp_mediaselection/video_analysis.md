---
title: Video Analysis Service
layout: home
parent: Components - Media Understanding & Selection
---

# Video Analysis Service

## Overview

The CERTH Video Analysis API is responsible for the extraction of metadata from videos, images and text. It generates structured and meaningful descriptions (also known as semantic metadata) that help systems interpret and analyze multimedia content. The API communicates with the weblyzard’s API through secure HTTPS requests. This connection enables the exchange of data and supports advanced content understanding.

### What problem does it solve?

This API solves the problem of making unstructured multimedia content—such as videos, images, and text—understandable and searchable by machines. Raw media lacks the semantic structure needed for effective interpretation, retrieval, or analysis. The API extracts meaningful metadata that describes the content’s structure, concepts, emotions, and key events, enabling systems to automatically analyze, index, and retrieve multimedia data more intelligently and efficiently.

### How to use it?

The API works by analyzing multimedia content—videos, images, and text—and extracting semantic metadata through secure HTTPS requests. For videos and images, the process is asynchronous and follows three steps:

1. Submit a request
2. Check the status
3. Retrieve the results once processing is complete


These requests are placed in a queue and handled in a first-come-first-served order. For text, the analysis is synchronous, and results are returned instantly.
Depending on the content type and requested method, the API performs tasks such as scene and shot segmentation, keyframe extraction, visual concept and event detection, sentiment analysis, video summarization, saliency detection for 360° videos, and cross-modal signature extraction. These operations produce structured metadata that helps systems understand and interact with multimedia content more effectively.
