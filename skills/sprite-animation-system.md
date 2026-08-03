# Sprite Animation System

## Purpose
Standardized approach for handling sprite animations in Rebourne games using delta-time for frame-rate independence and animation state machines.

## When to Use
- Any character, enemy, or object requiring animated sprite sheets
- When animations need to be synchronized with game logic (movement, attacks, states)
- For looping animations (idle, walk, run) and finite animations (attacks, hurt, death)

## Implementation Pattern

### 1. Animation Data Structure
```javascript
// Define animations in create() or init()
this.animations = {
  idle: { frames: ['frame_000.png', 'frame_001.png', 'frame_002.png', 'frame_003.png'], frameRate: 8, loop: true },
  run: { frames: ['frame_000.png'...'frame_009.png'], frameRate: 12, loop: true },
  jab: { frames: ['frame_001.png'...'frame_004.png'], frameRate: 15, loop: false },
  // ... other animations
};
```

### 2. Animation State Machine
```javascript
// In your entity class
class GameEntity {
  constructor(scene, x, y, textureKey) {
    this.scene = scene;
    this.sprite = scene.add.sprite(x, y, textureKey);
    this.currentAnimation = 'idle';
    this.animationTimer = 0;
    this.frameIndex = 0;
    this.isAnimating = false;
    this.animationFinishedCallback = null;
  }
  
  playAnimation(animationName, callback = null, forceRestart = false) {
    const anim = this.animations[animationName];
    if (!anim) return;
    
    // Don't restart if already playing same animation (unless forced)
    if (this.currentAnimation === animationName && this.isAnimating && !forceRestart) return;
    
    this.currentAnimation = animationName;
    this.animationTimer = 0;
    this.frameIndex = 0;
    this.isAnimating = true;
    this.animationFinishedCallback = callback;
    
    // Set first frame immediately
    this.updateSpriteFrame(0);
  }
  
  update(deltaTime) {
    if (!this.isAnimating) return;
    
    this.animationTimer += deltaTime;
    const frameDuration = 1000 / this.animations[this.currentAnimation].frameRate;
    
    if (this.animationTimer >= frameDuration) {
      this.animationTimer -= frameDuration;
      this.frameIndex++;
      
      if (this.frameIndex >= this.animations[this.currentAnimation].frames.length) {
        if (this.animations[this.currentAnimation].loop) {
          this.frameIndex = 0;
        } else {
          this.isAnimating = false;
          if (this.animationFinishedCallback) {
            this.animationFinishedCallback();
            this.animationFinishedCallback = null;
          }
          // Optionally return to idle or previous state
          return 'animation_complete';
        }
      }
      
      this.updateSpriteFrame(this.frameIndex);
    }
  }
  
  updateSpriteFrame(frameIndex) {
    const frame = this.animations[this.currentAnimation].frames[frameIndex];
    this.sprite.setTexture(frame.textureKey || this.sprite.texture.key, frame.frameIndex || 0);
  }
}
```

### 3. Delta-Time Integration
```javascript
// In your scene's update method
update(time, delta) {
  // Update all entities with delta time
  this.player.update(delta);
  this.enemies.forEach(enemy => enemy.update(delta));
  this.obstacles.forEach(obstacle => obstacle.update(delta));
  
  // Handle animation completion events
  const animResult = this.player.update(delta);
  if (animResult === 'animation_complete') {
    // Transition to next state (e.g., attack -> idle)
    this.player.playAnimation('idle');
  }
}
```

## Key Rules

### Always:
- Use delta-time for frame-rate independent animation updates
- Define all animations in a central configuration object
- Implement an animation state machine to manage transitions
- Set the first frame immediately when starting an animation
- Provide callbacks for animation completion when needed
- Standardize frame naming within sprite sheets (000, 001, 002...)
- Use transparent backgrounds on all sprite assets
- Keep frame dimensions consistent within a sprite sheet
- Separate animation logic from game logic (don't mix rendering with gameplay)

### Never:
- Hard-code frame counts in game logic (always reference animation data)
- Mix rendering code with gameplay mechanics in the same functions
- Assume fixed frame rates (always use delta time)
- Reference undefined or non-existent animation frames
- Block the main thread with synchronous animation loading
- Forget to clean up animation timers/state when objects are destroyed
- Use setInterval or setTimeout for animation timing (use game loop delta)

## Verification Steps
1. [ ] All sprite sheets load correctly in preload()
2. [ ] Animations play at correct speed regardless of FPS
3. [ ] Animation state transitions work correctly (idle -> run -> idle)
4. [ ] Finite animations trigger completion callbacks
5. [ ] Looping animations restart seamlessly
6. [ ] Animation doesn't interfere with physics or collision detection
7. [ ] Memory is properly cleaned when entities are destroyed
8. [ ] Works correctly at both 30 FPS and 60 FPS
9. [ ] Sprite sheet frame dimensions match expected hitbox sizes
10. [ ] Animation callbacks fire at the correct frame (not early/late)

## Related Systems
- Asset Loading Pattern: Ensures all sprite sheets are preloaded
- Hitbox Management: Animation frames should align with collision boxes
- State Management: Animation states should sync with entity states (idle, attacking, hurt)
- Object Pooling: Animated entities can be pooled for performance