<p align="center">
  <img src="assets/header.svg" width="100%" />
</p>
---
# 🧠 SurgicalVisualization

<p align="center">

![.NET](https://img.shields.io/badge/.NET-9.0+-512BD4?logo=dotnet&logoColor=white)
![C#](https://img.shields.io/badge/C%23-Engineering-239120?logo=c-sharp&logoColor=white)
![WPF](https://img.shields.io/badge/WPF-Desktop%203D-5C2D91)
![HelixToolkit](https://img.shields.io/badge/HelixToolkit-Viewport3D-1f2937)
![Three.js](https://img.shields.io/badge/Three.js-WebGL-black?logo=three.js)
![Architecture](https://img.shields.io/badge/Architecture-MVVM%20%7C%20Deterministic-0f172a)

</p>

A precision-focused 3D surgical model inspection platform built with WPF + HelixToolkit and a complementary Three.js browser implementation.

Designed around **deterministic geometry calculations, traceable logging, and clean MVVM separation** to support reproducible surgical guidance and alignment workflows.

---

# Executive Overview

**Surgical Visualization** demonstrates:

- Real-time STL / OBJ rendering
- Deterministic geometry metrics
- Quaternion-based alignment workflows
- Deep-freeze cross-thread safety for WPF 3D objects
- Audit-ready logging (UTC + invariant culture)
- Offline-first secure deployment design
- Desktop + Browser rendering parity

This project reflects engineering discipline suited for controlled environments such as:

- Surgical simulation platforms  
- Robotic guidance systems  
- Research labs  
- Government or defense visualization workflows  

---

# 🖥 Application Preview

<p align="center">

<img src="SurgicalVisualization/Assets/screens/desktop-wireframe.png" width="85%" />
<br/><br/>
<img src="SurgicalVisualization/Assets/screens/mesh-closeup.png" width="85%" />
<br/><br/>
<img src="SurgicalVisualization/Assets/screens/browser-viewer.png" width="85%" />

</p>

---

# 🎬 Rendering & Processing Workflow

<p align="center">
<img src="SurgicalVisualization/Assets/demo/surgical-workflow.svg" width="90%" />
</p>

---

# Core Capabilities

### 3D Model Import
- STL (binary & ASCII)
- OBJ (with MTL + textures where available)

### Deterministic Metrics
- Triangle count
- Bounding box dimensions
- Center of mass (double precision)
- Load duration timing

### Alignment Engine
- Quaternion-based rotation
- Align principal axis to target vector
- Repeatable orientation math

### Telemetry
- FPS reporting
- Camera position & look direction
- Orientation readouts

### Export Tools
- Screenshot capture
- Append-only UTC log export

---

# Architecture Overview

```mermaid
flowchart TD
    User[Operator] -->|Commands| UI[MainWindow + MVVM]
    UI --> VM[MainViewModel]
    VM --> Loader["ModelLoader (STA Thread)"]
    Loader --> Metrics[PrecisionMathService]
    Loader --> Model["Model3DGroup (Frozen)"]
    VM --> Logger[LoggerService]
    VM --> Viewport[HelixViewport3D]
    Model --> Viewport
    Metrics --> VM
    Logger --> Logs[(Logs/system_log.txt)]
```

### Architectural Highlights

- **Dedicated STA Thread** for WPF 3D object creation
- **Deep-freeze geometry artifacts** before UI injection
- Deterministic double-precision math
- Strict MVVM orchestration layer
- Append-only logging for traceability

---

# Model Loading & Metrics Pipeline

```mermaid
sequenceDiagram
    participant User
    participant VM as MainViewModel
    participant Loader as ModelLoader
    participant Math as PrecisionMathService
    participant Logger as LoggerService

    User->>VM: Load Model (STL/OBJ)
    VM->>Logger: Log "LoadModel start"
    VM->>Loader: LoadAsync(path)
    Loader->>Math: Compute bbox, COM, triangle count
    Loader-->>VM: Frozen model + ModelInfo
    VM->>Logger: Log metrics + duration
    VM-->>User: Update UI + ZoomExtents
```

---

# MVVM Interaction Model

```mermaid
flowchart LR
    View[MainWindow.xaml]
    ViewModel[MainViewModel]
    Services[Services]
    Models[Models]

    View -->|Bindings + Commands| ViewModel
    ViewModel -->|Uses| Services
    ViewModel -->|Consumes| Models
```

---

# Desktop Rendering Stack

- .NET 9.0 (adjustable to 8.0)
- WPF
- HelixToolkit.Wpf
- Viewport3D GPU rendering
- Deterministic math service
- Thread-isolated model loading

### Desktop Rendering Flow

```
User Loads STL / OBJ
        ↓
ModelLoader (STA Thread)
        ↓
PrecisionMathService
        ↓
Deep-Frozen Model3DGroup
        ↓
HelixViewport3D
        ↓
Interactive Camera Controls
```

---

# Browser Rendering Stack

Located at: `SurgicalVisualization/index.html`

- Three.js
- STLLoader
- WebGL Renderer
- Local static server required

### Browser Flow

```
STL File
   ↓
Three.js STLLoader
   ↓
BufferGeometry
   ↓
Scene + Camera + Lighting
   ↓
WebGL Rendering Loop
```

---

# Repository Layout

```
SurgicalVisualization/
├─ README.md
├─ SurgicalVisualization/
│  ├─ App.xaml / App.xaml.cs
│  ├─ MainWindow.xaml / MainWindow.xaml.cs
│  ├─ ViewModels/
│  ├─ Models/
│  ├─ Services/
│  ├─ Helpers/
│  ├─ Assets/
│  ├─ Logs/
│  └─ SurgicalVisualization.sln
└─ assets/
   ├─ desktop-main.png
   ├─ desktop-wireframe.png
   ├─ browser-viewer.png
   ├─ mesh-closeup.png
   └─ surgical-workflow.svg
```

---

# Build & Run (Desktop)

1. Open `SurgicalVisualization/SurgicalVisualization.sln`
2. Use Visual Studio 2022+ or Rider
3. Ensure .NET desktop workload is installed
4. Target `net9.0-windows` (or `net8.0-windows`)
5. Restore NuGet packages
6. Build & Run
7. Click **Load Model**

---

# Run Browser Viewer

From repository root:

```bash
python -m http.server 8000
```

Then open:

```
http://localhost:8000/SurgicalVisualization/index.html
```

> Do not open via `file://` — ES module loading requires a local server.

---

# Logging & Determinism

- Logs written in UTC ISO-8601 format
- Stored at `SurgicalVisualization/Logs/system_log.txt`
- Invariant culture formatting
- Double-precision calculations
- All geometry deep-frozen before UI access

---

# Troubleshooting

**OBJ not visible**
- Ensure `.mtl` and texture files are colocated

**Black model**
- Missing textures or lighting — try Reset View

**Thread ownership errors**
- Confirm model import runs through `ModelLoader`

---

# Engineering Focus Areas

This repository demonstrates:

- Deterministic computational geometry
- Cross-thread WPF 3D safety
- Quaternion alignment math
- MVVM separation of concerns
- GPU-aware rendering design
- Offline-first deployment thinking
- Audit-friendly telemetry logging

---

# Roadmap

- Measurement tools
- Cross-sectional slicing
- Annotation overlays
- Multi-model comparison
- DICOM integration
- AI-assisted anomaly detection

---
