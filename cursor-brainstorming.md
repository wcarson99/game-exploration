# Game Brainstorming

## Initial Prompt

You are a browser based game designer. You will help me create a specification for a new game, based on the types of games I like. I want you to ask me questions that will help to generate the spec. use a top-down approach.

# Browser-Based Game Preliminary Requirements

## Core Concept
A strategic puzzle/strategy game with 5-minute rounds, designed for mobile play with offline capability and interrupted session support.

## Game Type & Structure
- **Genre**: Puzzle/Strategy with deck-building elements
- **Session Length**: 5-minute rounds with long runs or endless mode
- **Player Count**: Single-player
- **Platform**: Browser-based, mobile-optimized
- **Session Management**: Supports interrupted runs and offline play

## Strategic Elements
- **Multiple Decision Types**: Resource management (short-term/long-term), synergies, combos
- **Resource System**: 
  - Global resources: Money and Energy
  - Unit-specific resources (to be defined)
- **Win Conditions**: Multiple paths to victory (inspired by MTG)
- **Board State**: Focus on building and/or destroying board state (avoiding traditional combat)

## Theme & Atmosphere
- **Setting**: High fantasy OR soft sci-fi
- **Tone**: Mysterious and exploratory OR light/optimistic
- **Narrative**: Adventurer's story theme
- **Story Mode**: Completely deterministic
- **Random Encounters**: Random elements for replayability

## Progression & Longevity
- **Meta-Progression**: None (each run is self-contained)
- **Deterministic Mode**: Story mode with no randomness
- **Random Mode**: Random encounters for variety
- **Long-term Engagement**: To be explored further

## Technical Requirements
- **Mobile Compatibility**: Must work on mobile devices
- **Offline Play**: Full offline capability
- **Session Persistence**: Save progress for interrupted runs
- **Control Scheme**: TBD (tap/click, drag/drop, swipe, or minimal input)

## Inspiration & References
- **Primary**: Slay the Spire, Dawncaster
- **Secondary**: Magic: The Gathering (multiple win conditions, board state manipulation)
- **Avoid**: Traditional combat mechanics

## Open Questions
- Specific win conditions
- Unit-specific resource types
- Visual style and interface preferences
- Time management within 5-minute rounds
- Exploration mechanics
- Long-term engagement drivers

# Browser-Based Game Detailed Requirements

## Style Decisions (Confirmed)
- **Turn Structure**: Simultaneous turns - all players/actions happen at once
- **Core Gameplay**: Hybrid approach with board/tableau development
- **Visual Style**: Minimalist geometric (best for free sprites/animations)
- **Interaction**: Tap/click with drag & drop for card/component manipulation

## Refined Gameplay Elements
- **Tableau Building**: Players develop a personal board state through strategic placement
- **Simultaneous Action**: All players make decisions simultaneously, then resolve together
- **Hybrid Mechanics**: Combines card play, resource management, and strategic positioning
- **Visual Approach**: Clean geometric shapes, simple animations, scalable design

## Implementation Concepts

### Tableau Structure
```
Player Board Layout:
┌─────────────────────────────────┐
│ Resource Row: [Money] [Energy]  │
│ Card Row: [Hand] [Played] [Discard] │
│ Tableau Grid: 3x3 or 4x4 grid  │
│ ┌─┬─┬─┐                        │
│ │ │ │ │  (Cards placed here)   │
│ ├─┼─┼─┤                        │
│ │ │ │ │                        │
│ └─┴─┴─┘                        │
└─────────────────────────────────┘
```

### Simultaneous Action Flow
1. **Planning Phase** (30 seconds): All players select actions
2. **Resolution Phase** (10 seconds): Actions resolve simultaneously
3. **Tableau Update** (20 seconds): New cards/effects apply to boards

### Hybrid Mechanics Examples
- **Cards** → Provide resources, abilities, or tableau pieces
- **Resources** → Fuel card play and tableau expansion
- **Tableau** → Generates ongoing effects and victory points
- **Exploration** → Reveals new cards and opportunities

## Core Game Concept: Cute Civilization Builder

### Theme & Setting
- **Cute aesthetic**: Adorable buildings, friendly characters, warm colors
- **Economic focus**: No combat, emphasis on trade and development
- **Grid-based civilization**: Build and manage a personal civilization on a grid
- **Exploration-driven**: Discover existing communities in the world

### Gameplay Overview
- **Goal**: Build a thriving civilization that continues and grows
- **Core loop**: Explore → Discover communities → Trade → Build → Achieve
- **Session length**: 5-minute rounds with longer runs or endless mode
- **Turn structure**: Simultaneous planning and resolution phases

