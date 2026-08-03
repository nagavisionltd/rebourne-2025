# Rebourne Skills Directory

This directory contains reusable skill files that encapsulate standardized patterns, techniques, and best practices for Rebourne game development.

## Purpose
Skills are distilled knowledge that survives model changes and session restarts. Instead of re-explaining concepts, you can simply "load" the relevant skill file.

## Skill Files

### sprite-animation-system.md
Standardized approach for handling sprite animations using delta-time for frame-rate independence and animation state machines.
- Animation state machine implementation
- Delta-time integration patterns
- Finite vs looping animation handling
- Callback systems for animation completion
- Memory management and cleanup

### lane-based-spawning-system.md
Standardized approach for spawning entities in Rebourne's 2.5D lane system.
- Lane configuration and management
- Variable spawn intervals with variance
- Entity tracking and cleanup
- Difficulty scaling patterns
- Different spawner types (enemy, obstacle, pickup)
- Lane selection algorithms (random, avoid player, etc.)

## How to Use Skills
At the start of each work session or when beginning a new task:
1. Review CODEX.md for project-wide standards
2. Review CREATIVE-DNA.md for personal style preferences
3. Load relevant skill files for the specific task at hand
4. Apply the patterns and techniques documented
5. At session end, update SESSION.md and consider if new skills should be created

## Creating New Skills
When you solve a problem or implement a pattern that you'll likely need again:
1. Document the solution in SESSION.md
2. At session end, evaluate if it's reusable
3. If yes, create a new skill file in this directory
4. Follow the established format:
   - Clear purpose statement
   - When to use guidance
   - Implementation pattern with code examples
   - Key rules (always/never)
   - Verification steps
   - Related systems
   - Optional: examples or variations

## Benefits
- Reduces token usage by avoiding re-explanation
- Ensures consistency across sessions and model changes
- Captures lessons learned so mistakes aren't repeated
- Provides quick reference for implementation patterns
- Enables knowledge transfer between team members (or future you)
- Creates a growing library of proven solutions