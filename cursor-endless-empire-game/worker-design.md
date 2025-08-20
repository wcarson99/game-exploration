# Worker System Design Document

## 1. Overview

The Worker system is a core component of the Endless Empire game, representing the specialized units that players manage to explore, develop, and produce resources. Workers are the primary means of interacting with the world and driving economic growth.

## 2. Core Concept

Workers are specialized units that can be hired at settled plots to perform specific actions. Each worker type has unique abilities and can only perform certain development and production tasks. Workers require salary payments and can only perform one action per turn, creating strategic decisions about resource allocation.

---

## 3. Worker Types

### 3.1. Explorer
**Role**: Discovery and settlement
**Specialization**: Converting unsettled plots to settled plots

**Actions:**
- **Move**: Travel to unsettled plots
- **Settle**: Convert plot from unsettled to settled, revealing plot attributes

**Development Abilities**: None (cannot develop plots)
**Production Abilities**: None (cannot work plots)

### 3.2. Farmer
**Role**: Agricultural development and food production
**Specialization**: Farming and agricultural resources

**Actions:**
- **Move**: Travel to settled plots
- **Develop Plot**: Create Farms (requires soil + water)
- **Work Plot**: Generate agricultural resources from farms

**Development Abilities**: Can develop plots into Farms
**Production Abilities**: Produces food and agricultural resources

### 3.3. Miner
**Role**: Resource extraction and mining operations
**Specialization**: Extracting finite resources from the earth

**Actions:**
- **Move**: Travel to settled plots
- **Develop Plot**: Create Mines (requires stone/coal/ore deposits)
- **Work Plot**: Extract finite resources from mines

**Development Abilities**: Can develop plots into Mines
**Production Abilities**: Produces coal, stone, and ore

### 3.4. Blacksmith
**Role**: Manufacturing and energy production
**Specialization**: Creating tools, machinery, and energy infrastructure

**Actions:**
- **Move**: Travel to settled plots
- **Develop Plot**: Create Windmills (wind), Water Mills (water), or Boilers (coal)
- **Work Plot**: Generate energy and manufactured goods

**Development Abilities**: Can develop plots into Windmills, Water Mills, or Boilers
**Production Abilities**: Produces energy and manufactured goods

### 3.5. Trader
**Role**: Commerce and market management
**Specialization**: Trade relationships and market operations

**Actions:**
- **Move**: Travel to settled plots
- **Develop Plot**: Create Markets (requires population and trade routes)
- **Work Plot**: Generate trade goods and income

**Development Abilities**: Can develop plots into Markets
**Production Abilities**: Produces trade goods and generates income

---

## 4. Worker Mechanics

### 4.1. Basic Properties
- **Salary**: 1 credit per turn per worker
- **Movement**: One plot per turn (adjacent plots only)
- **Action Limit**: One action per turn per worker
- **Hiring**: Any settled plot can hire workers (player chooses worker type)
- **Specialization**: Each worker type has specific development abilities

### 4.2. Movement Rules
- Workers can move to adjacent plots only
- Movement costs one action for the turn
- Workers cannot move through unsettled plots (except Explorers)
- Movement is required before performing other actions

### 4.3. Action System
**Action Types:**
1. **Move**: Change location to adjacent plot
2. **Settle**: Convert unsettled plot to settled (Explorer only)
3. **Develop Plot**: Transform settled plot into specific type
4. **Work Plot**: Generate production from developed plot

**Action Rules:**
- Each worker can perform only one action per turn
- Actions are performed during the "Worker Actions" phase
- Some actions require specific resources to be available
- Actions have immediate effects on the game state

---

## 5. Worker Class Implementation

### 5.1. Properties
```typescript
interface Worker {
  id: string;
  type: WorkerType; // Explorer, Farmer, Miner, Blacksmith, Trader
  location: Position; // Current plot coordinates
  salary: number; // Credits per turn (default: 1)
  hasActed: boolean; // Whether worker has performed action this turn
  owner: 'player' | 'computer';
}
```

### 5.2. Methods
```typescript
class Worker {
  // Core actions
  move(targetPlot: Plot): boolean;
  settle(): boolean; // Explorer only
  developPlot(plot: Plot, developmentType: DevelopmentType): boolean;
  workPlot(plot: Plot): ResourceProduction;
  
  // State management
  canPerformAction(actionType: ActionType): boolean;
  canMoveTo(targetPlot: Plot): boolean;
  canDevelopPlot(plot: Plot, developmentType: DevelopmentType): boolean;
  canWorkPlot(plot: Plot): boolean;
  
  // Turn management
  startTurn(): void;
  endTurn(): void;
  resetActionState(): void;
  
  // Utility
  getSalary(): number;
  getMovementRange(): Position[];
  getDevelopmentAbilities(): DevelopmentType[];
}
```

---

## 6. Strategic Considerations

### 6.1. Worker Allocation
- **Exploration vs Development**: Balance between discovering new plots and developing existing ones
- **Specialization**: Different worker types excel at different tasks
- **Resource Requirements**: Some developments require specific resource ratings
- **Economic Efficiency**: Workers cost money but generate value through production

### 6.2. Turn Planning
- **Action Priority**: Decide which actions are most important each turn
- **Resource Flow**: Ensure workers have access to required resources
- **Movement Planning**: Position workers strategically for future actions
- **Salary Management**: Balance worker costs against production benefits

### 6.3. Long-term Strategy
- **Worker Mix**: Maintain appropriate ratios of different worker types
- **Development Paths**: Choose which plot types to prioritize
- **Expansion Planning**: Plan for future growth and resource needs
- **Efficiency Optimization**: Maximize production per worker

---

## 7. Integration with Other Systems

### 7.1. Plot System
- Workers interact with plots to perform actions
- Plot resource ratings affect worker production efficiency
- Plot development state determines available worker actions

### 7.2. Resource System
- Workers consume resources for development actions
- Workers produce resources through work actions
- Resource availability affects worker capabilities

### 7.3. Economy System
- Workers require salary payments
- Worker production contributes to economic growth
- Worker hiring decisions affect economic balance

### 7.4. Map System
- Workers move across the map grid
- Worker actions reveal and develop the world
- Map events can affect worker capabilities

---

## 8. Balance Considerations

### 8.1. Worker Costs
- **Salary**: 1 credit per turn provides consistent economic pressure
- **Hiring Cost**: Should be balanced against worker value
- **Maintenance**: Ongoing costs encourage efficient worker use

### 8.2. Worker Value
- **Production Output**: Must exceed salary costs over time
- **Development Speed**: Balance between immediate and long-term value
- **Specialization**: Each worker type should have clear advantages

### 8.3. Strategic Depth
- **Multiple Viable Paths**: Different worker combinations should be effective
- **Situational Value**: Worker effectiveness should vary by circumstances
- **Risk vs Reward**: Higher-value workers should have higher costs or requirements

---

## 9. Future Enhancements

### 9.1. Advanced Worker Types
- **Engineer**: Advanced building development
- **Scholar**: Research and technology advancement
- **Diplomat**: Enhanced community interaction

### 9.2. Worker Progression
- **Experience System**: Workers improve with use
- **Specialization Paths**: Workers can develop unique abilities
- **Promotion System**: Workers can advance to more powerful types

### 9.3. Worker Interactions
- **Teamwork**: Multiple workers can collaborate on complex tasks
- **Mentorship**: Experienced workers can train new ones
- **Specialization**: Workers can focus on specific aspects of their role

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Related Documents: plot-design.md, map-design.md*
