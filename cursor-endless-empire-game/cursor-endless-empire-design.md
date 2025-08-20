# Cute Civilization Builder - Game Design Document

## 1. Executive Summary

**Game Title**: Endless Empire  
**Genre**: Strategic Puzzle/Strategy with Economic Focus  
**Platform**: Browser-based, Mobile-optimized  
**Session Length**: 5-minute rounds with longer runs or endless mode  
**Target Audience**: Casual to mid-core strategy players  

**Core Concept**: A browser-based civilization builder where players manage a cute, grid-based civilization through exploration, trade, and economic development. No combat - pure economic strategy with discovery mechanics.

---

## 2. Game Overview

### 2.1 High-Level Concept
Players build and manage a civilization on a grid, discovering existing communities in a computer-managed world. Success comes through economic development, trade relationships, and strategic resource management rather than conquest.

### 2.2 Core Pillars
- **Exploration-Driven**: Discover communities that develop over time
- **Economic Focus**: No combat, emphasis on trade and development
- **Cute Aesthetic**: Adorable buildings, friendly characters, warm colors
- **Strategic Depth**: Multiple paths to victory through different playstyles
- **Accessible**: 5-minute rounds make it easy to pick up and play

### 2.3 Unique Selling Points
- Computer-managed world that evolves independently
- Multiple achievement paths rather than single win condition
- Offline capability with session persistence
- Mobile-optimized with touch-friendly controls

---

## 3. Core Gameplay Mechanics

### 3.1 Turn Structure & Game State

*For detailed turn structure and game state specifications, see [game-state-design.md](game-state-design.md)*

**Overview**: The game uses a three-phase turn system with centralized state management. GameState serves as the single source of truth for all game data and coordinates between all systems.

**Key Concepts**:
- **Three-Phase Turns**: Market & Economy → Worker Actions → Plot Production
- **Centralized State**: GameState manages all game data and system coordination
- **Persistence**: Automatic save/load to localStorage with JSON serialization
- **State Validation**: Data integrity checks and error recovery
- **System Integration**: Coordinates Map, Market, Workers, and Plots
---

## 4. Market & Resources

*For detailed market and resource system specifications, see [market-design.md](market-design.md)*

**Overview**: The market system provides a centralized economic hub where players buy and sell resources. It features dynamic pricing based on supply and demand, creating strategic economic decisions.

**Key Concepts**:
- **Central Market**: Single trading hub for all resource transactions
- **Dynamic Pricing**: Prices adjust based on supply and demand
- **Resource Types**: 7 resource types (Wind, Sunlight, Water, Soil, Coal, Stone, Ore)
- **Trading Interface**: Plots can buy/sell resources through market methods
- **Economic Balance**: Market never runs out of resources or money

---

## 5. Plot System

*For detailed plot system specifications, see [plot-design.md](plot-design.md)*

**Overview**: Plots are the building blocks of the game world, each with inherent resource potential that determines what can be built and how efficiently it can produce resources.

**Key Concepts**:
- **Plot States**: Unsettled → Settled → Developed
- **Resource Ratings**: 0-10 ratings for 7 resource types (Wind, Sunlight, Water, Soil, Coal, Stone, Ore)
- **Development Types**: Farm, Mine, Windmill, Water Mill, Boiler, Market, Town
- **Worker Requirements**: Each development type requires specific worker types
- **Production System**: Developed plots generate resources based on ratings and worker presence

**Starting Conditions**:
- **Player Assets**: 1 Town, TBD Credits, 1 Explorer
- **World Generation**: Procedurally generated or read from file
- **Computer Towns**: 0 or more existing communities to discover

---

## 6. Worker System

*For detailed worker system specifications, see [worker-design.md](worker-design.md)*

**Overview**: Workers are specialized units that can be hired at settled plots to perform specific actions. Each worker type has unique abilities and can only perform certain development and production tasks.

**Key Concepts**:
- **5 Worker Types**: Explorer, Farmer, Miner, Blacksmith, Trader
- **Action System**: One action per turn per worker (move, settle, develop, work)
- **Specialization**: Each worker type can only develop specific plot types
- **Economic Cost**: 1 credit salary per turn per worker
- **Strategic Depth**: Balance between exploration, development, and production

---

## 7. Software Architecture

### 7.1 System Overview

The game follows a modular architecture with GameState as the central coordinator. All systems interact through well-defined interfaces, with clear separation of concerns and data flow.

