# Map System Design Document

## Overview

The Map system is a fundamental component of the Endless Empire game, managing the grid-based world structure, procedural world generation, community discovery, and turn management. The map serves as the foundation that connects all other game systems together.

## Core Concept

The map represents a dynamic, procedurally generated world that players explore and develop. It manages a grid of plots, handles world generation, tracks discovered communities, and coordinates the turn-based game flow. The map evolves over time as players explore and as computer-managed communities develop independently.

---

## Map Structure

### Grid System
- **2D Grid**: Rectangular grid of plots (width × height)
- **Plot Coordinates**: Each plot has (x, y) coordinates
- **Adjacency**: Plots are adjacent if they share an edge (not corners)
- **Boundaries**: Grid has defined boundaries (no wrapping)

### Plot Organization
- **Plot Array**: 2D array of Plot objects
- **Plot States**: Each plot can be unsettled, settled, or developed
- **Plot Ownership**: Plots can belong to player, computer, or be unsettled
- **Plot Discovery**: Unsettled plots are hidden until discovered

### World Dimensions
- **Default Size**: Configurable grid dimensions
- **Scalability**: Should support various world sizes
- **Performance**: Optimized for mobile devices
- **Memory**: Efficient storage of plot data

---

## World Generation

### Procedural Generation
- **Seed-Based**: Deterministic generation from seed value
- **Resource Distribution**: Realistic placement of resource deposits
- **Geographic Features**: Rivers, mountains, forests, etc.
- **Community Placement**: Strategic placement of computer communities

### Generation Parameters
- **World Size**: Width and height of the grid
- **Resource Density**: How common each resource type is
- **Community Count**: Number of computer-managed communities
- **Difficulty Scaling**: Resource distribution based on difficulty

### Resource Distribution
- **Clustering**: Related resources grouped together
- **Realistic Geography**: Resources placed based on logical factors
- **Balance**: Ensure all resource types are available
- **Scarcity**: Some resources limited to create strategic choices

### Community Generation
- **Community Types**: Fishing villages, market towns, trading cities, capitals
- **Development Level**: Communities start at different development stages
- **Resource Specialization**: Communities focus on specific resources
- **Strategic Placement**: Communities positioned for meaningful interaction

---

## Community Discovery System

### Discovery Mechanics
- **Exploration Required**: Communities only revealed through player exploration
- **Progressive Revelation**: Communities discovered as player expands
- **Development Tracking**: Communities grow and evolve over time
- **Interaction Opportunities**: Discovered communities enable trade and cooperation

### Community Types
```
Round 1:  "You discover a small fishing village (3 buildings)"
Round 5:  "You find a bustling market town (8 buildings)"
Round 10: "You encounter a major trading city (15 buildings)"
Round 15: "You discover an ancient capital (25 buildings)"
```

### Community Development
- **Independent Growth**: Communities develop without player intervention
- **Time-Based Evolution**: Communities grow more advanced over time
- **Resource Influence**: Community development affected by local resources
- **Interaction Effects**: Player actions can influence community development

### Community Interaction
- **Trade Relationships**: Establish markets in discovered communities
- **Cultural Exchange**: Learn new building types from advanced communities
- **Resource Competition**: Compete for limited resources and trade routes
- **Cooperation**: Form alliances and share benefits with communities

---

## Turn Management

### Turn Structure
**Turn Phases**:
1. **Market & Economy** - Buy/sell resources, transactions, worker salaries, resource consumption
2. **Worker Actions** - Move, develop plots, work plots (with resources already available)
3. **Plot Production** - Resources generated based on worker actions

### Turn Flow
- Each turn represents a period of development and exploration
- Market activity happens first (buy resources needed for development)
- Workers perform their actions with resources already available
- Plot production follows from worker actions

### Phase Management
- **Current Phase**: Track which phase the game is in
- **Phase Transitions**: Automatic progression through phases
- **Action Validation**: Ensure actions are performed in correct phase
- **State Persistence**: Save game state between phases

---

## Map Class Implementation

### Properties
```typescript
interface Map {
  // Grid structure
  width: number;
  height: number;
  grid: Plot[][];
  
  // Game state
  turn: number;
  phase: GamePhase; // "market_economy" | "worker_actions" | "plot_production"
  gameState: GameState;
  
  // World management
  discoveredCommunities: Community[];
  worldEvents: WorldEvent[];
  environmentalConditions: EnvironmentalConditions;
  
  // Generation parameters
  seed: number;
  generationParams: WorldGenerationParams;
  
  // Performance tracking
  lastUpdateTime: number;
  updateCount: number;
}

interface Community {
  id: string;
  name: string;
  type: CommunityType;
  position: Position;
  developmentLevel: number;
  buildings: Building[];
  discoveredTurn: number;
  lastInteractionTurn: number;
  tradeRelationships: TradeRelationship[];
}

interface WorldEvent {
  id: string;
  type: EventType;
  turn: number;
  duration: number;
  effects: EventEffect[];
  description: string;
}

interface EnvironmentalConditions {
  season: Season;
  weather: Weather;
  temperature: number;
  rainfall: number;
  windSpeed: number;
}
```

