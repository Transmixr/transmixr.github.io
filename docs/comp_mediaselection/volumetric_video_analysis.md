---
title: Volumetric Video Analysis and Summarization
layout: home
parent: Components - Media Understanding & Selection
---

# Volumetric Video Analysis and Summarization

# **Overview** {#overview}

Volumetric video (VV) is a burgeoning media format, important for future applications of XR applications. It captures real-world subjects in 3D, allowing them to be viewed from any angle in a virtual environment, enhancing realism and interactivity of immersive applications. However it can be challenging for XR creators to work with such datasets due to their large size and complexity, as they represent detailed, photorealistic 3D shape and appearance information animated across often lengthy temporal sequences. Furthermore, the tools and standards to deal with this data type are fragmented and most have not yet reached the level of maturity of tools for processing other media assets such as video or traditional 3D models.
To address these challenges, we developed a system for semantic understanding of volumetric video that extracts meaningful metadata from VV datasets, generates compact visual and textual summaries, and offers a search interface to help creators more easily discover, work with and deploy VV content in immersive applications.

### What problems does it solve?

1. Pose & action recognition and statistical analysis: In our Volumetric Video Analysis system, an auto-rigging component first extracts a representation of the 3D pose of the human subject in each frame of the volumetric video, analogous to motion capture data. This information can be directly used for several purposes including animation, interaction and composition (e.g., combining and transitioning between multiple volumetric sequences). In addition, low-level data about the volumetric video is gathered including the 3D representation format, statistical information about the sequence, the scale of the model, and the interaction space. Such data is relevant for deploying volumetric videos alongside other assets in XR applications.
2. Visual and textual summary of volumetric video: The volumetric video summarisation system extracts high-level metadata, including keyframes based on 3D shape analysis and natural language textual descriptions of the actions and appearance of the content. In addition, animated visual summaries are generated, depicting the essential content in the data in a compact graphical representation. These outputs form the basis for efficiently cataloguing VV datasets in digital asset libraries, providing structured and searchable representations that help retrieve relevant VV datasets, and making it easier to pre-visualise and integrate them into XR experiences. As volumetric videos become more abundant in the future, solutions such as this will increase the discoverability, visibility and accessibility of VVs for media practitioners.
3. 	Web-based search interface for volumetric video. While the system is primarily intended as a backend tool for processing volumetric video datasets for a broad range of end-user applications, we also provide a front-end demonstrator in the form of a web interface that allows media creators to leverage pre-processed outputs of our summarization system to efficiently search and inspect available volumetric video assets. Furthermore, the component metadata captured in the web dataset can be indexed by general search engines so that volumetric video datasets gathered during the course of the project can be discovered and accessed by media creators and the general public.

### How does it work?

The volumetric video analysis system leverages a set of traditional and AI-enhanced tools including 3D renderers, neural network models, and optimisers to extract data, such as the rigged 3D poses of human subject in each frame of the volumetric video. The output from this analysis process is used in the volumetric video summarisation component, extracting key frames, visual summaries, natural language descriptions and spatiotemporal data, which are integrated into the metadata description of the volumetric video, providing a structured and searchable representation that helps retrieve relevant VV datasets from search engines, and making it easier to integrate them into XR experiences

