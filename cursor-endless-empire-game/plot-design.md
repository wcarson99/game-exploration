# Plot System Design Document

## Overview

The Plot system is a fundamental component of the Endless Empire game, representing individual grid locations that can be discovered, developed, and worked to produce resources. Each plot has unique resource ratings and can be transformed into different development types based on available resources and worker actions.

## Core Concept

Plots are the building blocks of the game world, each with inherent resource potential that determines what can be built and how efficiently it can produce resources. Plots start as unsettled, can be converted to settled through exploration, and then developed into specific types of infrastructure that generate different resources.

---

## Plot States

### 1. Unsettled
**Description**: Undiscovered plots that are not yet available for development
**Visibility**: Hidden from player until discovered
**Actions Available**: None (must be settled first)
**Resource Ratings**: Unknown until settled

### 2. Settled
**Description**: Discovered plots that are available for development
**Visibility**: Visible to player with resource ratings revealed
**Actions Available**: Can hire workers, can be developed
**Resource Ratings**: Fully visible (0-10 for each resource type)

### 3. Developed
**Description**: Plots that have been transformed into specific infrastructure
**Types**: Farm, Mine, Power Plant, Town, Market
**Actions Available**: Can be worked by appropriate workers
**Production**: Generates resources based on plot type and resource ratings

---

## Resource System

### Collected Resources (Renewable)
These resources are naturally available and can be harvested sustainably:

#### Wind (💨)
- **Rating Range**: 0-10
- **Factors**: Elevation, exposure, geographic location
- **Uses**: Windmills, energy production
- **Renewability**: Always available, varies by weather

#### Sunlight (☀️)
- **Rating Range**: 0-10
- **Factors**: Latitude, weather patterns, seasonal changes
- **Uses**: Solar power, agriculture
- **Renewability**: Daily cycle, seasonal variations

#### Water (💧)
- **Rating Range**: 0-10
- **Factors**: Proximity to rivers/lakes, rainfall patterns
- **Uses**: Agriculture, water mills, population needs
- **Renewability**: Natural flow, seasonal variations

#### Soil (🌱)
- **Rating Range**: 0-10
- **Factors**: Fertility, drainage, organic content
- **Uses**: Agriculture, farming
- **Renewability**: Can be improved over time

### Extracted Resources (Finite)
These resources are limited and deplete over time:

#### Coal (⛏️)
- **Rating Range**: 0-10
- **Factors**: Geological deposits, depth
- **Uses**: Energy production, industrial processes
- **Renewability**: Finite, depletes with use

#### Stone (🪨)
- **Rating Range**: 0-10
- **Factors**: Rock formations, geological history
- **Uses**: Building materials, construction
- **Renewability**: Finite, depletes with use

#### Ore (⚒️)
- **Rating Range**: 0-10
- **Factors**: Mineral deposits, geological conditions
- **Uses**: Metal production, tools, machinery
- **Renewability**: Finite, depletes with use

---

## Plot Development

### Development Types

#### Farm
**Worker Required**: Farmer
**Resource Requirements**: Soil + Water (minimum ratings to be determined)
**Production**: Food and agricultural resources
**Efficiency**: Based on soil and water ratings

#### Mine
**Worker Required**: Miner
**Resource Requirements**: Stone/Coal/Ore deposits (minimum ratings to be determined)
**Production**: Extracted resources (coal, stone, ore)
**Efficiency**: Based on deposit ratings

#### Windmill
**Worker Required**: Blacksmith
**Resource Requirements**: Wind (minimum rating to be determined)
**Production**: Energy
**Efficiency**: Based on wind rating

#### Water Mill
**Worker Required**: Blacksmith
**Resource Requirements**: Water (minimum rating to be determined)
**Production**: Energy and mechanical power
**Efficiency**: Based on water rating

#### Boiler
**Worker Required**: Blacksmith
**Resource Requirements**: Coal (minimum rating to be determined)
**Production**: Energy and heat
**Efficiency**: Based on coal rating

#### Market
**Worker Required**: Trader
**Resource Requirements**: Population and trade routes
**Production**: Trade goods and income
**Efficiency**: Based on population and connectivity

#### Town
**Special Type**: Starting base and population center
**Requirements**: Cannot be developed (starting condition)
**Production**: Population growth and basic resources
**Efficiency**: Base level, can be improved with surrounding developments

### Development Mechanics

#### Resource Requirements
- Each development type requires specific resource ratings
- Minimum ratings determine if development is possible
- Higher ratings improve production efficiency
- Some developments require multiple resource types

#### Worker Specialization
- Only the appropriate worker type can develop each plot type
- Worker must be present at the plot to perform development
- Development consumes the worker's action for the turn
- Development may require additional resources beyond plot ratings

#### Development Cost
- Credits required to establish the development
- Materials may be required (varies by development type)
- Cost scales with development complexity
- Some developments have ongoing maintenance costs

