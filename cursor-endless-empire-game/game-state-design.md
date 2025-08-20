# Game State Design Document

## Overview

The GameState system is the central hub of the Endless Empire game, managing the complete game state, turn progression, phase transitions, and persistence. It serves as the single source of truth for all game data and coordinates interactions between all other game systems.

## Core Concept

GameState is a direct state object that contains all game data and provides methods for state transitions, persistence, and validation. It manages the turn-based game flow, coordinates between different systems (Map, Market, Workers, Plots), and handles save/load functionality. The state is observable and can be directly accessed by the UI and other game systems.

---

## State Structure

### Core Game Data
- **Turn Information**: Current turn number and phase
- **Game Systems**: References to Map, Market, and other core systems
- **Collections**: Arrays of Plots, Workers, Communities, etc.
- **Game Configuration**: Settings, difficulty, world parameters
- **Achievement Data**: Progress tracking and unlocked achievements

### State Persistence
- **localStorage Integration**: Automatic save/load to browser storage
- **JSON Serialization**: Complete state serialization for persistence
- **Session Management**: Handle interrupted games and resume functionality
- **State Validation**: Ensure data integrity on load

### State Transitions
- **Turn Progression**: Manage turn phases and state updates
- **Phase Transitions**: Coordinate between Market & Economy, Worker Actions, Plot Production
- **State Validation**: Ensure valid state transitions
- **Event Handling**: Process game events and their effects

---

## Turn Structure

### Turn Phases
**Three-Phase Turn System**:
1. **Market & Economy** - Buy/sell resources, transactions, worker salaries, resource consumption
2. **Worker Actions** - Move, develop plots, work plots (with resources already available)
3. **Plot Production** - Resources generated based on worker actions

### Phase Management
- **Current Phase**: Track which phase the game is in
- **Phase Transitions**: Automatic progression through phases
- **Action Validation**: Ensure actions are performed in correct phase
- **State Persistence**: Save game state between phases

### Turn Flow
- Each turn represents a period of development and exploration
- Market activity happens first (buy resources needed for development)
- Workers perform their actions with resources already available
- Plot production follows from worker actions
- State is saved after each phase completion

---

## Game State Implementation

### Properties
```typescript
interface GameState {
  // Core game information
  gameId: string;
  turn: number;
  phase: GamePhase; // "market_economy" | "worker_actions" | "plot_production"
  gameMode: GameMode; // "quick_play" | "endless" | "story"
  difficulty: Difficulty;
  
  // Game systems
  map: Map;
  market: Market;
  
  // Game collections
  plots: Plot[];
  workers: Worker[];
  communities: Community[];
  achievements: Achievement[];
  events: GameEvent[];
  
  // Player data
  player: Player;
  credits: number;
  resources: ResourceInventory;
  
  // Game configuration
  settings: GameSettings;
  worldParams: WorldGenerationParams;
  
  // State metadata
  lastSaveTime: number;
  gameStartTime: number;
  version: string;
  isDirty: boolean; // Indicates unsaved changes
}

interface Player {
  id: string;
  name: string;
  score: number;
  achievements: Achievement[];
  statistics: PlayerStats;
  preferences: PlayerPreferences;
}

interface GameSettings {
  autoSave: boolean;
  autoSaveInterval: number;
  soundEnabled: boolean;
  musicEnabled: boolean;
  difficulty: Difficulty;
  tutorialEnabled: boolean;
}

interface GameEvent {
  id: string;
  type: EventType;
  turn: number;
  description: string;
  effects: EventEffect[];
  isActive: boolean;
}
```

### Methods
```typescript
class GameState {
  // Core state management
  initialize(settings: GameSettings): void;
  reset(): void;
  validate(): boolean;
  isGameOver(): boolean;
  
  // Turn management
  startTurn(): void;
  endTurn(): void;
  advancePhase(): void;
  getCurrentPhase(): GamePhase;
  canAdvancePhase(): boolean;
  
  // State persistence
  save(): Promise<boolean>;
  load(gameId: string): Promise<boolean>;
  autoSave(): void;
  exportState(): GameStateData;
  importState(data: GameStateData): boolean;
  
  // Game progression
  updateGameState(): void;
  processEvents(): void;
  checkAchievements(): void;
  checkVictoryConditions(): VictoryCondition[];
  
  // System coordination
  updateMarket(): void;
  updateWorkers(): void;
  updatePlots(): void;
  updateCommunities(): void;
  
  // Utility methods
  getGameStatistics(): GameStats;
  getPlayerProgress(): PlayerProgress;
  getResourceSummary(): ResourceSummary;
  getAchievementProgress(): AchievementProgress;
  
  // Event handling
  addEvent(event: GameEvent): void;
  removeEvent(eventId: string): void;
  getActiveEvents(): GameEvent[];
  processEventEffects(): void;
}
```

---

## State Transitions

### Turn Progression
```typescript
// Turn start
startTurn() {
  this.turn++;
  this.phase = "market_economy";
  this.updateMarket();
  this.processEvents();
  this.checkAchievements();
  this.save();
}

// Phase advancement
advancePhase() {
  switch (this.phase) {
    case "market_economy":
      this.phase = "worker_actions";
      this.updateWorkers();
      break;
    case "worker_actions":
      this.phase = "plot_production";
      this.updatePlots();
      break;
    case "plot_production":
      this.endTurn();
      break;
  }
  this.save();
}
```

