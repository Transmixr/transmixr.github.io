---
title: Media Understanding & Selection
weight: 1
---

# Overview

Our toolbox for media selection and understanding caters to XR adaptions in workflows where there already exists media content (textual, image and video). Examples are media archives or broadcast companies, that want to repurpose their existing content via XR for more immersive story telling. Core solutions offered by the media selection and understanding components are:

* Search across languages: Find videos in any language with our AI-powered search.
* Instant summaries: Get to the heart of any video with automatic summaries.
* Immersive 360° experiences: Explore 360° videos in a whole new way with interactive elements.
* Understanding complex data: Our technology makes sense of complex media, so you don’t have to.


To experience the media understanding and selection components in action, we have integrated the TRANSMIXR technologies in the webLyzard media analytics dashboard:
<p align="center">
<iframe width="420" height="315" src="//www.youtube.com/embed/xbqPjo2HXbk" frameborder="0" allowfullscreen="allowfullscreen">&nbsp;</iframe>
</p>

The webLyzard media analytics dashboard allows media professionals to manage their media assets and enrich and transform them in integrated workflows.

## Where does it fit in TRANSMIXR?

The Media Understanding & Selection toolbox provides tools for asset management and transformation. Media corporations that already possess media assets such as image and video can use the media understanding and selection workflows to integrate their content into immersive story-telling applications.


## What problems does it solve?

1. Manage media content (2d and volumetric videos and images): ingest, enrich, search
2. Repurpose content for various production environments
3. Analyze content and provide insight

## How does it work?

The pipeline collects accessible digital content in the form of data items via a Web crawler or the API. Each item is processed in the pipeline in three ways:

1. Extracting and cleaning raw content from the item;
2. Extracting or creating technical metadata for the item; and
3. Determining the descriptive metadata for the item (NLP, NER and NEL components for keyword and entity detection).

Herein, we provide documentation for 
1. Ingesting and retrieval of media assets into the webLyzard dashboard via [RESTful APIs](https://transmixr.github.io/comp_mediaselection/toolbox/). On top of classical search, we also support vector-based similarity search on the analyzed video content (during ingest).
2. Analysing and enriching media items via the [CERTH Analytics APIs](https://transmixr.github.io/comp_mediaselection/video_analysis/).
3. Summarizing video content via the [CERTH Summarization API](https://transmixr.github.io/comp_mediaselection/video_summarization/).
4. Analysis and Summarization of volumtric video via [API](https://transmixr.github.io/comp_mediaselection/volumetric_video_analysis/).

![image](/img/media_selection/media-selection-overview.png)
