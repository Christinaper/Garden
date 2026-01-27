# UE5 Garden – Lightweight Drawing-Based Plant Generation Game

## 🎮 Project Overview
Draw paths by dragging the mouse; plants and particle effects are generated automatically along the trajectory.

**Tech Stack**: UE5 + Blueprint + Spline + Niagara  
**Estimated Development Time**: 3–4 days

---

## 📅 Development Progress

### ✅ Day 1: Drawing System (Completed)

**Implemented Features**:
- Orthographic top-down camera (no perspective distortion)
- Visible mouse cursor without affecting camera rotation
- Fullscreen raycasting (no dead zones)
- Hold left mouse button to draw a blue spline path
- Release to stop; press again to start a new stroke

**Technical Notes**:
```

Camera Setup: Orthographic, Rotation(-90, -90, 0)
Input System: Set Input Mode Game and UI + Ignore Look Input
Drawing Logic: Spline Component + real-time raycasting

```

**Key Findings**:
- Pawn default forward is +Y (not +X); top-down view requires an extra -90° Yaw rotation
- Near Clip Plane in orthographic projection is relative to camera distance, not world coordinates
- Draw Debug Sphere is the most effective tool for debugging raycasts

---

### 🔜 Day 2: Plant Generation System (Planned)

- [ ] Generate plants using Instanced Static Mesh
- [ ] Even distribution along the spline
- [ ] Random rotation/scale for natural variation
- [ ] Performance optimization via instancing

---

### 🔜 Day 3: Visual Polish (Planned)

- [ ] Niagara particle trails
- [ ] Material optimization (emissive, translucency)
- [ ] Simple UI hints

---

## 🚀 How to Run

1. Open the project with UE5.3+
2. Content Browser → Maps → open `DrawingLevel`
3. Click Play (Alt+P)
4. Hold the left mouse button and drag to draw

---

## 🛠️ Core Architecture
```

BP_DrawingPawn (Player Pawn)
└─ Camera Component (Orthographic Camera)

BP_DrawingManager (Actor)
├─ SplineComponent (Path Drawing)
└─ Event Graph
├─ BeginPlay: Set input mode
├─ Left Mouse Button: Toggle drawing state
└─ Tick: Raycast + add spline points

GM_Drawing (GameMode)
└─ Default Pawn Class: BP_DrawingPawn

```

---

## 📝 Development Notes

### Debugging Techniques
1. **Black Screen**: Use Print String to verify Actor spawning → check camera activation
2. **Dead Zones**: Use Draw Debug Sphere to verify raycast coverage
3. **Input Not Triggering**: Check Input Mode → Enable Input vs Game and UI

### Pitfalls Encountered
- ❌ Camera Z = 0 caused Near Clip clipping issues
- ❌ Rotation(-90, 0, 0) resulted in no visible output → Pawn orientation issue
- ✅ Final setup: Z = 2000 + Rotation(-90, -90, 0)

---

## 🎯 Expected Showcase

**Technical Highlights**:
- Dynamic spline generation
- Instanced Mesh performance optimization
- Niagara particle system
- UE5 Lumen lighting
- Modular Blueprint architecture

---

## 📄 License
Learning project, for reference only

**Last Updated**: 2026-01-27 23:45  
**Current Status**: Day 1 completed, preparing for Day 2