### State Validation
- **Data Integrity**: Ensure all required properties exist
- **Relationship Validation**: Verify references between objects
- **Business Logic**: Validate game rules and constraints
- **Version Compatibility**: Handle state format changes

### Error Handling
- **Corruption Recovery**: Handle corrupted save data
- **Partial State**: Recover from incomplete saves
- **Version Migration**: Update old save formats
- **Fallback States**: Provide default values for missing data

---

## Persistence System

### localStorage Integration
```typescript
save(): Promise<boolean> {
  try {
    const stateData = this.exportState();
    const serialized = JSON.stringify(stateData);
    localStorage.setItem(`game_${this.gameId}`, serialized);
    this.lastSaveTime = Date.now();
    this.isDirty = false;
    return true;
  } catch (error) {
    console.error('Save failed:', error);
    return false;
  }
}

load(gameId: string): Promise<boolean> {
  try {
    const serialized = localStorage.getItem(`game_${gameId}`);
    if (!serialized) return false;
    
    const stateData = JSON.parse(serialized);
    return this.importState(stateData);
  } catch (error) {
    console.error('Load failed:', error);
    return false;
  }
}
```

### Auto-Save System
- **Interval-Based**: Save at regular intervals
- **Event-Based**: Save after important actions
- **Phase-Based**: Save after each phase completion
- **Error Recovery**: Handle save failures gracefully

### Save Management
- **Multiple Saves**: Support multiple save slots
- **Save Metadata**: Store save information (date, turn, progress)
- **Save Validation**: Verify save data integrity
- **Save Cleanup**: Remove old or corrupted saves

---

## System Integration

### Map System
- GameState contains the Map instance
- Coordinates map updates with turn progression
- Manages map state persistence
- Handles world generation and community discovery

### Market System
- GameState contains the Market instance
- Coordinates market updates with turn phases
- Manages market state persistence
- Handles market events and price updates

### Worker System
- GameState manages the collection of Workers
- Coordinates worker actions with turn phases
- Manages worker hiring and firing
- Tracks worker salaries and costs

### Plot System
- GameState manages the collection of Plots
- Coordinates plot development and production
- Manages plot ownership and discovery
- Tracks plot resource ratings and production

### Achievement System
- GameState tracks achievement progress
- Triggers achievement checks after relevant actions
- Manages achievement rewards and notifications
- Persists achievement data

---

## Strategic Considerations

### State Management
- **Single Source of Truth**: All game data flows through GameState
- **Observable Changes**: State changes are easily trackable
- **Immutable Updates**: Consider immutable state updates for complex changes
- **Performance**: Optimize for frequent state updates

### Turn Optimization
- **Phase Efficiency**: Minimize processing time per phase
- **Batch Updates**: Group related state changes
- **Lazy Loading**: Load data only when needed
- **Caching**: Cache frequently accessed data

### Persistence Strategy
- **Incremental Saves**: Save only changed data
- **Compression**: Compress save data for storage efficiency
- **Backup Strategy**: Maintain backup saves
- **Migration Support**: Handle save format changes

### Error Recovery
- **Graceful Degradation**: Continue game with partial data
- **State Rollback**: Revert to last known good state
- **User Notification**: Inform users of data issues
- **Recovery Options**: Provide recovery mechanisms

---

## Balance Considerations

### Turn Pacing
- **Phase Duration**: Each phase should feel meaningful
- **Action Limits**: Balance action availability with strategic depth
- **State Complexity**: Manage state complexity for performance
- **User Experience**: Ensure smooth turn progression

### Save Frequency
- **Auto-Save Interval**: Balance safety with performance
- **Manual Saves**: Allow user control over save timing
- **Save Size**: Optimize save data size
- **Save Time**: Minimize save operation duration

### State Validation
- **Validation Frequency**: Balance safety with performance
- **Validation Scope**: Validate critical vs. non-critical data
- **Error Tolerance**: Handle minor data inconsistencies
- **Recovery Speed**: Minimize recovery time

### Memory Management
- **State Size**: Monitor total state size
- **Garbage Collection**: Clean up unused data
- **Memory Leaks**: Prevent memory leaks in long sessions
- **Performance Monitoring**: Track memory usage

---

## Future Enhancements

### Advanced State Management
- **State History**: Track state changes for undo/redo
- **State Branching**: Support multiple game branches
- **State Synchronization**: Support cloud saves
- **State Analytics**: Track state patterns for optimization

### Enhanced Persistence
- **Cloud Storage**: Support cloud save synchronization
- **Cross-Platform**: Share saves between devices
- **Backup Services**: Automatic cloud backup
- **Version Control**: Track save file versions

### Performance Optimizations
- **State Compression**: Compress state data
- **Lazy Loading**: Load state data on demand
- **State Caching**: Cache frequently accessed data
- **Background Processing**: Process state updates in background

### Advanced Features
- **Replay System**: Record and replay game sessions
- **State Debugging**: Advanced state inspection tools
- **State Migration**: Automatic state format updates
- **State Analytics**: Analyze state patterns and trends

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Related Documents: worker-design.md, plot-design.md, market-design.md, map-design.md*
