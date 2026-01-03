<div align="center">

# ChaiNNer Workspace

<img width="5072" height="3761" alt="image" src="https://github.com/user-attachments/assets/4c795237-7183-449f-b049-cbbe28055209" />

</div>

---

This repository contains a custom workspace file (`.chn`) for [chaiNNer](https://github.com/chaiNNer-org/chaiNNer), focused on high-quality upscaling with intelligent segmentation.

The key feature of this workflow is its ability to separate the **entire person** (body and face) from the background, treating each layer with specific AI models to achieve the best possible result, reconstructing the final image without losing naturalness.

## ✨ Key Features

- **Hybrid (Video and Image):** A single file serves both purposes with a simple selector.
- **Full Body Segmentation:** Unlike workflows that focus only on faces, this uses advanced models to cutout the complete human silhouette.
- **Dedicated Processing:**
  - **Person/Face:** Processed with models focused on skin and facial features.
  - **Background:** Processed with models focused on realistic scenarios and textures.
- **Smart Resizing:** Option to define a `Target Height`, automatically maintaining the aspect ratio.

---

## ⚠️ Important Requirement (Avoiding Errors)

Due to how the chaiNNer validator works, **you must load files into BOTH input nodes (Image and Video)**, even if you are only going to process one type.

If you leave one of the nodes empty, chaiNNer will prevent execution with the following errors:
> *• Image: Load Image: Missing required input data: Image File*
> *• Image: Load Video: Missing required input data: Video File*

**Solution:**
* If processing a **Video**: Load your video into the video node and any "dummy" image (any jpg/png) into the image node.
* If processing an **Image**: Load your image into the image node and any "dummy" video (any short mp4) into the video node.

---

## ⚙️ How to Use (Mode Selector)

The workspace has been updated with logic to easily switch between Image and Video modes without manually disabling nodes.

### 1. Setup Inputs
Load your files into the **Load Image** and **Load Video** nodes (remember the warning above: both nodes must have files loaded).

### 2. Select the Type (Node "SELECT THE TYPE")
Locate the group of nodes with the yellow note **"Select Input"**. In the **"Select the Type"** node:

* **Change the "Value Index" to:**
    * **`A`** 📸 → To process **IMAGE**.
    * **`B`** 🎥 → To process **VIDEO**.

### 3. (Optional) Define Size
In the **"Target size"** group, change the value in the **"Target Height"** node:
* Set the desired height in pixels (e.g., `1080` for FHD).
* Leave at **`0`** to disable resizing and keep the original size (or the native upscale size).

---

## 🧠 Models Used

To ensure the best quality, make sure you have the following models:

### 1. Person/Face Upscale
**Model:** [4xFaceUpDAT](https://openmodeldb.info/models/4x-FaceUpDAT)  
Successor to 4xFFHQDAT, trained on the FaceUp dataset.
* *Recommendation:* Use the `4xFaceUpDAT` version.

### 2. Background Removal (Segmentation)
**Model:** [briaai/RMBG-1.4](https://huggingface.co/briaai/RMBG-1.4)  
State-of-the-art background removal model to precisely isolate the full human figure.

### 3. Background Upscale
**Model:** [4x-ClearRealityV1](https://openmodeldb.info/models/4x-ClearRealityV1)  
Trained on the SPAN architecture, this model focuses on realistic imagery and natural scenery.
