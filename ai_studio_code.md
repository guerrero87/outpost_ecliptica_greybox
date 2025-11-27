# Prototype Generation Prompt: Outpost Ecliptica (Three.js Greybox)

**Role:** Act as a Senior Creative Developer specialized in Three.js and Game Design.
**Task:** Generate a single, self-contained HTML file (including CSS and JavaScript) representing a playable "Greybox" prototype based on the specifications below.

## 1. Technical Stack & Setup
- **Library:** Use Three.js via CDN (e.g., unpkg or cdnjs).
- **Controls:** Use `PointerLockControls` for First-Person interaction.
- **Visual Style:** Minimalist/Prototyping. Use simple geometries (Box, Cylinder) with wireframes or flat colors. No external textures or assets.
- **Responsiveness:** Handle `window.resize`.

## 2. Scene & Environment ("The Base")
- **Room:** Create a medium-sized enclosed room (e.g., a large inverted BoxGeometry).
- **Lighting:**
  - `AmbientLight`: Low intensity (base visibility).
  - `PointLight`: Positioned near the "Light Generator". Its intensity is tied to the game logic (see Section 4).
- **Interactable Objects:**
  1.  **The Door:** A rectangular prism on one wall.
      - *Visual Feedback:* Color changes from Green (100% HP) to Red (0% HP).
  2.  **Light Generator:** A distinct cylinder or machine-like shape in the center or corner.
      - *Visual Feedback:* Emits the main light of the scene.

## 3. Game State & Variables
Initialize the following variables:
- `cycleState`: Enum/String ['SAFE', 'DANGER']. Starts in 'SAFE'.
- `energy`: Integer (Player resource). Starts at 100. Max 100.
- `doorHealth`: Float (0-100). Starts at 100.
- `lightHealth`: Float (0-100). Starts at 100.
- `timer`: Countdown timer for cycle switching.
- `isRepairing`: Boolean (for interaction logic).

## 4. The Core Loop Logic
The game loop runs inside the `animate()` function.

### A. Safe Cycle
- **Duration:** 10 Seconds.
- **Logic:**
  - Timer counts down.
  - No health decay.
  - **On Start of Cycle:** Automatically refill `energy` by a fixed amount (e.g., +50, up to max).
- **UI:** Display "SAFE CYCLE - SYSTEMS STABLE".

### B. Danger Cycle
- **Duration:** 20 Seconds.
- **Logic:**
  - Timer counts down.
  - **Decay:** `doorHealth` and `lightHealth` decrease automatically every frame (e.g., -5 HP per second).
  - **Visual Consequence:**
    - Link `PointLight.intensity` to `lightHealth`. As health drops, the room gets darker (scary atmosphere).
  - **Lose Condition:** If `doorHealth` <= 0, trigger **Game Over**.
- **UI:** Display "DANGER CYCLE - SURVIVE" in red.

## 5. Player Controller
- **Movement:** Standard FPS WASD movement.
  - **Speed:** Constant base speed (no sprint, no crouch).
  - Use simple collision detection (keep player inside the room bounds).
- **Interaction (Hold-to-Repair):**
  - Use a `Raycaster` from the center of the camera.
  - **Logic:**
    - The player must look at an interactable object (Door or Generator) within distance < 4 units.
    - The player must **HOLD** the Left Mouse Button (mousedown/mouseup events).
    - **While Holding:**
      - If `energy` > 0:
        - Decrease `energy` (e.g., -10 per second).
        - Increase Object `health` (e.g., +20 per second, clamped to 100).
      - If `energy` == 0: Stop repair.
- **Reticle:** A simple HTML/CSS crosshair in the center of the screen to help aiming.

## 6. User Interface (HUD)
Use a clean HTML overlay positioned absolutely over the canvas.
- **Top Center:** Cycle State & Timer.
- **Bottom Left:** Energy Counter (Current/Max).
- **World Labels (Optional):** Floating HTML labels near the Door/Generator showing their HP %, OR use screen-space bars.
- **Game Over Screen:** A hidden overlay that appears when the player loses, offering a "Restart" button.

## 7. Implementation Steps for the Code
1. Setup Scene, Camera, Renderer.
2. Add geometries for Room, Door, Generator, Player.
3. Implement `PointerLockControls` with a "Click to Start" overlay.
4. Build the `update()` function to handle:
   - Cycle timer logic.
   - Health decay (only in Danger cycle).
   - Input handling (Movement + Raycasting + Repair logic).
   - UI updates (DOM manipulation).
5. Render loop.

## 8. Workflow & Version Control Strategy
Although you will provide a single final file, structure your explanation and code comments as if you were following this Git workflow. 

### Commit Style
Use **Conventional Commits**:
- `feat:` for new features.
- `fix:` for bug fixes.
- `style:` for formatting/UI.
- `refactor:` for code restructuring.

### Milestones & Pull Requests (PRs)
Break down the development into the following PRs. In your final output, denote which blocks of code belong to which milestone via comments.

**PR #1: Environment & Setup**
- **Branch:** `feature/environment-setup`
- **Goal:** Render the room, the door, and the generator with basic lighting.
- **Expectation:** Static scene, no movement yet.

**PR #2: Player Controller**
- **Branch:** `feature/player-controller`
- **Goal:** Implement `PointerLockControls`, WASD movement, and basic collision (stay inside room).
- **Expectation:** Player can walk around and look.

**PR #3: Core Logic & UI**
- **Branch:** `feature/game-loop`
- **Goal:** Implement the Safe/Danger timers, variable initialization, and the HUD overlay.
- **Expectation:** UI updates based on the timer, but objects don't decay yet.

**PR #4: Interaction & Polish (MVP)**
- **Branch:** `feature/interaction-mechanic`
- **Goal:** Implement Raycaster, Hold-to-Repair logic, Health Decay system, and Game Over state.
- **Expectation:** Fully playable loop according to specs.

**Deliverable:** 
1. Provide the **Final Code** (Merge of PR #4).
2. Briefly summarize the "Commit History" you would have generated to reach this state.