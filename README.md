# CRT Filter for URP (Unity 6 Render Graph)

![Unity Version](https://img.shields.io/badge/Unity-6000.0%2B-black?logo=unity)
![Render Pipeline](https://img.shields.io/badge/Pipeline-URP-blue)
![API](https://img.shields.io/badge/API-Render%20Graph-green)

This package provides a high-performance, **Render Graph API** compatible CRT (Cathode Ray Tube) post-processing effect designed specifically for Unity 6. Unlike legacy blit-based effects, this system leverages modern transient resource management to minimize GPU memory overhead.

## Core Features
* **Unity 6 Native:** Built from the ground up for the Render Graph API.
* **Screen Geometry:** Authentic CRT curvature (Screen Bend) and overscan controls.
* **Analog Artifacts:** High-quality Gaussian blur, phosphor bleeding, and per-scanline jitter (smidge).
* **Dynamic Effects:** Moving shadowlines, aperture grille simulation, and procedural noise.
* **SOLID Architecture:** Decoupled data (Volume) and logic (Pass) for maximum sustainability.

---

## Installation

### 1. Via Unity Package Manager (UPM)
1.  Open your Unity Project.
2.  Navigate to `Window > Package Manager`.
3.  Click the `+` icon and select **Add package from git URL...**.
4.  Paste the repository URL:
    `https://github.com/Astrobotiq/crt-filter-urp.git`

### 2. Material & Renderer Feature Setup
Since this package provides the raw shader to keep the repository lightweight, you must create a material first:
1.  In your Project window, right-click and select **Create > Material**. Name it `CRT_Material`.
2.  Select the newly created material and change its shader to `Hidden/Custom/CRTFilter` from the Inspector dropdown.
3.  Locate your project's `Universal Renderer Data` asset.
4.  Click **Add Renderer Feature** and select `Crt Renderer Feature`.
5.  Assign your `CRT_Material` to the material slot in the Renderer Feature.

---

## Parameter Reference

### Screen Geometry
| Parameter | Default | Description |
| :--- | :--- | :--- |
| **Screen Bend** | 0.0 | Controls the spherical curvature of the tube. |
| **Screen Overscan** | 0.0 | Adjusts the zoom level to hide/show screen edges. |
| **Pixel Resolution** | 320x240 | Sets the virtual resolution for the downscaling effect. |

### Artifacts & Blur
| Parameter | Default | Description |
| :--- | :--- | :--- |
| **Blur** | 0.0 | Intensity of the Gaussian blur pass. |
| **Bleed** | 0.0 | Horizontal phosphor smear effect. |
| **Smidge** | 0.0 | Micro-jittering of scanlines for analog instability. |

### Scanlines & Noise
| Parameter | Default | Description |
| :--- | :--- | :--- |
| **Scanlines** | 0.04 | Opacity of the horizontal scanline grid. |
| **Shadowlines** | 0.0 | Speed and intensity of the vertical rolling dark bar. |
| **Noise Alpha** | 0.0 | Amount of procedural analog signal noise. |

---

## Technical Architecture

The system utilizes the **Render Graph API** to manage resources efficiently. 

* **Two-Pass Execution:** The effect renders to a transient `CRTTempTexture` before blitting back to the active color buffer, ensuring compatibility with other post-processing effects.
* **Resource Management:** Textures are allocated and released automatically by the Render Graph system, preventing memory leaks and frame-buffer contention.
* **Optimized Shader:** The fragment shader uses branchless logic where possible and relies on the URP Core library for cross-platform compatibility.

---

## Repository Structure
```text
CrtEffect/
├── Runtime/
│   ├── CrtFilterVolume.cs      # Volume component data
│   ├── CrtRendererFeature.cs   # Renderer Feature injector
│   └── CrtRenderPass.cs        # Render Graph logic
├── Shaders/
│   └── CRTFilter.shader        # HLSL fragment shader
├── package.json                # UPM package manifest
└── README.md                   # Documentation