**Core Architecture:**
```
┌─────────────────────────────────────────────────────────────┐
│                        UI Layer                             │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │   Welcome   │ │ Turn Phases │ │ Game Over   │           │
│  │   Scene     │ │   Scenes    │ │   Scene     │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    GameState (Central)                      │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ Turn Mgmt   │ │ State       │ │ Persistence │           │
│  │ & Phases    │ │ Validation  │ │ & Save/Load │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
└─────────────────────────────────────────────────────────────┘
                              │
                    ┌─────────┼─────────┐
                    ▼         ▼         ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│    Map      │ │   Market    │ │   Workers   │ │    Plots    │
│  System     │ │   System    │ │   System    │ │   System    │
└─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘
```

### 7.2 System Relationships

**Data Flow Architecture:**
```
UI Scenes
    │
    ▼
GameState (Central Coordinator)
    │
    ├─── Map System
    │    ├─── Grid Management
    │    ├─── World Generation
    │    └─── Community Discovery
    │
    ├─── Market System
    │    ├─── Resource Trading
    │    ├─── Dynamic Pricing
    │    └─── Transaction Management
    │
    ├─── Worker System
    │    ├─── Worker Management
    │    ├─── Action Processing
    │    └─── Movement & Development
    │
    └─── Plot System
         ├─── Plot States
         ├─── Resource Ratings
         └─── Production Management
```

### 7.3 Component Responsibilities

**GameState (Central Coordinator):**
- Manages turn progression and phases
- Coordinates all system interactions
- Handles state persistence and validation
- Provides single source of truth for game data

**Map System:**
- Manages 2D grid structure
- Handles world generation and procedural content
- Tracks community discovery and development
- Provides spatial relationships and pathfinding

**Market System:**
- Manages resource trading and pricing
- Handles supply/demand dynamics
- Processes transactions and economic balance
- Provides resource availability information

**Worker System:**
- Manages worker types and actions
- Handles worker movement and development
- Processes worker salaries and costs
- Coordinates worker-plot interactions

**Plot System:**
- Manages plot states and development
- Handles resource ratings and production
- Processes plot development requirements
- Coordinates plot-worker interactions

**UI System:**
- Manages scene transitions and user interaction
- Displays game state and feedback
- Handles input processing and validation
- Provides help system and navigation

### 7.4 Data Flow Patterns

**Turn Progression Flow:**
```
1. UI Scene → GameState.advancePhase()
2. GameState → Market.updatePrices()
3. GameState → Workers.processActions()
4. GameState → Plots.generateProduction()
5. GameState → Map.updateCommunities()
6. GameState.save() → localStorage
7. GameState → UI Scene (next phase)
```

**State Persistence Flow:**
```
GameState.save()
    │
    ├─── Serialize all system states
    │    ├─── Map.grid → JSON
    │    ├─── Market.resources → JSON
    │    ├─── Workers[] → JSON
    │    └─── Plots[] → JSON
    │
    └─── localStorage.setItem()
```

**System Integration Flow:**
```
Worker.move(targetPlot)
    │
    ├─── Map.isValidPosition(targetPlot)
    ├─── Plot.canAcceptWorker(worker)
    ├─── GameState.validateAction(worker, action)
    └─── GameState.updateWorkerPosition(worker, targetPlot)
```

### 7.5 Implementation Strategy

**Phase 1: Core Systems (Bottom-Up)**
```
1. Plot System (Foundation)
   ├─── Plot states and resource ratings
   ├─── Basic plot operations
   └─── Plot validation

2. Worker System (Building on Plots)
   ├─── Worker types and actions
   ├─── Worker-plot interactions
   └─── Action validation

3. Market System (Economic Layer)
   ├─── Resource trading interface
   ├─── Dynamic pricing
   └─── Transaction processing

4. Map System (Spatial Layer)
   ├─── Grid management
   ├─── World generation
   └─── Community discovery

5. GameState (Central Coordinator)
   ├─── State management
   ├─── Turn progression
   └─── System coordination
```

**Phase 2: UI Layer (Top-Down)**
```
1. GameState Integration
   ├─── State observation
   ├─── Action dispatching
   └─── Scene transitions

2. Scene Implementation
   ├─── Welcome scene
   ├─── Turn phase scenes
   └─── Game over scene

3. UI Components
   ├─── Grid rendering
   ├─── Resource displays
   └─── Help system
```

### 7.6 Code Organization

**File Structure:**
```
src/
├── core/
│   ├── GameState.ts          # Central state management
│   ├── Map.ts               # World and grid management
│   ├── Market.ts            # Resource trading system
│   ├── Worker.ts            # Worker management
│   └── Plot.ts              # Plot management
├── ui/
│   ├── scenes/
│   │   ├── WelcomeScene.ts
│   │   ├── MarketScene.ts
│   │   ├── WorkerScene.ts
│   │   ├── ProductionScene.ts
│   │   └── GameOverScene.ts
│   ├── components/
│   │   ├── Grid.ts
│   │   ├── ResourceDisplay.ts
│   │   └── HelpModal.ts
│   └── utils/
│       ├── InputHandler.ts
│       └── SceneManager.ts
├── types/
│   ├── GameState.ts         # TypeScript interfaces
│   ├── Resources.ts
│   └── Workers.ts
└── utils/
    ├── Persistence.ts       # Save/load utilities
    ├── Validation.ts        # State validation
    └── Constants.ts         # Game constants
```

