---
title: Template-based Authoring
layout: home
parent: Creation
nav_order: 3
---


# Template-based Authoring

The template-based authoring approach is intended to support rapid creation of XR  
experiences. A template is a pre-designed and pre-defined format that serves as a  
foundation for creating XR experiences. Templates allow non-technical users to  
create XR experiences by modifying various design features. These may include  
adjusting experience elements such as selecting and populating interactions,  
adding content objects, selecting games, and more, all as part of the XR experience.  
This means that the same template can be (re-)used by the same organisation  
multiple times to create experiences around different topics at a lower cost. This  
concept has been informed by the requirements of and developed and tested in the  
cultural heritage use case but as a tool is broadly applicable to the Creative and  
Cultural sector.

## **Overview**

In **Curator Studio**, curators create VR experiences by selecting from pre-built **experience templates** instead of starting from scratch. Each template defines a narrative structure (scenes, mini-games, etc.) into which the curator’s content will be inserted. The system uses **CSV metadata files** (spreadsheets of collection data) along with associated media (images, audio, videos, 3D models; currently only images are supported), to automatically fill out the template’s story world. In other words, curators can **transform CSV-based curated archive data into immersive narratives** without coding, simply by mapping their data to the template’s slots. This template-based authoring approach makes it straightforward to **tailor or refresh the VR experience** over time – curators can update the CSV or swap media to generate new story content, since the underlying template remains constant. By using flexible, pre-designed templates, the system **streamlines the authoring workflow** and ensures that even extensive archive datasets can be deployed as visually compelling, interactive VR stories that are easy to modify and redeploy across sites.

## **Problems the Application Solves**

Cultural heritage institutions often face challenges in making their vast collections accessible and engaging to the public. Template-Based Authoring system addresses several key problems:

* **Accessible & Efficient Curation:** Museums and archives need tools to build immersive content **without requiring advanced technical skills**. Curator Studio provides a user-friendly interface for creating VR exhibits, allowing curators to import **large volumes of archival data via CSV** and link each entry with media files in just a few clicks. This lowers the barrier for heritage professionals to create high-tech experiences, by automating metadata ingestion and media handling.

* **Engaging Interactive Experiences:** Traditional exhibits can struggle to captivate modern audiences. The VR game component turns archive items and metadata into an **interactive narrative** – players collaborate in solving puzzles, sorting items, and exploring virtual spaces derived from the archive content. This transforms passive data into a multi-sensory, game-like experience that is **educational, collaborative, and visually immersive**, helping to attract and engage visitors. Compelling storytelling and game mechanics ensure that the cultural content holds audience attention and delivers educational value in a fun way.

* **Audience Accessibility & Inclusivity:** The system is designed so that **visitors of various ages and backgrounds can easily participate** in the VR experience. It offers *straightforward entry* with minimal setup or instruction and supports features like multiple language options and intuitive interactions, ensuring **no visitor is excluded** due to language or technical barriers. By providing an immersive experience that is easy to use (even for those new to VR) and allowing social play (up to 4 players together), the prototype makes digital heritage content more accessible and appealing to a broad public.

* **Content Reuse & Longevity:** Heritage exhibits must be sustainable and easily updated. The template-based approach promotes **reusability** – curators can quickly refresh the experience with new data or media without rebuilding the software. The same VR template can be redeployed across multiple locations or over long periods, simply by plugging in different CSV datasets for different collections. This ensures the immersive experience stays relevant and can adapt to new content, supporting efficient *reuse of curation efforts* and longer exhibit lifespans.

## **How Template-Based Authoring Works**

*The **Curator Studio** authoring interface, showing the template selection step. The sidebar outlines the workflow steps (Select Template, Data Upload, Chambers & Minigames, etc.), and the main panel provides options and previews for the chosen template.* 

Using the Curator Studio tool, curators follow a step-by-step workflow to build and export an immersive VR experience from their archival content. The process is as follows:

