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

### 3.1 Turn Structure
**Turn Phases**:
1. **Market & Economy** - Buy/sell resources, transactions, worker salaries, resource consumption
2. **Worker Actions** - Move, develop plots, work plots (with resources already available)
3. **Plot Production** - Resources generated based on worker actions

**Turn Flow**:
- Each turn represents a period of development and exploration
- Market activity happens first (buy resources needed for development)
- Workers perform their actions with resources already available
- Plot production follows from worker actions

### 3.2 Game State
Game state is stored as a JSON dictionary in localStorage with the following structure:
- **turn**: Current turn number
- **phase**: Current phase ("market_economy", "worker_actions", "plot_production")
- **plots**: Array of plot objects
- **workers**: Array of worker objects

**Plot Data Structure:**
- Each plot has ratings for each collected and extracted resource
- Owner (player/computer)
- State of undeveloped, farm, mine, power plant, town

**Worker Data Structure:**
- Each worker has a location and type
---

## 4. Resources

### 4.1 Core Resources
- **Money (💰)**: Primary currency for building and trading
- **Energy (⚡)**: Powers advanced buildings and special abilities
- **Food (🍎)**: Sustains population and enables growth
- **Trade Goods (📦)**: Specialized resources for community interaction

### 4.2 Resource Categories

**Collected Resources (Renewable)**:
- **Wind (💨)**: Available in open areas, variable output
- **Sunlight (☀️)**: Available everywhere, varies by location/weather
- **Water (💧)**: Flows naturally, can be collected/directed
- **Soil (🌱)**: Can be improved/regenerated over time

**Extracted Resources (Finite)**:
- **Coal (⛏️)**: High energy density, polluting
- **Stone (🪨)**: Durable building material
- **Ore (⚒️)**: Raw metal source, requires processing

---

## 5. Plot System

### 5.1 Plot Resource Ratings
Each plot has a rating of 0-10 for each resource type:

**Collected Resources (Renewable)**:
- **Wind (💨)**: 0-10 rating based on elevation and exposure
- **Sunlight (☀️)**: 0-10 rating based on latitude and weather patterns
- **Water (💧)**: 0-10 rating based on proximity to rivers/lakes
- **Soil (🌱)**: 0-10 rating based on fertility and drainage

**Extracted Resources (Finite)**:
- **Coal (⛏️)**: 0-10 rating based on geological deposits
- **Stone (🪨)**: 0-10 rating based on rock formations
- **Ore (⚒️)**: 0-10 rating based on mineral deposits

### 5.2 Plot Development
Plots can be developed for specific roles by placing the appropriate worker and providing necessary resources:

- **Farmer** → Creates a **Farm** (requires soil + water)
- **Miner** → Creates a **Mine** (requires stone/coal/ore deposits)
- **Blacksmith** → Creates **Windmill** (requires wind), **Water Mill** (requires water), or **Boiler** (requires coal)
- **Trader** → Creates a **Market** (requires population and trade routes)

### 5.3 Development Mechanics
- **Resource Requirements**: Each development type requires specific resource ratings
- **Worker Specialization**: Only the appropriate worker type can develop each plot type
- **Development Cost**: Credits and materials required to establish the development
- **Production Output**: Higher resource ratings result in better production efficiency

### 5.4 Starting Conditions
**Player Starting Assets:**
- **1 Town**: Starting base of operations
- **TBD Credits**: Starting money (amount to be determined)
- **1 Explorer**: Starting worker for discovery

**World Generation:**
- **Map**: Either read from file OR procedurally generated
- **Computer Towns**: 0 or more existing communities to discover

---

## 6. Worker System

### 6.1 Worker Types
- **Explorer**: Discovers new plots and converts them from unsettled to settled
- **Farmer**: Develops farms, produces agricultural resources
- **Miner**: Develops mines, extracts finite resources
- **Blacksmith**: Creates windmills, steam engines, tools
- **Trader**: Creates markets and manages trade relationships

### 6.2 Worker Mechanics
- **Salary**: 1 credit per turn per worker
- **Movement**: One plot per turn
- **Hiring**: Any settled plot can hire workers (player chooses worker type)
- **Specialization**: Each worker type has specific development abilities

### 6.3 Worker Actions
**Explorer Actions:**
- **Move**: Go to unsettled plot
- **Settle**: Convert plot from unsettled to settled, reveal plot attributes

**Other Worker Actions:**
- **Move**: Go to a settled plot
- **Develop Plot**: Transform settled plot into specific type (requires resources)
- **Work Plot**: Generate production from developed plot (ongoing action)

**Key Rules:**
- **No Worker = No Production**: Undeveloped plots and developed plots without workers produce nothing
- **Worker Allocation**: Each worker can only do one action per turn
- **Strategic Choices**: Balance between exploring/settling, developing new plots, and working existing plots

---

## 7. World & Exploration

### 7.1 Computer-Managed World
- **Initial Environment**: Computer generates starting world and resources
- **Community Discovery**: As player explores, computer reveals existing communities
- **Community Development**: Discovered communities grow and evolve over time
- **Environmental Events**: Market fluctuations, seasonal changes, special events

### 7.2 Community Discovery System
```
Round 1:  "You discover a small fishing village (3 buildings)"
Round 5:  "You find a bustling market town (8 buildings)"
Round 10: "You encounter a major trading city (15 buildings)"
Round 15: "You discover an ancient capital (25 buildings)"
```

### 7.3 Community Interaction
- **Trade Relationships**: Establish markets in discovered communities
- **Cultural Exchange**: Learn new building types from advanced communities
- **Resource Competition**: Compete for limited resources and trade routes
- **Cooperation**: Form alliances and share benefits with communities

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
