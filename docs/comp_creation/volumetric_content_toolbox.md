---
title: Volumetric Content Toolbox
layout: home
parent: Creation
nav_order: 2
---


# Volumetric Content Toolbox

![image info](/assets/images/comp_creation/vct/vct_slovenia.png)

## Overview

The Volumetric Content Toolbox is a workflow that shows how volumetric image and video technologies are used to create interactive XR experiences. 
The workflow was created and tested in collaboration with journalists by the example of a VR reportage on the annual ski jumping event of Planica valley in Slovenia.


### What problems does it solve?

Creation of interactive immersive experiences based on real spaces.

### How does it work?

The workflow includes both postprocessing and static 3D reconstruction from recordings of a real-world scene. Image or video material is color-corrected and creatively adjusted according to a desired visual outcome. Video material can be split into individual frames before reconstruction. The reconstruction step involves Structure from Motion and reconstruction through 3D Gaussian Splatting.

![image info](/assets/images/comp_creation/vct/Workflow.png)

With the right plugins, Gaussian splatting assets (volumetric images) and volumetric video can be brought together into one Unity 3D application.

## Details

### Capture

- **Goal:** Record multi-view imagery or video of the environment.  
- **Tools:** Drone, handheld cameras, or mobile devices.  

Follow the capture guidelines below.

### Reconstruction

- **Goal:** Convert video or images into Gaussian Splatting (GS) assets.  

- **Tools:**
  - *Luma AI* (cloud or mobile app) - not recommended anymore as of 2025.  
  - *Postshot* — better control over training parameters, recommended for VR applications where splat count matters.

Keep splat count low for VR. 


### Editing

- **Goal:** Clean up noisy GS reconstructions and prepare them for integration.  

- **Tools:**
  - [*Supersplat*](https://superspl.at/editor/) (browser-based) — allows 3D editing similar to Photoshop.  
  - Remove floating or oversized Gaussians.  
  - Export in `.ply` format.

### Preparation of Additional Assets

Alongside GS content, prepare additional media:  
- **Volumetric video (VV):**
  - Record a person with a single 2D camera.  
  - Process with *Volograms* to generate volumetric assets.
- **2D video & images:** For interviews, archival footage, or overlays.  
- **3D meshes & animations:**.  


### Integration

- **Goal:** Combine GS and VV assets into a cohesive XR experience.  
- **Tools:** 
    - Unity.
    - [Unity Gaussian Splatting plugin](https://github.com/aras-p/UnityGaussianSplatting).  
    - [Volograms Unity SDK](https://github.com/Volograms/volograms_unity_plugin).


### Performance

# VOLUMETRIC CAPTURE GUIDELINES

# Volumetric Static Scene Capture 

Terminology:  
3DGS : 3D Gaussians (Point Cloud)  
NeRF : Neural Radiance Fields. A learned 3D representation based on neural networks that is rendered using volume rendering techniques (expensive).

Please also read through the capture tips [provided by Luma](https://docs.lumalabs.ai/MCrGAEukR4orR9)

## Input Data

* Video (\>1 minute or photos \> 100). 

### Camera

Check if your camera is set up correctly.

#### *Hard Requirements*

* Keep focal length fixed. Don’t zoom.  
* Turn off digital camera stabilization.  
* Normal or fisheye lenses are okay. 360° cameras are not yet supported (for 3DGS).

#### *General Tips*

* Fix the white balance.  
* Photos can be SDR or HDR/raw.  
* [Turn off the HDR setting](https://support.apple.com/guide/iphone/adjust-hdr-camera-settings-iph2cafe2ebc/ios) if you are using an iPhone.  
* Adjust camera exposure to the light level that you want in the final capture. Fix the exposure levels if the amount of light does not change a lot. This usually means setting the camera to manual exposure or setting ISO and shutter speed to a fixed value.  
* Orbit around the center of the scene.  
* Capture at multiple heights (orbits). E.g., 3 different orbits are good.  
* *If you shoot in low light and use a camera/phone, shoot fotos instead of video (and use a tripod or other physical stabilization).*

### Capture

Now on to the actual capture.

#### *Tips*

* The length of footage required depends on how well you cover the scene and how large the scene is.  
* Avoid (motion) blur.  
* Avoid moving objects: people, animals, leaves/grass in the wind.  
* Avoid complex reflections.  
* Avoid covering the whole of the field of view with a uniform surface (e.g., a white wall).  
* Interiors are often much harder to capture than objects or exteriors.

#### *Drones*

![Capture Angles](/assets/images/comp_creation/vct/CaptureAngles.png)

* Avoid nadir shots (straight down).  
* Max. 6km/h (day), 4km/h (night) *\[this might only be true for close distance and the further away the object is, the faster you can go?\]*   
* Keep capture below 10 minutes (because things can change a lot in a scene in this timeframe).  
* Good: lateral movement (left, right). Bad: in/out (does not give new data).  
* Good: spiral between orbits.  
* It seems like it is better to keep an angle of over 45 degrees to the ground (especially when you record trees, as they can take on the color of the sky otherwise).

##### Simulators

You can practice using a drone simulator. Regrettably, most drone simulators are focused on FPV (first person view racing) drones which are harder to handle than hovering video drones.

You can make a screen recording while flying and upload this to Luma to see how your capture went.

- [Dji Flight Simulator](https://www.dji.com/pt/downloads/softwares/dji-flightsimulator-launcher) (recording video seems to only be supported in the costly pro version). Supports normal drones.  
- [Real Drone Simulator](https://www.realdronesimulator.com/downloads). The following reconstructions were captured from virtual flights in this simulator (the UI can be turned off).   
  - [Canyon](https://lumalabs.ai/embed/1a047ce5-cf9a-412b-b094-05ee89cc982f?mode=sparkles&background=%23ffffff&color=%23000000&showTitle=true&loadBg=true&logoPosition=bottom-left&infoPosition=bottom-right&cinematicVideo=undefined&showMenu=false)  
  - [Hungary](https://lumalabs.ai/embed/239ee9a3-5834-41b0-adcd-f9e28d36163b?mode=sparkles&background=%23ffffff&color=%23000000&showTitle=true&loadBg=true&logoPosition=bottom-left&infoPosition=bottom-right&cinematicVideo=undefined&showMenu=false)  
- FPV Skydive. Cannot turn off UI.

## Output Data

What do we get out of this?

1. **3D Gaussian Splatting point cloud (viewable in specialized viewers on web or other 3D engines).** Contains real lighting.  
2. A NeRF representation (not easily viewable).  
3. **Textured polygon meshes**. Can be extracted from 1\. or 2\. Currently does not contain real lighting.




# Publications

1. Haslbauer, Philipp, et al. "VR Planica: Gaussian Splatting Workflows for Immersive Storytelling." ACM International Conference on Interactive Media Experiences Workshops (IMXw). SBC, 2025.
2. Verhofstadt, Arno, et al. "Exploring user feedback in VR: the added value of qualitative evaluation methods." ACM International Conference on Interactive Media Experiences Workshops (IMXw). SBC, 2025.