### Grid-Based Civilization Structure
```
Player Civilization Layout:
┌─────────────────────────────────┐
│ Resource Row: [Money] [Energy] [Population] │
│ ┌─┬─┬─┬─┐                      │
│ │🏠│🏭│🌾│🏪│  (Your civilization) │
│ ├─┼─┼─┼─┤                      │
│ │🌾│🏭│🏠│🌾│                     │
│ ├─┼─┼─┼─┤                      │
│ │🏪│🏠│🏭│🌾│                     │
│ └─┴─┴─┴─┘                      │
└─────────────────────────────────┘
```

### Building Types & Economy
- **Houses (🏠)**: Generate population and income
- **Factories (🏭)**: Convert resources to money
- **Farms (🌾)**: Produce food and raw materials
- **Markets (🏪)**: Generate trade income and enable commerce
- **Special buildings**: Create synergies and unique effects

### Computer as World Manager
- **Initial environment**: Computer generates starting world and resources
- **Community discovery**: As player explores, computer reveals existing communities
- **Community development**: Discovered communities grow and evolve over time
- **Environmental events**: Market fluctuations, seasonal changes, special events

### Community Discovery System
```
Round 1:  "You discover a small fishing village (3 buildings)"
Round 5:  "You find a bustling market town (8 buildings)"
Round 10: "You encounter a major trading city (15 buildings)"
Round 15: "You discover an ancient capital (25 buildings)"
```



### Achievement System
- **Size achievements**: "Reach 16 buildings", "Build a metropolis"
- **Production achievements**: "Generate 100 gold in one turn"
- **Longevity achievements**: "Survive 20 turns without bankruptcy"
- **Efficiency achievements**: "Have 3 markets connected"
- **Variety achievements**: "Build one of each building type"
- **Exploration achievements**: "Discover 5 different communities"

### Resource Management
- **Money**: Primary currency for building and trading
- **Energy**: Powers advanced buildings and special abilities
- **Population**: Limits building capacity and generates income
- **Food**: Sustains population and enables growth
- **Trade goods**: Specialized resources for community interaction

### Community Interaction
- **Trade relationships**: Establish markets in discovered communities
- **Cultural exchange**: Learn new building types from advanced communities
- **Resource competition**: Compete for limited resources and trade routes
- **Cooperation**: Form alliances and share benefits with communities

### Game Economy

**Resource Categories:**

**Collected Resources (Renewable):**
- **Wind (💨)**: Available in open areas, variable output
- **Sunlight (☀️)**: Available everywhere, varies by location/weather
- **Water (💧)**: Flows naturally, can be collected/directed
- **Soil (🌱)**: Can be improved/regenerated over time

**Extracted Resources (Finite):**
- **Coal (⛏️)**: High energy density, polluting
- **Stone (🪨)**: Durable building material
- **Ore (⚒️)**: Raw metal source, requires processing

**Currency:**
- **Credits (💰)**: Primary currency for trade and expansion

**Production Chains:**

**Energy Production:**
- Coal → Energy (high output, polluting)
- Wind → Energy (variable, clean)
- Sunlight → Energy (reliable, clean)

**Material Production:**
- Coal + Ore → Metal (requires energy)
- Stone → Building materials
- Wood → Building materials (from forests)

**Resource Flow:**
```
Collected: Wind/Sunlight + Water → Energy
Extracted: Coal + Ore → Metal (using Energy)
Combined: Energy + Materials → Advanced buildings
```

**Strategic Implications:**

**Location Matters:**
- Windy areas: Better wind energy production
- Sunny areas: Better solar energy production
- Mountainous areas: Access to coal, stone, ore
- River areas: Water access, fertile soil

**Resource Scarcity:**
- Coal and ore are finite - creates tension
- Wind/sunlight are renewable but location-dependent
- Metal production requires both finite resources

**Emergent Possibilities:**
- Energy-rich areas can process more ore into metal
- Metal scarcity creates trade opportunities
- Different energy sources create different environmental impacts
- Resource depletion forces adaptation

## Turn Structure

### Turn Phases:
1. **Update Plots** - Production, market transactions, development
2. **Hire/Fire Workers** - Manage workforce
3. **Move Workers** - Position workers for next turn

## Workers

### Worker Types:
- **Explorer** - Discovers new plots and hires local workers
- **Farmer** - Develops farms, produces agricultural resources  
- **Miner** - Develops mines, extracts finite resources
- **Blacksmith** - Creates windmills, steam engines, tools
- **Builder** - Develops towns and infrastructure

### Worker Mechanics:
- **Salary**: Paid in credits per turn
- **Movement**: One plot per turn
- **Hiring**: Explorers can hire local workers in discovered plots
- **Specialization**: Each worker type has specific development abilities

### Starting Conditions:
- **Starting Credits**: Enough to hire explorer + farmer/miner + buy initial resources
- **Starting Plot**: One developed town
- **Bootstrap Strategy**: Explorer discovers → Hire local worker → Develop plot → Generate resources