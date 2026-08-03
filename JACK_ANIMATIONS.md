# Jack Rebourne Animation Blueprint (2026)

This document maps the new frame-based sprites for Jack Rebourne to the game engine states and functions.

## 1. Attack Combo System
**Path:** `jack_rebourne_sprites/attack_combo/`
**Function:** `handleCombo()` / `executeAttack(comboCount)`

| Move | Frames | Pathway |
| :--- | :--- | :--- |
| Jab | 4 | `1.jab/frame_001.png` to `frame_004.png` |
| Right Punch | 2 | `2.right_punch/frame_001.png`, `frame_002.png` |
| Right Kick | 3 | `3.right_kick/frame_001.png` to `frame_003.png` |
| Right Uppercut | 3 | `4.right_uppercut/frame_01.png` to `frame_03.png` |
| Left Kick | 2 | `5.left_kick/frame_001.png`, `frame_002.png` |

## 2. Movement & Run
**Path:** `jack_rebourne_sprites/run/`
**Function:** `handleRun()`

- **Frames:** 10 frames (`frame_000.png` to `frame_009.png`)
- **Logic:** Looping animation triggered when horizontal velocity > 0.

## 3. Jump & Air Combat
**Path:** `jack_rebourne_sprites/jump/` & `jump_attack/`
**Functions:** `handleJump()`, `handleJumpAttack()`

- **Jump Up:** `1.jump_up/` (2 frames)
- **Mid-Air:** `2.air/` (3 frames)
- **Landing:** `3.land/` (2 frames)
- **Jump Attack Hit 1:** `jump_attack/hit1/` (2 frames)
- **Jump Attack Hit 2:** `jump_attack/hit2/` (2 frames)

## 4. Vehicle Mode (Bike)
**Path:** `jack_rebourne_sprites/bike/` & `bike_shoot/`
**Function:** `handleBikeState()`

- **Bike Idle/Move:** 15 frames (`frame_000.png` to `frame_014.png`)
- **Bike Shooting:** Organized by burst shots:
  - **Shot 1:** `shot1/frame_001.png` to `frame_003.png`
  - **Shot 2:** `shot2/frame_004.png` to `frame_006.png`
  - **Shot 3:** `shot3/frame_007.png` to `frame_010.png`
  - **Shot 4:** `shot4/frame_011.png` to `frame_014.png`

## 5. Damage & Death
**Path:** `jack_rebourne_sprites/hurt/`
**Function:** `takeDamage()`

- **Hurt Frames:** `frame_001.png` to `frame_005.png`
- **Falling:** `frame_006_fall.png`
- **Death:** `frame_010_death.png`

## 6. Enemy Assets
**Path:** `new_enemies/`

### A. Grunt
- **Walk:** `grunt/grunt_walk/` (7 frames, `frame_000` to `frame_006`)
- **Attack (Kick):** `grunt/grunt_attack/kick/` (3 frames, `000`, `003`, `004`)
- **Attack (Punch):** `grunt/grunt_attack/punch/` (3 frames, `000`, `001`, `002`)

### B. Security
- **Walk:** `security/walk/` (5 frames, `frame_000` to `frame_004`)
- **Attack (Bat):** `security/attack_bat/` (2 frames, `005`, `006`)
- **Attack (Punch):** `security/attack_punch/` (3 frames, `000`, `007`, `008`)

