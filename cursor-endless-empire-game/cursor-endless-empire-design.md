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

## 7. Map & World System

*For detailed map and world system specifications, see [map-design.md](map-design.md)*

**Overview**: The map system manages the grid-based world structure, procedural world generation, community discovery, and turn management. It serves as the foundation that connects all other game systems.

**Key Concepts**:
- **Grid Structure**: 2D grid of plots with adjacency and boundaries
- **World Generation**: Procedural generation with seed-based determinism
- **Community Discovery**: Computer-managed communities revealed through exploration
- **System Coordination**: Integrates with GameState for turn progression and state management

---

## 8. Achievement & Progression System

### 8.1 Achievement Categories
- **Size Achievements**: "Reach 16 buildings", "Build a metropolis"
- **Production Achievements**: "Generate 100 gold in one turn"
- **Longevity Achievements**: "Survive 20 turns without bankruptcy"
- **Efficiency Achievements**: "Have 3 markets connected"
- **Variety Achievements**: "Build one of each building type"
- **Exploration Achievements**: "Discover 5 different communities"

### 8.2 Multiple Paths to Success
- **Economic Victory**: Achieve massive wealth
- **Cultural Victory**: Establish trade with all communities
- **Efficiency Victory**: Maximize resource production
- **Exploration Victory**: Discover the entire world
- **Longevity Victory**: Survive and thrive for extended periods

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

## 10. Visual Design

### 10.1 Art Style
- **Aesthetic**: Cute, friendly, warm colors
- **Style**: Minimalist geometric with rounded edges
- **Palette**: Warm earth tones, pastels, vibrant accents
- **Animation**: Simple, smooth, delightful micro-interactions

### 10.2 UI/UX Principles
- **Clarity**: Clear visual hierarchy and information architecture
- **Accessibility**: High contrast, readable fonts, scalable interface
- **Delight**: Satisfying feedback, smooth transitions, playful elements
- **Efficiency**: Minimal clicks/taps to complete actions

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

### 14.1 Phase 1: Core Prototype
- Basic grid system and building placement
- Simple resource management
- Turn structure implementation
- Basic UI framework

### 14.2 Phase 2: Core Gameplay
- Worker system implementation
- Community discovery mechanics
- Achievement system
- Basic audio and visual polish

### 14.3 Phase 3: Polish & Features
- Advanced building synergies
- Environmental events
- Mobile optimization
- Offline capability

### 14.4 Phase 4: Launch Preparation
- Performance optimization
- User testing and feedback
- Bug fixes and balance adjustments
- Launch marketing materials

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

## 18. Appendix

### 18.1 Inspiration Sources
- **Slay the Spire**: Deck-building and strategic depth
- **Dawncaster**: Mobile-optimized card game design
- **Magic: The Gathering**: Multiple win conditions
- **Civilization**: 4X elements and progression
- **SimCity**: Economic building and management

### 18.2 Technical References
- **HTML5 Game Development**: Best practices and frameworks
- **Mobile Game Design**: Touch interface and performance optimization
- **Progressive Web Apps**: Offline capability and installation
- **Game Balance**: Economic systems and progression design

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Next Review: [Date + 2 weeks]*
