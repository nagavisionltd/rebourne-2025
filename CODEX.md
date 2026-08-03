# Project: Rebourne

## Core Vision
- Side-scrolling action beat 'em up with vehicle mechanics
- Pixel art animation-based combat system
- Highway/chase theme with progressive level difficulty
- Focus on responsive controls and impactful combat feedback
- Progressive unlock system for abilities and vehicles

## Tech Stack
- Single-file HTML5 game (index.html)
- Native JavaScript/Canvas API (no external libraries)
- Object-oriented architecture with Entity-Component patterns
- Sprite sheet-based animation system
- Local storage for save/persistence (planned)
- Responsive design for multiple screen sizes

## Art Direction
- Gritty urban/post-apocalyptic aesthetic
- Dark color palette with neon accents (consistent with Jack's design)
- 2.5D lane system (3 lanes for player/enemy/obstacle movement)
- Sprite-based characters with directional facing
- Geometric placeholder obstacles (to be replaced with sprites)
- Consistent lighting and shadow effects
- Pixel-perfect collision detection

## Animation Rules
Based on JACK_ANIMATIONS.md and observed patterns:

**Jack Rebourne (Player):**
- Idle: 4 frames (idle_jack.png sequence)
- Walk/Run: 10 frames (run/ sequence)
- Attack Combos: Variable frames per move (Jab:4, Right Punch:2, etc.)
- Jump: 7 frames total (Jump Up:2, Mid-Air:3, Landing:2)
- Jump Attack: 4 frames total (Hit1:2, Hit2:2)
- Bike Mode: 15 frames idle/move + 4x4 frames shooting bursts
- Hurt: 5 frames (hurt/ sequence)
- Falling: 1 frame (frame_006_fall.png)
- Death: 1 frame (frame_010_death.png)

**Enemies:**
- Grunt Walk: 7 frames
- Grunt Attack (Kick/Punch): 3 frames each
- Security Walk: 5 frames
- Security Attack (Bat/Punch): 2/3 frames

**General Principles:**
- All animations use delta-time for frame-rate independence
- Animation state machine prevents invalid state transitions
- Frame dimensions standardized per sprite sheet
- Transparent backgrounds on all sprite assets
- Horizontal sprite sheet layout

## Sprite Sheet Standard
- Format: PNG with transparency
- Layout: Horizontal frame sequence
- Naming Convention: descriptive_lowercase.png or pathway/frame_xxx.png
- Frame Size: Varies by asset but consistent within sprite sheet
- Examples:
  - jack_idle.png: Single frame idle
  - jack_rebourne_sprites/run/frame_000.png to frame_009.png: Run animation
  - jack_rebourne_sprites/attack_combo/1.jab/frame_001.png to frame_004.png: Jab combo
  - new_enemies/grunt/grunt_walk/frame_000.png to frame_006.png: Grunt walk

## Coding Standards
Adapted from CODING_STANDARDS_2026.md and observed patterns:

**Phaser Lifecycle Compliance:**
- init(): Handle data passing and initial state (used for level setup)
- preload(): Load ALL assets (images, sprite sheets, JSON) - NEVER instantiate objects here
- create(): Instantiate game objects (player, enemies, UI) - NEVER load assets here
- update(): Frame-based logic (use delta for movement and timers)

**Scope & `this`:**
- Use Arrow Functions for callbacks (listeners, tweens) to maintain `this` binding to Scene
- Avoid mixing `self`, `that`, and `this` patterns

**JSON Integrity:**
- No trailing commas in JSON data
- All keys must be double-quoted
- Verify data is loaded before iterating/accessing

**Physics & Positioning:**
- Verify `this.physics` is enabled before use
- Use `this.cameras.main.centerX/Y` for center-based positioning
- Implement momentum-based movement (acceleration + friction) not direct position setting
- Variable jumps: scale gravity/jump force based on button hold duration

**Clean-up & Memory Management:**
- Always remove event listeners when destroying objects
- Destroy unused sprite sheets and textures when changing levels
- Pool objects where possible (bullets, particles)
- Check for null/undefined before accessing object properties

**Code Organization:**
- Modular functions with single responsibility
- Clear, descriptive naming (camelCase for variables/functions, PascalCase for classes)
- Document complex systems with JSDoc-style comments
- Avoid magic numbers; use named constants
- No global variables; encapsulate in appropriate classes/objects
- Separate concerns: rendering, input, physics, AI, UI, audio systems

## Lessons Learned
From SUMMARY.md, LEVEL5_UPDATES.md, and direct observations:

1. **Asset Loading:** All sprite assets MUST be included in the allImagePaths array during preload() or they won't display (fixed idle_jack.png issue)
2. **Animation References:** Never reference undefined animations (fixed 'bike_jump' -> 'bike' issue)
3. **Hitbox Sizing:** Enemy hitboxes should match visual sprite size tightly (reduced from 120x120 to 80x80 improved combat feel)
4. **Jump Mechanics:** Jump animation should be clearly defined and tested; reusing existing animations is acceptable for iteration
5. **Obstacle Systems:** Lane-based spawning should reuse existing laneYPositions array for consistency
6. **Collision Feedback:** Visual/audio feedback improves player understanding of collisions (needs implementation)
7. **Reward Systems:** Granting resources for skillful play (energy for obstacle jumps) encourages mastery
8. **Environmental Objects:** Should have update() and draw() methods with clear lifecycle management (returns 'remove' for cleanup)
9. **Spawn Timers:** Variable intervals (1500-2500ms) create more engaging unpredictability than fixed timers
10. **Progression Systems:** Obstacle frequency/speed should scale with level progress for difficulty curve
11. **Placeholder Assets:** Geometric shapes are acceptable for initial implementation but should be replaced with sprites
12. **Input Handling:** Jump should consume input by clearing relevant keys to prevent double-triggering
13. **Projectile Origins:** Adjust spawn points based on character state (player.y-45 for bike positioning)
14. **Enemy Behavior:** Enemies should spawn in lanes matching player for fair combat encounters
15. **State Management:** Game state should be clearly defined (playing, paused, game over, level complete)

## Known Issues
From SUMMARY.md and LEVEL5_UPDATES.md:

1. **Jump Animation:** Could be improved with specific bike jump frames (currently reusing bike animation)
2. **Obstacle Variety:** Limited to 3 types (barrels, roadblocks, spikes); could add more or animated versions
3. **Difficulty Scaling:** Obstacle frequency/speed doesn't increase over time within a level
4. **Sound Effects:** Missing audio feedback for jumps, collisions, shots, and enemy hits
5. **Visual Polish:** Obstacles use geometric shapes instead of sprite-based assets
6. **Health Bars:** No visible health indicators for player or entities (planned but not implemented)
7. **UI Elements:** Minimal on-screen display (score, energy, health, level progress)
8. **Save System:** No persistence between sessions (planned for localStorage)
9. **Mobile Optimization:** Touch controls not implemented (currently keyboard-only)
10. **Performance:** Particle systems may need optimization for mobile devices
11. **Level Transitions:** No smooth transition between levels (current implementation is abrupt)
12. **Boss Encounters:** No defined boss battles or level-end challenges
13. **Ability System:** No skill trees or ability upgrades beyond basic combat
14. **Story Integration:** Minimal narrative delivery between gameplay segments
15. **Accessibility:** No colorblind modes, remappable controls, or difficulty presets

## Rebourne-Specific Systems to Document as Skills
These should be created as reusable skill files in a `/skills/` directory:

1. sprite-animation-system.md - Animation state machine and frame-rate independent updates
2. lane-based-spawning.md - Enemy/obstacle spawning in 2.5D lane system
3. momentum-physics.md - Acceleration/friction based movement with variable jumps
4. combo-system.md - Attack chaining with frame validation and input buffering
5. bike-vehicle.md - Vehicle state handling with shooting mechanics
6. obstacle-system.md - Lane-based obstacle spawning, collision, and reward systems
7. hitbox-management.md - Precise collision detection with visual debugging
8. asset-loading-pattern.js - Reliable preloading with error handling and fallbacks
9. health-display.md - Visual health bars with damage/heal animations
10. ui-overlay.md - Score, energy, level progress display system