**Module Dependencies:**
```
UI Scenes
    │
    ▼
GameState (imports all core systems)
    │
    ├─── Map (imports Plot)
    ├─── Market (standalone)
    ├─── Worker (imports Plot)
    └─── Plot (standalone)
```

---

## 8. Map & World System

*For detailed map and world system specifications, see [map-design.md](map-design.md)*

**Overview**: The map system manages the grid-based world structure, procedural world generation, community discovery, and turn management. It serves as the foundation that connects all other game systems.

**Key Concepts**:
- **Grid Structure**: 2D grid of plots with adjacency and boundaries
- **World Generation**: Procedural generation with seed-based determinism
- **Community Discovery**: Computer-managed communities revealed through exploration
- **System Coordination**: Integrates with GameState for turn progression and state management

---

## 8. Achievement & Progression System

*For detailed achievement and progression specifications, see [game-state-design.md](game-state-design.md) section 9.4*

**Overview**: The game features multiple achievement paths and victory conditions, tracked through the GameState system.

**Key Concepts**:
- **Achievement Categories**: Size, Production, Longevity, Efficiency, Variety, Exploration
- **Multiple Victory Paths**: Economic, Cultural, Efficiency, Exploration, Longevity
- **Progressive Unlocking**: Achievements unlock new content and abilities
- **Record Tracking**: Personal best runs and performance metrics

---

## 9. Technical Requirements

### 9.1 Platform Specifications
- **Game Engine**: Phaser 3 with TypeScript
- **Target Device**: iPhone 14 (A14 Bionic, 6GB RAM)
- **Data Persistence**: localStorage with JSON state structure
- **Mobile-optimized**: Responsive design, touch-friendly

### 9.2 Performance Requirements
- **Load time**: < 5 seconds on mobile
- **Frame rate**: 30 FPS for smooth animations
- **Memory usage**: < 100MB on mobile devices
- **Battery efficiency**: Optimized for mobile play

### 9.3 Control Scheme
- **Primary**: Tap/click for selection and actions
- **Secondary**: Drag & drop for building placement
- **Mobile**: Touch gestures for zoom and pan
- **Accessibility**: Keyboard navigation support

---

## 10. UI/UX Design

*For detailed UI/UX specifications, see [ui-design.md](ui-design.md)*

**Overview**: The UI follows a mobile-first, MVP approach with three core scenes and minimal complexity.

**Key Concepts**:
- **MVP Scene Structure**: Welcome, Turn Phases, Game Over
- **Mobile-First Design**: Touch-friendly, responsive layout
- **Help System**: Static help menu accessible from any scene
- **Record Tracking**: Previous best runs displayed in welcome scene

---

## 11. Audio Design

### 11.1 Sound Effects
- **Building placement**: Satisfying "plop" sounds
- **Resource collection**: Gentle chimes and rustles
- **Discovery**: Magical sparkle sounds
- **Achievement**: Celebratory fanfares

### 11.2 Music
- **Ambient**: Calm, peaceful background music
- **Dynamic**: Music changes based on game state
- **Mood**: Uplifting and optimistic tone
- **Variety**: Different tracks for different biomes/communities

---

## 12. Game Modes

### 12.1 Quick Play (5-minute rounds)
- **Objective**: Achieve specific goals within time limit
- **Scoring**: Points based on achievements and efficiency
- **Replayability**: Random starting conditions and events

### 12.2 Endless Mode
- **Objective**: Build the largest, most efficient civilization
- **Progression**: Unlock new building types and abilities
- **Challenge**: Increasing difficulty and resource scarcity

### 12.3 Story Mode
- **Objective**: Complete narrative-driven scenarios
- **Deterministic**: No randomness, focused on strategic puzzles
- **Progression**: Unlock new story chapters and challenges

---

## 13. Monetization Strategy

### 13.1 Free-to-Play Model
- **Core Game**: Completely free with full functionality
- **Cosmetics**: Optional building skins and themes
- **Convenience**: Save slots and quick-start options
- **No Pay-to-Win**: All gameplay advantages available through play

### 13.2 Premium Features
- **Visual Themes**: Seasonal and special event themes
- **Sound Packs**: Additional music and sound effect sets
- **Advanced Analytics**: Detailed statistics and insights
- **Cloud Saves**: Cross-device synchronization

---

## 14. Development Roadmap