### Methods
```typescript
class Map {
  // Core map functionality
  generateWorld(seed: number, params: WorldGenerationParams): void;
  getPlot(x: number, y: number): Plot | null;
  getAdjacentPlots(position: Position): Plot[];
  isValidPosition(position: Position): boolean;
  
  // Turn management
  startTurn(): void;
  endTurn(): void;
  advancePhase(): void;
  getCurrentPhase(): GamePhase;
  
  // Community management
  discoverCommunity(position: Position, communityData: CommunityData): void;
  updateCommunities(): void;
  getCommunitiesInRange(position: Position, range: number): Community[];
  
  // World events
  triggerWorldEvent(eventType: EventType): void;
  updateEnvironmentalConditions(): void;
  getActiveEvents(): WorldEvent[];
  
  // Pathfinding and movement
  findPath(start: Position, end: Position): Position[];
  getMovementRange(position: Position, maxDistance: number): Position[];
  isAccessible(position: Position): boolean;
  
  // State management
  saveGameState(): GameState;
  loadGameState(state: GameState): void;
  exportMapData(): MapData;
  importMapData(data: MapData): void;
  
  // Utility
  getWorldStatistics(): WorldStats;
  getResourceDistribution(): ResourceDistribution;
  getDevelopmentProgress(): DevelopmentProgress;
}
```

---

## Strategic Considerations

### Exploration Strategy
- **Systematic Exploration**: Methodical approach to discovering new areas
- **Resource Prioritization**: Focus on areas with valuable resources
- **Community Discovery**: Balance exploration with community interaction
- **Risk Management**: Consider costs and benefits of expansion

### Development Planning
- **Resource Clustering**: Develop related resources together
- **Infrastructure Planning**: Build efficient networks of developments
- **Community Integration**: Plan for interaction with discovered communities
- **Long-term Growth**: Consider future expansion and resource needs

### Turn Optimization
- **Phase Efficiency**: Maximize actions within each phase
- **Resource Flow**: Ensure resources are available when needed
- **Worker Coordination**: Coordinate worker actions across the map
- **Market Timing**: Align market transactions with production cycles

### World Interaction
- **Community Relationships**: Build and maintain trade relationships
- **Resource Competition**: Compete effectively for limited resources
- **Cultural Exchange**: Learn from advanced communities
- **Strategic Alliances**: Form beneficial partnerships

---

## Integration with Other Systems

### Plot System
- Map provides the grid structure for plots
- Map manages plot discovery and settlement
- Map coordinates plot production and development
- Map handles plot adjacency and connectivity

### Worker System
- Map provides movement space for workers
- Map validates worker actions and movement
- Map coordinates worker interactions with plots
- Map manages worker pathfinding and accessibility

### Market System
- Map affects resource availability and distribution
- Map influences market prices through supply/demand
- Map coordinates market transactions with plot production
- Map manages community markets and trade routes

### Game State System
- Map manages overall game state and progression
- Map coordinates turn structure and phase transitions
- Map handles save/load functionality
- Map tracks achievement progress and victory conditions

---

## Balance Considerations

### World Size
- **Accessibility**: World should be explorable within reasonable time
- **Complexity**: Large enough for strategic depth, small enough for mobile
- **Performance**: Balance world size with device capabilities
- **Replayability**: Different world sizes for different game modes

### Resource Distribution
- **Variety**: Ensure all resource types are available
- **Scarcity**: Some resources should be limited for strategic choices
- **Clustering**: Related resources should be grouped logically
- **Accessibility**: Resources should be discoverable through exploration

### Community Placement
- **Strategic Value**: Communities should provide meaningful interaction
- **Development Progression**: Communities should grow at appropriate rates
- **Competition**: Communities should create interesting strategic choices
- **Cooperation**: Communities should offer beneficial partnerships

### Turn Structure
- **Pacing**: Turns should feel meaningful but not overwhelming
- **Complexity**: Each phase should have clear purpose and actions
- **Balance**: All phases should contribute to strategic depth
- **Accessibility**: Turn structure should be understandable for new players

---

## Future Enhancements

### Advanced World Generation
- **Biome System**: Different environmental regions with unique characteristics
- **Dynamic Terrain**: Terrain that changes over time
- **Natural Disasters**: Events that affect the world and communities
- **Seasonal Changes**: World that evolves with seasons

### Community Evolution
- **Advanced AI**: More sophisticated community behavior and development
- **Cultural Systems**: Communities with unique cultures and preferences
- **Political Systems**: Communities with different government types
- **Historical Events**: Communities with rich histories and traditions

### Map Features
- **Transportation Networks**: Roads, rivers, and trade routes
- **Strategic Locations**: Points of interest and special areas
- **Environmental Hazards**: Dangerous areas and natural obstacles
- **Hidden Secrets**: Discoverable locations with special rewards

### Multiplayer Features
- **Shared Worlds**: Multiple players in the same world
- **Competitive Play**: Players competing for resources and territory
- **Cooperative Play**: Players working together to achieve goals
- **Community Building**: Players creating and managing communities together

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Related Documents: worker-design.md, plot-design.md, market-design.md*