1. **Select a Template:** The curator begins by browsing the available experience templates and choosing one that fits the story or game they want to create*(currently only "Space Archivist" is available)*. For example, they might select the *“Space Archivist”* template – a pre-designed multiplayer VR game scenario with defined scenes and puzzles. The Curator Studio displays information about the chosen template’s structure (how many scenes, what kind of mini-games, etc.) so the curator understands the narrative framework they will be filling with content.

2. **Import Dataset & Media:** Next, the curator uploads a **dataset** containing the archive entries they wish to include. This is typically a CSV file exported from a collection database, containing metadata fields for each item (e.g. title, date, category, description). Along with the CSV, the curator also imports the associated **media files** (such as images, audio clips, or 3D models; currently only images are supportd) referenced by the metadata. Upon upload, Curator Studio’s **CSV parser** processes the file and converts each row into a structured object or “card” representing an archive item. The system automatically validates and stores the media, creating thumbnails/previews, and prepares the metadata fields for use in the VR scenes. This bulk import capability allows dozens or even hundreds of records to be ingested at once, linking each data entry with its media so they can be used interactively in the VR template.

3. **Configure Scenes & Metadata Mapping:** Once the data is loaded, the curator configures how it will appear and behave in the VR experience. Curator Studio presents options to customize each **scene or “chamber”** in the template and assign which mini-game or interaction will take place there. The curator decides on the number of scenes (e.g. how many chambers in the space archive), and for each scene, picks a mini-game template from the library (for example, a data-sorting game, a timeline puzzle, or a geolocation challenge). They then **map the metadata to the gameplay** – selecting which metadata attributes will be used in each mini-game (for instance, using the “date” field for a timeline order game, or a “category” field for a sorting game). The curator also chooses the specific archive items (“cards”) that will appear in each scene from the uploaded dataset, and defines any categories or correct answers for the sorting/quiz mechanics. Essentially, this step is where the curator **links their content to the template’s story logic**: deciding what the players will see and do with the imported archive items in the VR world (e.g. matching artifacts to themes, placing events on a timeline, etc.). Curator Studio provides a visual interface for these assignments, so the curator can drag-and-drop media or select fields from dropdowns, rather than writing code.

4. **Additional Customization:** The authoring tool also allows further tailoring of the experience’s presentation and narrative elements. For example, curators can choose how players are represented in the VR – either via **real-time volumetric captures** of participants or using stylized 3D avatar characters. They can enable **multilingual support** by adding multiple languages for content and interface text, making the experience playable in multiple languages. The system supports adding **introductory or ending narrative scenes** as well: curators may include a flat video, a 360° immersive video, or custom narrative sequences that play at the start or end of the experience to provide context or story closure. These options let institutions adapt the template to their specific storytelling needs – for instance, introducing the historical scenario with a short video, or ending the experience with a fly-through of a virtual archive gallery using the uploaded images. All of these configurations are done through Curator Studio’s GUI, with prompts and previews to guide the curator through each decision.

5. **Export & Deployment:** After the curator has configured the template and is satisfied with the setup, Curator Studio **exports the experience**. At this final step, the tool generates a **JSON configuration file** that encapsulates all the curated content, metadata mappings, and settings for the VR game. This JSON file (together with the uploaded media assets) can then be deployed to the VR runtime. Essentially, the Curator Studio output serves as the “script” or **content package** for the Space Archivist game – telling the VR application which scenes to present, what items and puzzles to include, what narration or visuals to show, and so on. The platform provides a setup guide to help deploy this configuration to the target hardware (for example, installing the VR experience on museum PCs or a server). Once deployed, museum visitors can launch the multiplayer VR game, and it will fetch the curated JSON+media to run the experience as authored. This export process means curators can **build once and run anywhere** – the same template configuration can be loaded on different VR installations, enabling easy distribution of the experience.

Overall, the Cultural Heritage Experience prototype demonstrates how template-based authoring empowers **curators to become the authors of VR content**. With minimal technical effort, they can integrate archival collections into immersive story frameworks. The combination of a flexible authoring tool and a reusable VR game template addresses the core needs of cultural heritage institutions—making it easier to curate large datasets, **engage audiences through interactive storytelling, and continuously refresh and reuse content** within the dynamic medium of social virtual reality.