### 14.1 Phase 1: Core Systems Implementation
- **GameState System**: Central state management and persistence
- **Map System**: Grid structure and world generation
- **Worker System**: Worker types, actions, and mechanics
- **Plot System**: Plot states, development, and production
- **Market System**: Resource trading and dynamic pricing

### 14.2 Phase 2: UI Implementation
- **Scene Management**: Welcome, Turn Phases, Game Over scenes
- **Mobile Optimization**: Touch-friendly interface and responsive design
- **Help System**: Static help menu and documentation
- **Record Tracking**: Achievement display and personal best tracking

### 14.3 Phase 3: Integration & Polish
- **System Integration**: Connect all systems through GameState
- **Performance Optimization**: Mobile performance and memory management
- **Audio & Visual Polish**: Sound effects, music, and visual feedback
- **Testing & Balance**: Gameplay testing and economic balance

### 14.4 Phase 4: Launch Preparation
- **Performance Optimization**: 60fps on target devices
- **User Testing**: Mobile UX validation and feedback
- **Bug Fixes**: Critical issues and edge case handling
- **Launch Materials**: Marketing assets and documentation

---

## 15. Success Metrics

### 15.1 Engagement Metrics
- **Session Length**: Average 15-20 minutes per session
- **Retention**: 40% Day 1, 20% Day 7, 10% Day 30
- **Completion Rate**: 70% of players complete tutorial
- **Achievement Rate**: 60% of players earn at least one achievement

### 15.2 Technical Metrics
- **Load Time**: < 3 seconds on mobile
- **Crash Rate**: < 1% of sessions
- **Performance**: 60 FPS on target devices
- **Accessibility**: WCAG 2.1 AA compliance

---

## 16. Risk Assessment

### 16.1 Technical Risks
- **Browser Compatibility**: Ensuring cross-browser functionality
- **Mobile Performance**: Optimizing for lower-end devices
- **Offline Implementation**: Complex service worker setup
- **Data Management**: Efficient save/load system

### 16.2 Design Risks
- **Complexity Balance**: Maintaining accessibility while providing depth
- **Pacing Issues**: Ensuring 5-minute rounds feel satisfying
- **Balance Problems**: Resource economy and building synergies
- **Discovery Mechanics**: Making exploration feel rewarding

### 16.3 Market Risks
- **Competition**: Standing out in crowded casual strategy market
- **Platform Changes**: Browser and mobile OS updates
- **User Acquisition**: Reaching target audience effectively
- **Monetization**: Finding right balance for free-to-play model

---

## 17. To Do

### 17.1 Development Priorities
- **Evaluate PWA** - Consider adding Progressive Web App features for offline capability and home screen installation
- **Performance optimization** - Ensure 60fps on iPhone 14 target device
- **Mobile UX testing** - Validate touch interface and screen layout
- **Balance testing** - Fine-tune resource economy and progression

### 17.2 Future Enhancements
- **Additional building types** - Expand variety of developments
- **Community AI improvements** - More sophisticated computer-managed world
- **Multiplayer features** - Cooperative or competitive play
- **Seasonal events** - Dynamic world changes and challenges

---

## 18. Detailed System Documentation

### 18.1 Core System Documents
- **[GameState Design](game-state-design.md)**: Central state management, turn structure, persistence
- **[Map Design](map-design.md)**: World generation, community discovery, grid management
- **[Worker Design](worker-design.md)**: Worker types, actions, mechanics, and strategy
- **[Plot Design](plot-design.md)**: Plot states, development, resource ratings, production
- **[Market Design](market-design.md)**: Resource trading, dynamic pricing, economic balance
- **[UI Design](ui-design.md)**: Scene structure, mobile interface, help system

### 18.2 Documentation Strategy
- **Modular Design**: Each system has its own focused design document
- **Cross-References**: Documents reference each other for integration details
- **Implementation Ready**: Each document includes TypeScript interfaces and methods
- **LLM Optimized**: Structured for effective code generation and prototyping

### 18.3 Development Guidance
- **Start with GameState**: Implement central state management first
- **System by System**: Build each system using its focused design document
- **Integration Testing**: Use cross-references to ensure proper system interaction
- **UI Last**: Implement UI after core systems are functional

---

## 19. Appendix

### 19.1 Inspiration Sources
- **Slay the Spire**: Deck-building and strategic depth
- **Dawncaster**: Mobile-optimized card game design
- **Magic: The Gathering**: Multiple win conditions
- **Civilization**: 4X elements and progression
- **SimCity**: Economic building and management

### 19.2 Technical References
- **HTML5 Game Development**: Best practices and frameworks
- **Mobile Game Design**: Touch interface and performance optimization
- **Progressive Web Apps**: Offline capability and installation
- **Game Balance**: Economic systems and progression design

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Next Review: [Date + 2 weeks]*
