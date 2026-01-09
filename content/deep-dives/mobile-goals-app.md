---
title: Task Companion App (Mobile Goals App)
weight: 2
---

# Task Companion App

**A gamified accountability app that transforms task completion into an interactive game experience**

Great for: state management, data persistence, UX tradeoffs, iterative development, scope management

## Overview

Task Companion is a Flutter-based mobile application designed to help users organize tasks on a calendar while staying accountable. The original vision was an ambitious gamification system where real-world productivity drives the growth of a virtual game world, shared between a Player (task owner) and Companion (accountability partner).

**Current Status:** MVP completed with core task management features (30-40% of original vision)

**Repository:** [github.com/phillipw-orayh/task-app](https://github.com/phillipw-orayh/task-app)

## Problem Statement

**How do you make task management engaging enough that users actually stick with it?**

Most productivity apps fail because they're boring—just lists of things to do. Users download them with enthusiasm but abandon them within weeks. I wanted to solve this by:

1. **Adding meaningful accountability** through a companion/partner system
2. **Gamifying progress** to create visual, rewarding feedback loops
3. **Creating hierarchical task relationships** that show how small actions lead to big goals
4. **Making completion feel significant** through world-building mechanics

## Why This Problem Mattered

Personal productivity tools have a retention problem. According to various studies, most task management apps see 80%+ abandonment within the first month. People lose motivation when their progress feels invisible or meaningless.

By combining accountability (social pressure), gamification (intrinsic rewards), and visual progress (immediate feedback), I aimed to create something users would actually want to open every day—not because they *should*, but because they're curious about their world's progress.

## Constraints & Assumptions

### Time Constraints
- Solo developer project during non-work hours
- Needed to ship an MVP within a reasonable timeframe to validate core concept

### Skill Gaps
- First major Flutter/Dart project
- Learning mobile development patterns while building
- Limited experience with complex state management at scale

### Platform Limitations
- Mobile performance considerations (60fps animations, battery life)
- SharedPreferences limitations for large datasets
- Cross-platform compatibility (iOS and Android)

### Scope Boundaries
- Original vision documented 150+ features
- Had to decide what was truly "MVP" vs. "nice to have"
- Risk of building too much before validating core value proposition

## Approach & Development Process

### Phase 1: Vision & Documentation
Started with extensive planning documents outlining the complete vision:
- Three-tier task system: **Action** (7-13 days), **Quest** (14-29 days), **Conquest** (30-90 days)
- Dual-user model (Player creates tasks, Companion provides accountability and rewards)
- Hexagonal tile-based world generation system
- Reward mechanics tied to task completion

### Phase 2: Reality Check
During development, realized the full vision would take 3-6 months of full-time work. Made the strategic decision to:
- **Ship core task management first** to validate market fit
- **Postpone gamification** until proving users want better task management
- **Focus on data architecture** that could support future features

### Phase 3: MVP Implementation
Built core features:
- Task management with status tracking, priorities, and checklists
- Goal tracking with progress calculation from linked tasks
- Project management with milestones and deadlines
- Internal calendar with event visualization
- Journey system supporting Standard, Project, and Simple types
- Member and companion tracking within journeys

### Development Philosophy
Used Claude Code extensively for rapid prototyping and feature implementation, documenting the process as part of my "structured rapid-development workflow" approach.

## Key Decisions & Tradeoffs

### Why Flutter/Dart?
**Decision:** Build with Flutter instead of React Native or native code

**Reasoning:**
- Single codebase for iOS and Android
- Excellent performance for 60fps animations (important for future game elements)
- Strong widget composition model fits task hierarchy UX
- Hot reload speeds up iteration

**Tradeoff:** Smaller ecosystem than React Native, steeper learning curve

### Why SharedPreferences Over Isar Database?
**Decision:** Use SharedPreferences for data persistence in MVP

**Reasoning:**
- Faster initial development
- Adequate for ~200 tasks (most users won't hit this limit immediately)
- Simpler to reason about during prototyping

**Tradeoff:** Performance concerns at scale, will need migration for production

**Future Plan:** Migrate to Isar for better performance and query capabilities

### What We Deliberately Kept Simple
1. **No external calendar integration** (Google Calendar, Apple Calendar) in MVP
2. **Basic internal calendar** instead of full scheduling system
3. **Simple linear task lists** before introducing complex visualizations
4. **Text-based rewards** instead of full game mechanics

### What We Postponed
1. **Entire gamification system** (rewards, achievements, world-building)
2. **Hexagonal tile map generation** and exploration
3. **Companion reward assignment** and penalty system
4. **Randomized battles** and in-game currency
5. **World elements** responding to task completion

## Solution & Implementation

### Core Architecture

**Task Hierarchy:**
```
Conquest (30-90 days)
  └── Quest (14-29 days)
      └── Action (7-13 days)
          └── Checklist Items
```

**Journey System:**
- **Standard Journey:** Full task/goal/project structure
- **Project Journey:** Milestone-based tracking
- **Simple Journey:** Basic task list only

**State Management:**
- Local state persistence via SharedPreferences
- Progress calculation cascades up from completed tasks to parent goals
- Event-driven calendar updates

### Key Features Implemented

1. **Task Management**
   - Status tracking (Not Started, In Progress, Completed, Blocked)
   - Priority levels
   - Checklists with sub-task progress
   - Deadline and reminder support

2. **Goal Tracking**
   - Automatic progress calculation from linked tasks
   - Visual progress indicators
   - Parent-child task relationships

3. **Project Management**
   - Milestone definitions
   - Deadline tracking
   - Progress reporting

4. **Calendar Integration**
   - Internal calendar system
   - Event visualization
   - Task scheduling

### Demo

*Screen recording or walkthrough to be added showing:*
- Creating a new task with checklist
- Linking tasks to goals
- Progress updating automatically
- Calendar view with scheduled tasks

## Iteration & Evolution

### Initial Idea → Current Reality

**Original Vision:**
- Ambitious gamified world-building system
- Dual-user accountability with reward mechanics
- 150+ documented features

**MVP Reality:**
- Core task management (45 features)
- Single-user focused (companion features stubbed)
- Foundation for future gamification

### Key Pivots

1. **Scope Reduction**
   - **Why:** 70% of documented features weren't essential to validate core value
   - **Result:** Shippable MVP in reasonable timeframe

2. **Database Decision**
   - **Initial:** Planned Isar from the start
   - **Reality:** Started with SharedPreferences
   - **Learning:** Premature optimization wastes time; simple works until it doesn't

3. **Calendar Approach**
   - **Initial:** External calendar sync (Google, Apple)
   - **Current:** Internal calendar
   - **Reason:** External sync adds complexity without proving core value first

### Features That Didn't Work

1. **Over-complex Journey Types**
   - Tried to support too many use cases early
   - Simplified to three clear types

2. **Detailed Time Tracking**
   - Added time estimation and tracking
   - Users found it tedious
   - Removed for MVP, may revisit

## Challenges & Lessons Learned

### Challenge 1: Documentation vs. Reality Gap
**Problem:** Documented 150+ features but could only build ~45 for MVP

**Impact:** Created confusion about project status and timeline

**Lesson:** **Separate planning docs from implementation roadmap.** Now organizing documentation into "MVP" vs. "Phase 2+ Roadmap" folders to manage expectations clearly.

### Challenge 2: Performance at Scale
**Problem:** SharedPreferences struggles with >200 tasks

**Impact:** App slows down with heavy use

**Lesson:** **"Adequate for now" has limits.** Should have performance-tested with realistic data volumes earlier. Planning Isar migration before public release.

### Challenge 3: State Management Complexity
**Problem:** Task hierarchy creates cascading state updates

**Impact:** Progress calculations can be expensive

**Lesson:** **Design for lazy evaluation.** Implemented caching and only recalculate progress when relevant tasks change, not on every app open.

### Challenge 4: Scope Creep from Vision
**Problem:** Constantly tempted to add "just one more feature" from the original vision

**Impact:** Delays in shipping MVP

**Lesson:** **Ship to learn, don't build to guess.** The gamification features sound cool, but I won't know if they're valuable until users prove they want better task management first.

## Future Improvements

### Performance Optimizations
- **Migrate to Isar database** for better query performance and scalability
- **Implement lazy loading** for large task lists
- **Add pagination** to calendar views
- **Optimize progress calculation** with smarter caching

### Architecture Upgrades
- **Implement proper state management** (BLoC or Riverpod) instead of setState
- **Add offline-first architecture** with sync capability
- **Introduce data migration** strategy for SharedPreferences → Isar

### Feature Additions (Phase 2+)

**Gamification System:**
- Rewards and achievement tracking
- World-building mechanics with hexagonal tiles
- Visual feedback for task completion
- Player progression system

**Companion Features:**
- Dual-user accountability system
- Reward assignment by companion
- Penalty system for missed deadlines
- Shared progress visibility

**External Integrations:**
- Google Calendar sync
- Apple Calendar sync
- Export/import capabilities
- API for third-party integrations

**Advanced Task Management:**
- Recurring tasks
- Task templates
- Bulk operations
- Advanced filtering and search

### Scaling Considerations
- **Backend infrastructure** for multi-user support
- **Real-time sync** between Player and Companion
- **Push notifications** for deadlines and accountability
- **Analytics** to understand user engagement patterns

## Skills Demonstrated

### Technical Skills
- **Flutter/Dart mobile development** - First major project in the framework
- **State management** - Complex hierarchical state with cascading updates
- **Data persistence** - Local storage with SharedPreferences
- **Calendar systems** - Event scheduling and visualization
- **Cross-platform development** - iOS and Android compatibility

### Product Skills
- **Scope management** - Making hard decisions about MVP vs. full vision
- **Iterative development** - Shipping in phases to validate assumptions
- **Documentation** - Extensive planning docs for complex system
- **User experience design** - Balancing simplicity with power features

### Problem-Solving Skills
- **Performance optimization** - Identifying and addressing bottlenecks
- **Technical tradeoffs** - Choosing simple solutions over perfect ones
- **Architecture planning** - Designing for future extensibility
- **Reality vs. vision management** - Adapting ambitious plans to constraints

### Process Skills
- **Rapid prototyping** with AI-assisted development (Claude Code)
- **Self-directed learning** in new framework (Flutter)
- **Strategic postponement** of features to ship faster
- **Honest self-assessment** of implementation progress

## Key Takeaways

1. **Documentation ≠ Implementation**: Having 150 features documented doesn't mean you need to build them all before shipping.

2. **Validate Before Building**: The gamification features sound amazing, but I won't know if they matter until I prove users want the core task management.

3. **Simple Works Until It Doesn't**: SharedPreferences is fine for MVPs, but have a migration plan before hitting limits.

4. **Scope Discipline Is Hard**: When you love your vision, cutting features feels like giving up—but it's actually how you ship.

5. **AI-Assisted Development Works**: Using structured rapid-development with Claude Code enabled solo development of a complex app in reasonable time.

## Links

- [GitHub Repository](https://github.com/phillipw-orayh/task-app)
- Live Demo: *iOS/Android builds to be added*
- App Store: *Pending public release*
