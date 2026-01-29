# UE5 Garden - Lightweight Drawing Plant Generation Game

## 🎮 Project Overview
Draw trajectories with mouse drag, automatically generate plants and particle effects along the path.

**Tech Stack**: UE5 + Blueprint + Spline + Instanced Mesh + Niagara  
**Development Time**: 3-4 days

---

## 📅 Development Progress

### ✅ Day 1: Drawing System (Completed)

**Features Implemented**:
- Orthographic camera top-down view (no perspective distortion)
- Mouse visible without affecting camera rotation
- Full-screen raycast detection (no dead zones)
- Draw blue Spline trajectory by holding left mouse button
- Release to stop, press again to restart

**Technical Highlights**:
```cpp
Camera Setup: Orthographic, Rotation(-90,-90,0)
Input System: Set Input Mode Game and UI + Ignore Look Input
Drawing Logic: Spline Component + Real-time raycast
```

**Key Discoveries**:
- Pawn class defaults to +Y forward (not +X), top-down view requires additional Yaw -90°
- Orthographic Near Clip is relative distance to camera, not world coordinate
- Draw Debug Sphere is the best tool for debugging raycasts

---

### ✅ Day 2: Plant Generation System (Completed)

**Features Implemented**:
- Automatic plant generation along Spline path
- Instanced Static Mesh for high-performance rendering
- Random rotation/scale for natural variation
- Adjustable spacing and randomization parameters

**Performance**:
- 500+ instances: 60+ FPS
- Using Instance rendering to reduce Draw Calls

**Technical Implementation**:
```cpp
Core Algorithm:
  SplineLength ÷ SpawnSpacing = Number of plants
  ForLoop iteration: Sample position + Random transform + AddInstance

Component Architecture:
  BP_DrawingManager (Actor)
  ├─ SplineComponent (Spline)
  │   └─ InstancedPlantMesh (Instanced Static Mesh) ← Child of Spline
  └─ PlantSpawner (AC_PlantSpawner) ← Logic component

Key Functions:
  - GeneratePlants(SplineComponent): Main generation logic
  - GetLocationAtDistanceAlongSpline: Precise position sampling
  - Random Float in Range: Natural variation
```

**Adjustable Parameters**:
- Spawn Spacing: Distance between plants (default: 100 units)
- Random Rotation Range: Rotation variation (0-360°)
- Random Scale Min/Max: Size variation (0.8-1.2)

---

### 🚧 Day 3: Visual Effects & Polish (In Progress)

**Current Status**: Technical MVP completed, visual refinement in progress

**Completed**:
- ✅ Niagara particle trail system
- ✅ UI counter and instructions
- ✅ Core gameplay loop functional

**Known Issues**:
- ⚠️ Top-down view makes plants hard to distinguish (too small/flat)
- ⚠️ Visual similarity to solid brush strokes (lacks depth)
- ⚠️ Limited camera control restricts viewing angles

**Planned Improvements** (Day 4-7):
- 🎥 Camera system: Zoom, pan, orbit controls
- 🌿 Plant visual upgrade: Larger scale, mixed varieties, height variation
- 🎨 Environment polish: Lighting, post-processing, ground details
- 🎮 Extended interactions: Multiple brush types, eraser tool
- 🎬 Demo preparation: Photo mode, video recording

**Technical Achievement**:
- Core architecture: ✓ Stable and performant
- Performance: ✓ 500+ instances @ 60 FPS
- Scalability: ✓ Ready for feature expansion

**Focus**: Currently prioritizing visual appeal over additional features to create a more engaging demo experience.

---

## 🚀 How to Run

1. Open project with UE5.3+
2. Content Browser → Maps → Open `DrawingLevel`
3. Click Play (Alt+P)
4. Hold left mouse button and drag to draw

---

## 🛠️ Core Architecture

```
BP_DrawingPawn (Player Pawn)
└─ Camera Component (Orthographic camera)
    - Location: Relative (0,0,0)
    - Ortho Width: 5000
    - Projection: Orthographic

BP_DrawingManager (Actor)
├─ SplineComponent (Path storage)
│   └─ InstancedPlantMesh (Plant rendering)
├─ PlantSpawner (AC_PlantSpawner logic)
└─ Event Graph
    ├─ BeginPlay: Setup input mode
    ├─ Left Mouse Button: Drawing state control
    └─ Tick: Raycast + Add Spline points

AC_PlantSpawner (Actor Component)
├─ Variables:
│   ├─ PlantMeshes (Array<Static Mesh>)
│   ├─ SpawnSpacing (Float)
│   ├─ RandomRotationRange (Float)
│   └─ RandomScale Min/Max (Float)
└─ Functions:
    └─ GeneratePlants(SplineComponent)
        - Calculate spawn count
        - Sample Spline positions
        - Add randomized instances

GM_Drawing (GameMode)
└─ Default Pawn Class: BP_DrawingPawn
```

---

## 📝 Development Notes

### Debugging Techniques Learned

**Day 1 Issues**:
1. Black screen → Print String to verify Actor spawning → Check camera activation
2. Dead zones → Draw Debug Sphere to verify raycast coverage
3. Input not triggering → Check Input Mode settings

**Day 2 Issues**:
1. "Accessed None" error → Forgot to pass SplineComponent reference in Released event
2. Plants clustering at center → Wrong Coordinate Space (Local instead of World)
3. Can't see plants → InstancedMesh not configured with Static Mesh/Material

### Lessons Learned

**Component Hierarchy**:
- InstancedStaticMeshComponent must be child of a Scene Component (needs Transform)
- Actor Components (like AC_PlantSpawner) are logic-only, exist at root level
- Scene Components form the transform hierarchy

**Blueprint Best Practices**:
- Always validate references with Is Valid before use
- Use Print String liberally during development
- Separate concerns: Pawn for camera, Manager for logic, Component for algorithms

**Performance Optimization**:
- Instanced Static Mesh reduces draw calls dramatically
- 500+ instances with 60 FPS vs ~10 FPS with individual Static Meshes
- Disable shadows on instances if performance is an issue

---

## 🎯 Technical Showcase Points

- ✅ Spline dynamic generation
- ✅ Instanced Mesh performance optimization  
- ✅ Procedural generation with randomization
- ✅ UE5 Lumen lighting (Day 3)
- ✅ Niagara particle system (Day 3)
- ✅ Blueprint modular architecture

---

## 🐛 Common Issues & Solutions

### Issue: Black screen after Play
**Solution**: Check camera Rotation (-90,-90,0) and Location (0,0,2000)

### Issue: Plants not visible
**Solution**: 
- Verify InstancedPlantMesh has Static Mesh assigned
- Check Material is opaque and visible
- Verify PlantSpawner.InstancedMeshComponent is connected in BeginPlay

### Issue: Plants clustering at origin
**Solution**: Set Coordinate Space to World (not Local) in GetLocationAtDistanceAlongSpline

### Issue: "Accessed None" error
**Solution**: Pass valid SplineComponent reference when calling GeneratePlants function

---

## 📄 License
Learning project for reference only

**Last Updated**: 2026-01-30 00:07 
**Current Status**: Day 3 completed, ready for Day 4 development

---

## 📊 Project Stats

- **Lines of Blueprint Nodes**: ~150
- **Assets Used**: Minimal (Plane, Cube/Basic Mesh, Materials)
- **Performance**: 500+ instances @ 60 FPS
- **Development Time**: ~6 hours
