---
title: CERTH Video Summarization API Specification
layout: home
parent: Video Summarization
---

# CERTH Video Summarization Specification  
**Application Programming Interface**

**Table of Contents**

[Overview](#overview)

[Initial Step](#initial-step)

[Step 1 \-  Upload video](#step-1---upload-video)

[Step 2 \- Workflow Options](#step-2---workflow-options)

[Step 3 \- Target summary / Aspect ratio](#step-3---target-summary--aspect-ratio)

[Performance and Results](#performance-and-results)

[Summarization Step](#summarization-step)

[Interface Features](#interface-features)

[Different functionalities based on chosen workflow](#different-functionalities-based-on-chosen-workflow)

[Aspect Ratio transformation Step](#aspect-ratio-transformation-step)

[Interface Features](#interface-features-1)

[Different functionalities based on chosen workflow](#different-functionalities-based-on-chosen-workflow-1)

[Final Page \- Results](#final-page---results)

# Overview

In case of issues or questions, please do not hesitate to contact us via:

|       | Names | Emails |
| :---- | :---- | :----- |
| **CERTH** | Ioannis Kontostathis <br> Vasileios Mezaris | [ioankont@iti.gr](mailto:ioankont@iti.gr) <br> [bmezaris@iti.gr](mailto:bmezaris@iti.gr) |


The **CERTH Video Summarization API** is responsible for the summarization of the videos. An online interactive video summarization service that allows users to submit videos in various formats, generate summaries for use in social media channels and refine the algorithm’s selected summary and aspect ratio transformation with just a few clicks, through interactive intermediate steps. You can access the tool to the following link: [https://multimedia2.iti.gr/interactivevideosumm/service/start.html]( https://multimedia2.iti.gr/interactivevideosumm/service/start.html). The UI also includes links to video tutorials demonstrating how to use it.

# Initial Step

The landing page of the interactive summarization tool allows users to process video files up to 400 MB and 20 minutes in duration. The service accepts videos from YouTube, Facebook, Twitter, Instagram, Vimeo, DailyMotion, LiveLeak, and Dropbox, and supports the following formats: mp4, webm, mov, wmv, ogv, mkv, and avi.

**Note:** Not all videos from these platforms are accessible, due to platform-specific restrictions or user-defined privacy settings. Additionally, submitted URLs must point to a **single video**, not a playlist.

## Step 1 \-  Upload video

Users can submit videos for processing in one of two ways:

* **By URL** – Provide the direct URL of an online video.  
* **By Upload** – Upload a local video file from their device.

## Step 2 \- Workflow Options

Before submission, users can choose one of the following processing modes:

* Default – Summarization followed by aspect ratio transformation  
* Summarization only  
* Aspect ratio transformation only  
* Aspect ratio transformation followed by summarization

## Step 3 \- Target summary / Aspect ratio

After selecting a workflow, users can specify the desired **summary duration** and/or **target aspect ratio**, depending on the chosen mode. 

## Performance and Results

* The processing service operates at high speed, often significantly faster than real-time.  
* Processing time may vary depending on server load and the number of queued requests.  
* After submission, users can keep the browser tab open to monitor progress.  
* Once analysis is complete, results are displayed automatically through the web interface.

![](https://transmixr.github.io/assets/images/media_selection/video_summarization/initial_page.png)
**Fig 1. The initial page of the tool.**

# Summarization Step

The **Summarization Step** allows users to review and edit the automatically generated video summary by adding or removing specific temporal segments.

## Interface Features

* **Video Player with Two Modes**:  
  * **Video Mode**: Playback and navigation are enabled across the entire video.  
  * **Summary Mode**: Playback is limited to only the segments included in the summary.

* **Timeline Axis**:  
  * Displays the full video segmented into shots, based on the system’s output.  
  * Users can navigate the timeline using the .  
  * Initially highlights the segments automatically selected for inclusion in the summary.

* **Interaction Capabilities**:  
  * **Include/Exclude Audio**: Click on a segment to toggle its original audio on or off.  
  * **Remove Segment**: Double-click on a selected fragment to remove it.  
  * **Add Segment**: Double-click the upper arrow at the start and end frames of the desired segment to include it in the summary.

* **Zoom Functionality**:  
  * A secondary axis positioned below the main timeline enables zooming in and out by a factor of 10 on the video. Double-clicking a segment on the secondary axis zooms in or out (10x) on the corresponding section of the main timeline.  
* **Player Display Information**:  
  * **Top Left**: Displays the *current summary duration* and the *target summary duration* based on the user's initial selection.  
  * **Top Right**: Shows the *current video timestamp* and the *total duration* of the original video.

## Different functionalities based on chosen workflow

Based on the selected workflow at the initial step, users have access to different capabilities: 

1\. Default Workflow:

* **Summarization → Aspect Ratio Transformation.** After reviewing and optionally editing the summary, the user proceeds to the aspect ratio transformation step.

2\. Custom Workflow:

* **Summarization** or **Aspect Ratio Transformation → Summarization.** Users can upload audio tracks (MP3 or WAV) for summary fragments where original audio has been removed. Also, users can upload subtitle files in .srt or .vtt format to generate a summarized version with captions.

# ![](https://transmixr.github.io/assets/images/media_selection/video_summarization/step2_page.png)
**Fig 2. The interface of the summarization step.**

# Aspect Ratio transformation Step

The **Aspect Ratio Transformation Step** allows users to adjust and preview the results of the automatic smart-cropping algorithm.

## Interface Features

* **Video Player with Bounding Box.** A bounding box overlays the video to indicate the cropped area selected by the algorithm. Users can manually adjust this box to better frame the content.

* **Timeline Axis.** Segmented according to the selected video fragments from the previous step (Summarization), enabling quick navigation.

* **Manual Adjustment.** Users can refine the crop by:  
  * Positioning the bounding box in the **first frame** of a shot they wish to modify.  
  * Re-positioning it in a **later frame** (in 10-frame increments) or the **last frame** of that shot.  
  * The system automatically interpolates the crop position across the frames between those two points.

* **Zoom Controls.** A **secondary axis** below the main timeline allows zooming in and out, following the same interaction model as in the Summarization Step.

* **Preview results.** A **“Preview Cropped Video”** button lets users view the current state of the smart crop in real time before finalizing their selection.

## Different functionalities based on chosen workflow

Based on the selected workflow at the initial step, users have access to different capabilities:

1\. Default Workflow:

* **Summarization → Aspect Ratio Transformation.** After finalizing the summary, users can adjust the cropped area if desired. Moreover, users can upload MP3 or WAV files to replace excluded audio in selected segments, as well as subtitle files. Once cropping is adjusted or confirmed, users proceed to the **final page** to review and download results.

2\. Custom Workflow

* **Aspect Ratio Transformation only.** Users skip the summarization step and apply cropping to the entire video. Users can retain original audio or upload a new audio track. Once cropping is complete, users proceed directly to the **final page**.  
* **Aspect Ratio Transformation → Summarization.** Users begin by adjusting the cropped area, and then proceed to the summarization step. After applying the necessary adjustments, they continue to the summarization step.

![](https://transmixr.github.io/assets/images/media_selection/video_summarization/step3_page.png) 

**Fig 3. The interface of the aspect ratio transformation step.**

# Final Page \- Results

Once video processing is complete, users can **view and download the final summary video** by clicking the button located at the bottom of the results page.

Notification via Email (Optional). If an email address was provided during submission, the user may safely close the web application. When the summarization is complete, an email notification will be sent. This email includes a unique link to access the results directly.

**Note:** The link remains active for **24 hours**. After this period, the video, the summarization results and the email address (if provided) are **automatically and permanently deleted** from our servers.

![](https://transmixr.github.io/assets/images/media_selection/video_summarization/final_page.png) 
**Fig 4. The interface of the results page.**
