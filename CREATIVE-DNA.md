# CREATIVE-DNA.md
## Personal Style Guide for NagaVision / Curtis Soul

## Preferred Colour Palettes
- **Primary:** Dark slate grays (#2D2D2D, #1A1A1A) with neon accent blues (#00F3FF, #00BFFF)
- **Secondary:** Deep purples (#4B0082, #8A2BE2) and toxic greens (#39FF14, #ADFF2F)
- **Accent:** Radiation orange (#FF4500, #FF6347) and cyber pink (#FF1493, #FF69B4)
- **Background:** Near-black gradients (#0A0A0A to #1A1A1A) with subtle texture
- **UI Elements:** Semi-transparent dark panels (rgba(0,0,0,0.7)) with neon borders
- **Health/Energy:** Red (#FF0000) to Yellow (#FFFF00) to Green (#00FF00) gradient
- **Danger Zones:** Pulsing red (#FF0000) with black concentric rings
- **Safe Zones:** Soft green (#00FF00) with gentle pulse

## Camera Movement Styles
- **Primary:** Locked horizontal scrolling with vertical lane movement (2.5D)
- **Screenshake:** Subtle directional shakes on impact (X: ±4px, Y: ±2px, decay over 150ms)
- **Lane Transition:** Smooth linear interpolation (120ms) when changing lanes
- **Jump Camera:** Slight downward follow (-10px Y) during jump apex
- **Velocity Blur:** Minimal motion blur at high speeds (only for background elements)
- **Focus Lock:** Camera locks on player during combat arenas (Streets of Rage 2 style)
- **Parallax Layers:** 3-4 layers with varying scroll speeds (background: 0.2x, mid: 0.5x, foreground: 0.8x)
- **Zoom:** Occasional tight zoom (0.8x) for boss introductions or dramatic moments

## UI Preferences
- **Minimalist HUD:** Only essential information displayed (health, energy, score, current weapon)
- **Radial Menus:** For quick weapon/ability selection during gameplay
- **Pixel-Perfect Text:** Retro bitmap fonts with 2px outline for readability
- **Iconography:** Simple, recognizable symbols (no intricate details)
- **Feedback Systems:** 
  - Hit: Screen flash + directional shake + hitmarker
  - Kill: Screen pulse + score popup + sound effect
  - Low Health: Pulsing red border + heartbeat sound
  - Combo: Multiplier display with decaying timer
- **Menus:** 
  - Main: Vertical list with neon highlight on selection
  - Pause: Semi-transparent overlay with resume/restart/options
  - Upgrade: Grid-based with resource costs and requirement checks
- **Typography:** 
  - Primary: Press Start 2P or similar retro gaming font
  - Secondary: Courier New for technical/data displays
  - Sizing: Consistent hierarchy (Title: 24px, Header: 18px, Body: 14px)

## Visual References & Inspirations
- **Primary Influences:**
  - Streets of Rage 2/3 (Sega Genesis) - level pacing, enemy variety, boss design
  - Gunstar Heroes (Sega Genesis) - weapon variety, high-energy traversal, segmented stages
  - Golden Axe (Sega Genesis) - fantasy elements, mount/enemy variety
  - Shinobi III (Sega Genesis) - ninja agility, precision platforming
  - Contra (NES/Arcade) - weapons, alien aesthetics, difficulty curve
- **Color Inspiration:**
  - Akira (1988 anime) - neon cyberpunk palette
  - Blade Runner (1982 film) - rainy noir with neon accents
  - Cyberpunk 2077 (video game) - modern neon dystopia
  - Hotline Miami (video game) - vibrant ultraviolence palette
- **Animation Style:**
  - Disney's 12 principles (squash & stretch, anticipation, follow-through)
  - Expressive exaggeration for impact frames
  - Anticipation before big moves (wind-up)
  - Follow-through after hits (recoil)
  - Secondary motion (clothing, hair, accessories)
- **Composition:**
  - Rule of thirds for important visual elements
  - Clear foreground/midground/background separation
  - Silhouette readability for enemy recognition
  - Color coding for enemy types (grunt = gray, security = yellow, etc.)

## Brand Voice & Tone
- **Narrative Delivery:** Minimalist, environmental storytelling (like Half-Life)
- **UI Text:** 
  - Concise, action-oriented ("JUMP OVER", "SHOOT", "RELOAD")
  - All caps for urgency/commands
  - Title case for menus/options
  - Consistent terminology (always "energy" never "stamina" or "mana")
- **Sound Design Preferences:**
  - Chiptune base with modern electronic elements
  - Distinct audio signatures for each weapon type
  - Environmental audio cues (distant traffic, industrial hum)
  - Reactive music that intensifies with combat
  - Satisfying "crunch" sounds for melee hits
  - Clear audio feedback for hits/misses/block
- **Difficulty Philosophy:**
  - Easy to learn, difficult to master
  - Clear telegraphing of enemy attacks (0.5s wind-up minimum)
  - Fair hitboxes that match visual size
  - Multiple paths to victory (different weapons/strategies)
  - Punish mistakes but don't make them feel cheap
  - Continue checkpoints rather than lives system

## Games to Avoid / Anti-Preferences
- **Avoid:** 
  - Generic mobile free-to-play aesthetics (excessive UI, pay-to-win vibes)
  - Overly bright, saturated palettes that hurt readability
  - Anime-proportioned characters (excessively large eyes, tiny bodies)
  - Complex particle systems that obscure gameplay
  - Unfair hitboxes or undodgeable attacks
  - Paywalls or artificial progression barriers
  - Excessive tutorials that break immersion
  - Random number generation that feels unfair
  - Grindy progression systems
  - Microtransaction-prominent design
  - Poorly optimized effects that tank performance on mid-tier devices

## Technical Art Preferences
- **Sprite Standards:**
  - Consistent ground line for all character sprites
  - Pivot points at character feet for proper positioning
  - Export sprite sheets with consistent frame dimensions
  - Name animations descriptively (idle, walk_run, attack_jab, etc.)
  - Include multiple directions when needed (left/right facing)
- **Effects Preferences:**
  - Reuse and recolor base effects rather than creating new ones
  - Additive blending for energy/laser effects
  - Multiply blending for shadows/dark effects
  - Screen effects limited to 10-15% of screen time
  - Particle counts capped (max 50 active particles per effect type)
  - Use object pooling for all particle systems
- **Performance Priorities:**
  - 60 FPS target on mid-range Android devices (Snapdragon 7xx series)
  - Budget: 16ms per frame
  - Prioritize gameplay clarity over visual effects
  - Implement object pooling for bullets, enemies, particles
  - Use texture atlases to reduce draw calls
  - Cull off-screen entities aggressively
  - Limit post-processing effects to essential ones only
  - Budget allocation: 40% gameplay logic, 30% rendering, 20% physics, 10% audio

## Level Design Philosophies
- **Streets of Rage 2 Style (Primary):**
  - Screen-based progression with clear arena definition
  - Wave pacing: 3-8 enemies per wave with escalating difficulty
  - Environmental variety every 2-3 screens (alleys, bars, factories)
  - 2.5D lane usage for dodging and flanking
  - Boss arenas focused on pattern recognition, not platforming
  - Hazards that affect both player and enemies (explosive barrels, conveyor belts)
- **Gunstar Heroes Style (Secondary):**
  - High-energy traversal with constant movement
  - Weapon variety and combination rewards
  - Segmented stages with distinct mechanics
  - Vertical level design with ledges, hanging structures, slopes
  - Mid-bosses and multi-phase bosses with clear pattern telegraphs
  - Backgrounds that convey speed and motion (scrolling cityscapes, machinery)
- **Universal Principles:**
  - Clear visual hierarchy (player > enemies > hazards > background)
  - Consistent telegraphing (0.3-0.8s wind-up for all attacks)
  - Fair risk/reward placement (power-ups in dangerous positions)
  - Multiple solutions to encounters (different routes/weapons)
  - Memorable moments (set pieces, unique enemy combinations)
  - Progressive introduction of mechanics (don't overwhelm early)
  - Meaningful choices (not just illusion of choice)
  - Environmental storytelling (tell story through level design, not cutscenes)

## Personal Rules & Mantras
- **Gameplay First:** If it doesn't improve gameplay, it doesn't belong in the game
- **Clarity Over Cleverness:** Players should understand mechanics immediately
- **Feedback is King:** Every action needs clear, immediate feedback
- **Consistency Breeds Trust:** Same inputs should produce same outputs
- **Elegance in Simplicity:** Simple systems that interact create complex emergent behavior
- **Respect the Player's Time:** No filler, no grind, no artificial lengthening
- **Design for Mastery:** Shallow depth to start, deep ceiling for experts
- **Feel Matters More Than Realism:** Exaggerated feedback creates satisfying experiences
- **Iterate Relentlessly:** The 10th version is usually better than the 1st
- **Steal Like an Artist:** Learn from the best, but make it your own
- **Playtest Early and Often:** Your intuition is wrong; data doesn't lie
- **Finish What You Start:** Completed imperfect game > perfect unfinished prototype