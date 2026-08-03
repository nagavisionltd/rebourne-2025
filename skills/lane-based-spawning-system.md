# Lane-Based Spawning System

## Purpose
Standardized approach for spawning entities (enemies, obstacles, pickups) in Rebourne's 2.5D lane system, ensuring consistent positioning and balanced gameplay.

## When to Use
- Spawning enemies that should appear in specific lanes matching player position
- Creating obstacles that players must jump over or avoid
- Spawning power-ups, collectibles, or environmental hazards
- Any gameplay element that uses the lane-based positioning system

## Lane System Overview
Rebourne uses a 3-lane system:
- Lane 0 (Top): y = 500
- Lane 1 (Middle): y = 580 (player's starting lane)
- Lane 2 (Bottom): y = 660

All lane-based entities should snap to these exact Y positions for consistent gameplay.

## Implementation Pattern

### 1. Lane Configuration
```javascript
// Define in your scene's init() or create()
this.laneConfig = {
  laneCount: 3,
  laneYPositions: [500, 580, 660], // Top, Middle, Bottom
  laneWidth: 100, // Approximate horizontal space per lane
  // Optional: lane-specific properties
  laneProperties: [
    { name: 'top',   y: 500, difficultyModifier: 0.8 },
    { name: 'middle',y: 580, difficultyModifier: 1.0 },
    { name: 'bottom',y: 660, difficultyModifier: 1.2 }
  ]
};
```

### 2. Spawner Base Class
```javascript
class LaneSpawner {
  constructor(scene, laneConfig) {
    this.scene = scene;
    this.laneConfig = laneConfig;
    this.spawnTimer = 0;
    this.spawnInterval = 2000; // Default milliseconds
    this.spawnVariance = 500;  // ± variance for unpredictability
    this.activeEntities = [];  // Track spawned entities for cleanup
    
    // Get a random lane index (0, 1, or 2)
    this.getRandomLaneIndex = () => 
      Math.floor(Math.random() * this.laneConfig.laneCount);
    
    // Get Y position for a lane index
    this.getLaneY = (laneIndex) => 
      this.laneConfig.laneYPositions[laneIndex] || 
      this.laneConfig.laneYPositions[1]; // Default to middle
    
    // Get a lane that's different from current (for avoidance)
    this.getDifferentLane = (currentLane) => {
      let newLane = this.getRandomLaneIndex();
      while (newLane === currentLane && this.laneConfig.laneCount > 1) {
        newLane = this.getRandomLaneIndex();
      }
      return newLane;
    };
  }
  
  update(deltaTime) {
    this.spawnTimer += deltaTime;
    
    // Calculate variable interval with variance
    const variance = Math.random() * this.spawnVariance * 2 - this.spawnVariance;
    const effectiveInterval = this.spawnInterval + variance;
    
    if (this.spawnTimer >= effectiveInterval) {
      this.spawnTimer = 0;
      this.spawnEntity();
    }
    
    // Update active entities
    this.activeEntities = this.activeEntities.filter(entity => {
      const result = entity.update(deltaTime);
      return result !== 'remove' && result !== 'destroy';
    });
  }
  
  spawnEntity() {
    // To be implemented by subclasses
    throw new Error('spawnEntity() must be implemented by subclass');
  }
  
  // Cleanup method
  destroy() {
    this.activeEntities.forEach(entity => {
      if (entity.destroy) entity.destroy();
    });
    this.activeEntities = [];
  }
}
```

### 3. Enemy Spawner (Example Implementation)
```javascript
class EnemySpawner extends LaneSpawner {
  constructor(scene, laneConfig, enemyFactory) {
    super(scene, laneConfig);
    this.enemyFactory = enemyFactory; // Function that creates enemy instances
    this.spawnInterval = 2500; // Base 2.5 seconds
    this.spawnVariance = 1000; // ±1 second variance
    
    // Difficulty scaling (increases over time)
    this.timeElapsed = 0;
    this.difficultyMultiplier = 1.0;
  }
  
  update(deltaTime) {
    super.update(deltaTime);
    
    // Increase difficulty over time (max 2.0x after 60 seconds)
    this.timeElapsed += deltaTime;
    if (this.timeElapsed < 60000) { // First minute
      this.difficultyMultiplier = 1.0 + (this.timeElapsed / 60000);
    }
    
    // Adjust spawn rate based on difficulty
    this.spawnInterval = 2500 / this.difficultyMultiplier;
  }
  
  spawnEntity() {
    const laneIndex = this.getRandomLaneIndex();
    const y = this.getLaneY(laneIndex);
    
    // Spawn off-screen right
    const x = this.scene.scale.width + 100;
    
    const enemy = this.enemyFactory(this.scene, x, y, laneIndex);
    enemy.setLane(laneIndex); // Store lane reference for AI
    this.activeEntities.push(enemy);
    
    return enemy;
  }
}
```

### 4. Obstacle Spawner (Example Implementation)
```javascript
class ObstacleSpawner extends LaneSpawner {
  constructor(scene, laneConfig, obstacleTypes) {
    super(scene, laneConfig);
    this.obstacleTypes = obstacleTypes; // Array of obstacle constructors
    this.spawnInterval = 2000; // Base 2 seconds
    this.spawnVariance = 500;  // ±500ms variance
  }
  
  spawnEntity() {
    const laneIndex = this.getRandomLaneIndex();
    const y = this.getLaneY(laneIndex);
    
    // Spawn off-screen right
    const x = this.scene.scale.width + 50;
    
    // Randomly select obstacle type
    const ObstacleType = 
      this.obstacleTypes[Math.floor(Math.random() * this.obstacleTypes.length)];
    
    const obstacle = new ObstacleType(this.scene, x, y, laneIndex);
    this.activeEntities.push(obstacle);
    
    return obstacle;
  }
}
```

## Key Rules

### Always:
- Use the exact laneYPositions array for spawning (never hard-code Y values)
- Spawn entities off-screen (right for right-to-left moving games)
- Implement variable spawn intervals with variance for unpredictable gameplay
- Track spawned entities for proper cleanup and memory management
- Provide lane index to spawned entities for AI/lane-changing logic
- Scale spawn frequency and difficulty over time for progression
- Use delta-time for all timing calculations (frame-rate independent)
- Implement cleanup mechanisms to prevent memory leaks
- Standardize spawn points (consistent X offset from screen edge)
- Provide different lane selection algorithms (random, avoid player, etc.)

### Never:
- Hard-code Y positions for lane-based entities (always use lane config)
- Spawn entities on-screen where they might appear suddenly
- Use fixed spawn intervals without variance (creates predictable patterns)
- Forget to clean up spawned entities when they leave screen or are destroyed
- Block the main thread with synchronous spawning operations
- Assume all lanes are equally likely (consider weighting for gameplay balance)
- Spawn entities without checking if the lane is already too crowded
- Use setInterval or setTimeout for spawning (use game loop delta)
- Hard-code lane count (always reference laneConfig.laneCount)

## Verification Steps
1. [ ] Entities spawn exactly at configured lane Y positions (500, 580, 660)
2. [ ] Spawn timing uses delta-time and is consistent across different FPS
3. [ ] Variable intervals create unpredictable but balanced spawning
4. [ ] Entities are properly cleaned up when leaving screen or destroyed
5. [ ] Lane index is correctly passed to spawned entities for AI use
6. [ ] Difficulty scaling increases spawn rate over time appropriately
7. [ ] Different spawner types (enemy, obstacle, pickup) work independently
8. [ ] Memory usage remains stable during extended gameplay
9. [ ] Spawn positions are consistently off-screen regardless of resolution
10. [ ] Lane selection algorithms work correctly (no same-lane repeats when avoiding)

## Integration with Game Logic
- **Player Lane Tracking:** Player should report current lane to spawner for smart spawning
- **Collision Systems:** Entities should report their lane for efficient collision checking
- **Level Progression:** Lane config can be modified per level (different Y positions, lane counts)
- **Visual Debugging:** Optionally render lane markers during development
- **Audio Feedback:** Spawn events can trigger audio cues for audio-design awareness
- **Particle Systems:** Spawn locations can trigger visual effects (dust, flashes, etc.)

## Related Systems
- Asset Loading Pattern: Ensures all entity sprite sheets are preloaded
- Sprite Animation System: Entities use standardized animation approaches
- Hitbox Management: Lane-based entities need properly sized collision boxes
- State Management: Entities should have clear states (spawning, active, dying)
- Object Pooling: High-frequency spawning benefits from entity pooling
- Lane-Changing AI: Enemies should be able to change lanes to pursue player