#### Production Output
- Higher resource ratings result in better production efficiency
- Production occurs when appropriate worker works the plot
- Output varies by plot type and resource ratings
- Some plots may have maximum production caps

---

## Plot Class Implementation

### Properties
```typescript
interface Plot {
  id: string;
  position: Position; // Grid coordinates (x, y)
  owner: 'player' | 'computer' | 'unsettled';
  state: PlotState; // Unsettled, Settled, Farm, Mine, etc.
  isSettled: boolean;
  
  // Resource ratings (0-10 for each type)
  resourceRatings: {
    // Collected resources
    wind: number;
    sunlight: number;
    water: number;
    soil: number;
    
    // Extracted resources
    coal: number;
    stone: number;
    ore: number;
  };
  
  // Development state
  developmentType?: DevelopmentType;
  assignedWorker?: Worker;
  developmentLevel?: number; // For upgrades
  
  // Production state
  isProducing: boolean;
  lastProductionTurn?: number;
  productionEfficiency?: number;
}
```

### Methods
```typescript
class Plot {
  // Core functionality
  getResourceRating(resourceType: ResourceType): number;
  canDevelop(developmentType: DevelopmentType, workerType: WorkerType): boolean;
  develop(developmentType: DevelopmentType, resources: ResourceCost): boolean;
  produceResources(worker: Worker): ResourceProduction;
  
  // State management
  settle(): void;
  assignWorker(worker: Worker): boolean;
  removeWorker(): void;
  getDevelopmentCost(developmentType: DevelopmentType): ResourceCost;
  
  // Production calculations
  calculateProductionEfficiency(): number;
  getProductionOutput(worker: Worker): ResourceProduction;
  canProduce(): boolean;
  
  // Utility
  getAdjacentPlots(): Plot[];
  getDistanceTo(otherPlot: Plot): number;
  isAdjacentTo(otherPlot: Plot): boolean;
}
```

---

## Strategic Considerations

### Plot Selection
- **Resource Potential**: Choose plots with high ratings for desired resources
- **Location**: Consider proximity to existing developments and workers
- **Development Path**: Plan which plot types to prioritize
- **Efficiency**: Balance resource ratings with development costs

### Development Planning
- **Worker Allocation**: Ensure appropriate workers are available for desired developments
- **Resource Flow**: Plan how developments will support each other
- **Economic Efficiency**: Consider development costs vs. production benefits
- **Long-term Value**: Some developments provide ongoing benefits

### Production Optimization
- **Worker Assignment**: Ensure developed plots have appropriate workers
- **Resource Synergies**: Combine complementary plot types
- **Efficiency Improvements**: Focus on high-rating plots for key resources
- **Maintenance**: Balance production with resource depletion

---

## Integration with Other Systems

### Worker System
- Plots provide locations for worker actions
- Plot development requires specific worker types
- Plot production requires worker presence
- Worker movement affects plot accessibility

### Resource System
- Plot resource ratings determine production potential
- Plot developments consume and produce resources
- Resource availability affects development options
- Resource depletion impacts plot value over time

### Map System
- Plots are organized in a grid structure
- Plot positions affect adjacency and connectivity
- Map generation determines initial resource distribution
- Plot discovery expands the playable area

### Economy System
- Plot developments have costs and benefits
- Plot production contributes to economic growth
- Plot ownership affects resource control
- Plot value changes based on development and resources

---

## Balance Considerations

### Resource Distribution
- **Variety**: Ensure all resource types are available
- **Scarcity**: Some resources should be limited to create strategic choices
- **Clustering**: Related resources should be grouped for realistic geography
- **Accessibility**: Resources should be discoverable through exploration

### Development Costs
- **Progression**: Early developments should be affordable
- **Scaling**: Advanced developments should require significant investment
- **Risk vs Reward**: Higher-cost developments should provide greater benefits
- **Maintenance**: Ongoing costs should balance production benefits

### Production Balance
- **Efficiency**: Higher resource ratings should provide meaningful benefits
- **Depletion**: Finite resources should deplete at appropriate rates
- **Renewability**: Renewable resources should provide consistent value
- **Synergies**: Complementary developments should provide bonuses

---

## Future Enhancements

### Advanced Plot Types
- **Research Centers**: Generate technology and knowledge
- **Cultural Sites**: Provide cultural benefits and tourism
- **Defensive Structures**: Protect against environmental threats
- **Transportation Hubs**: Improve connectivity and trade

### Plot Upgrades
- **Development Levels**: Plots can be upgraded to higher tiers
- **Specialization**: Plots can focus on specific aspects of production
- **Efficiency Improvements**: Technologies can improve production rates
- **Environmental Impact**: Developments can affect surrounding plots

### Dynamic Plot Changes
- **Environmental Events**: Natural disasters and weather can affect plots
- **Resource Depletion**: Finite resources can be exhausted
- **Regeneration**: Some resources can regenerate over time
- **Pollution**: Industrial developments can affect plot quality

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Related Documents: worker-design.md, map-design.md*
