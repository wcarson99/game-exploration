# Market System Design Document

## 1. Overview

The Market system is a core component of the Endless Empire game, providing a centralized economic hub where players can buy and sell resources. The market features dynamic pricing based on supply and demand, creating strategic economic decisions and resource management challenges.

## 2. Core Concept

The market serves as a single central trading hub where all resource transactions occur. It maintains dynamic pricing that adjusts based on supply and demand, creating economic opportunities and challenges. The market never runs out of resources or money, ensuring players can always trade, but prices fluctuate to reflect market conditions.

---

## 3. Market Interface

### 3.1. Black Box Design
The market is implemented as a "black box" - game code only interacts with the market through defined interface methods, while the internal pricing and supply/demand logic remains hidden.

### 3.2. Core Market Methods

#### `market.getResource(resourceType)`
**Purpose**: Get current market state for a resource
**Parameters**: `resourceType` - the resource to check
**Returns**: `{quantity: number, buyPrice: number, sellPrice: number}`
**Example**: `market.getResource("food")` returns current food availability and prices

### 3.3. Plot Trading Methods

#### `plot.buy(resourceType, quantity)`
**Purpose**: Plot buys resources FROM the market
**Action**: Plot gains resources, plot.owner loses credits
**Direction**: Market → Plot
**Returns**: Success/failure, actual cost
**Example**: `farmPlot.buy("coal", 5)` - farm gets 5 coal, owner pays credits

#### `plot.sell(resourceType, quantity)`
**Purpose**: Plot sells resources TO the market
**Action**: Plot loses resources, plot.owner gains credits
**Direction**: Plot → Market
**Returns**: Success/failure, actual payment
**Example**: `farmPlot.sell("food", 10)` - farm loses 10 food, owner gets credits

---

## 4. Market Behavior

### 4.1. Price Dynamics
**Supply and Demand Effects:**
- **High Supply**: Market has lots of a resource → Lower buy/sell prices
- **Low Supply**: Market has little of a resource → Higher buy/sell prices
- **High Demand**: Players buying lots of a resource → Higher prices
- **Low Demand**: Players selling lots of a resource → Lower prices

### 4.2. Spread Pricing
- **Market always maintains a spread** between buy and sell prices
- **Buy price < Sell price** for all resources
- **Spread size** may vary based on supply/demand conditions

### 4.3. Example Scenarios

**Food Surplus (Market has 100 food):**
- Market buys food at 1 credit, sells food at 2 credits
- Players can sell excess food cheaply, buy food cheaply

**Food Shortage (Market has 5 food):**
- Market buys food at 4 credits, sells food at 6 credits
- Players pay premium for food, get good prices for selling

---

## 5. Transaction Mechanics

### 5.1. Atomic Transactions
- **All transactions are atomic** - either complete fully or fail completely
- **No partial trades** - you can't get credits without losing resources
- **Credits and resources change together** in a single operation

### 5.2. Plot Ownership
- **Credits flow to/from plot.owner** (the player)
- **Resources are stored in the plot** (or plot inventory)
- **Clear ownership chain** for all transactions

### 5.3. Transaction Flow

#### Buying Process:
1. Check if plot.owner has enough credits
2. Check if market has enough resources
3. Transfer credits from plot.owner to market
4. Transfer resources from market to plot
5. Update market supply/demand

#### Selling Process:
1. Check if plot has enough resources
2. Transfer resources from plot to market
3. Transfer credits from market to plot.owner
4. Update market supply/demand

---

## 6. Resource System

### 6.1. Basic Resources
- **Food**: Agricultural products from farms
- **Stone**: Building materials from mines
- **Wood**: Building materials from forests
- **Coal**: Energy source from mines
- **Ore**: Raw metal source from mines

### 6.2. Processed Resources
- **Energy**: Generated from coal, wind, or sunlight
- **Metal**: Created from ore using energy
- **Building Materials**: Processed from stone or wood

### 6.3. Resource Flow
- **Plots consume resources** for development and production
- **Plots produce resources** for sale or use
- **Market facilitates transfer** between plots and players

### 6.4. Crop System
**Crop Types & Properties:**
```javascript
CROPS = {
  wheat: {
    waterPerTurn: 3,
    sunlightPerTurn: 2,
    growthTime: 1,       // turns to mature
    soilRequirement: 2   // minimum soil rating needed
  },
  corn: {
    waterPerTurn: 4,
    sunlightPerTurn: 3,
    growthTime: 2,
    soilRequirement: 3
  },
  rice: {
    waterPerTurn: 6,
    sunlightPerTurn: 2,
    growthTime: 3,
    soilRequirement: 5
  }
}
```

**Farm Development:**
- **Hire Farmer** (1 credit per turn)
- **Buy seeds** from market for chosen crop
- **Place farmer on plot** with appropriate resources
- **Plot becomes developed** as farm
- **Production starts next turn**

**Production Formula:**
- **Final Production** = `deratedProduction(plot, cropType)`
- **Function considers**: `plot.soil_rating`, `plot.water`, `plot.sunlight`
- **Resource consumption**: Water and sunlight consumed per turn based on crop type
- **Market values**: All crop values determined by market supply/demand

### 6.5. Mine System
**Mine Development:**
- **Hire Miner** (1 credit per turn)
- **Buy energy, minerals, and iron** from market (initial investment)
- **Place miner on plot** with appropriate resources
- **Plot becomes developed** as mine
- **Production starts next turn**

**Mine Operations:**
- **Ongoing costs**: Energy and minerals consumed per turn
- **Production**: Based on plot resource ratings (coal, stone, ore)
- **Resource consumption**: Energy and minerals needed to keep mine running

### 6.6. Explorer System
**Explorer Development:**
- **Hire Explorer** (1 credit per turn)
- **No initial resource costs** - just salary
- **Place explorer on any plot** to start exploring

**Explorer Actions:**
- **Move**: Go to unsettled plot
- **Settle**: Convert plot from unsettled to settled, reveal plot attributes

**Explorer Mechanics:**
- **No movement costs** - just the 1 credit per turn salary
- **Settlement reveals** plot resource ratings and makes plot available for development
- **Exploration enables** expansion by discovering new areas
- **Player then hires** appropriate worker type for settled plots

---

## 7. Market Class Implementation

### 7.1. Properties
```typescript
interface Market {
  // Market state
  resources: Map<ResourceType, MarketResource>;
  priceHistory: Map<ResourceType, PricePoint[]>;
  transactionHistory: Transaction[];
  
  // Market configuration
  basePrices: Map<ResourceType, number>;
  spreadMultiplier: number;
  supplyDemandSensitivity: number;
  
  // Market statistics
  totalTransactions: number;
  totalVolume: number;
  lastUpdateTurn: number;
}

interface MarketResource {
  quantity: number;
  buyPrice: number;
  sellPrice: number;
  lastTransactionPrice: number;
  supplyTrend: 'increasing' | 'decreasing' | 'stable';
  demandTrend: 'increasing' | 'decreasing' | 'stable';
}

interface PricePoint {
  turn: number;
  buyPrice: number;
  sellPrice: number;
  quantity: number;
}

interface Transaction {
  id: string;
  turn: number;
  resourceType: ResourceType;
  quantity: number;
  price: number;
  type: 'buy' | 'sell';
  plotId: string;
  playerId: string;
}
```

### 7.2. Methods
```typescript
class Market {
  // Core market functionality
  getResource(resourceType: ResourceType): MarketResource;
  updatePrices(): void;
  processTransaction(transaction: Transaction): boolean;
  
  // Plot trading interface
  buyResource(plot: Plot, resourceType: ResourceType, quantity: number): TransactionResult;
  sellResource(plot: Plot, resourceType: ResourceType, quantity: number): TransactionResult;
  
  // Price calculations
  calculateBuyPrice(resourceType: ResourceType): number;
  calculateSellPrice(resourceType: ResourceType): number;
  updateSupplyDemand(resourceType: ResourceType, quantity: number, isBuy: boolean): void;
  
  // Market analysis
  getPriceHistory(resourceType: ResourceType, turns: number): PricePoint[];
  getMarketTrends(): Map<ResourceType, MarketTrend>;
  getMostExpensiveResource(): ResourceType;
  getCheapestResource(): ResourceType;
  
  // Utility
  resetMarket(): void;
  saveMarketState(): MarketState;
  loadMarketState(state: MarketState): void;
}
```

---

## 8. Strategic Considerations

### 8.1. Market Timing
- **Sell when market has surplus** (get better prices)
- **Buy when market has surplus** (pay lower prices)
- **Hold resources when market is short** (prices will rise)
- **Monitor price trends** for optimal trading opportunities

### 8.2. Resource Management
- **Balance production and consumption** to maintain steady income
- **Diversify resource portfolio** to reduce market risk
- **Plan for seasonal variations** in resource availability
- **Consider long-term vs short-term** resource needs

### 8.3. Economic Strategy
- **Balance expansion costs** with income generation
- **Invest in high-value resources** when prices are low
- **Sell excess production** to fund new developments
- **Maintain cash reserves** for unexpected opportunities

### 8.4. Risk Management
- **Don't over-invest** in single resource types
- **Monitor market trends** to anticipate price changes
- **Maintain production diversity** to hedge against market fluctuations
- **Plan for resource shortages** and price spikes

---

## 9. Integration with Other Systems

### 9.1. Plot System
- Plots consume resources purchased from market
- Plots produce resources sold to market
- Plot development requires market resources
- Plot production efficiency affects market supply

### 9.2. Worker System
- Workers cost credits (salary from market)
- Worker hiring requires market funds
- Worker production contributes to market supply
- Worker specialization affects market demand

### 9.3. Economy System
- Market is the primary source of credits
- Market prices affect economic balance
- Market transactions drive economic growth
- Market conditions influence strategic decisions

### 9.4. Game State System
- Market state persists between turns
- Market prices update at turn start
- Market transactions recorded in game history
- Market affects achievement progress

---

## 10. Balance Considerations

### 10.1. Price Stability
- **Base prices** should provide reasonable starting points
- **Price volatility** should create interesting decisions
- **Supply/demand sensitivity** should be balanced
- **Spread pricing** should provide market sustainability

### 10.2. Economic Flow
- **Credit generation** should match credit consumption
- **Resource availability** should support player strategies
- **Market liquidity** should prevent deadlocks
- **Transaction costs** should encourage strategic trading

### 10.3. Strategic Depth
- **Multiple viable strategies** should be supported
- **Market timing** should provide meaningful advantages
- **Resource specialization** should have clear benefits
- **Risk vs reward** should be balanced across options

### 10.4. Accessibility
- **New players** should understand basic market mechanics
- **Market interface** should be clear and intuitive
- **Price information** should be readily available
- **Trading decisions** should have clear consequences

---

## 11. Future Enhancements

### 11.1. Advanced Trading
- **Futures contracts** for resource delivery
- **Bulk trading** discounts for large quantities
- **Price prediction** tools for strategic planning
- **Market orders** for automated trading

### 11.2. Community Markets
- **Separate markets** for discovered communities
- **Different pricing** based on community size and type
- **Trade routes** between communities
- **Cultural exchange** affecting market preferences

### 11.3. Market Events
- **Price booms and crashes** for economic variety
- **Resource shortages** creating price spikes
- **Seasonal variations** affecting resource values
- **Economic cycles** creating long-term trends

### 11.4. Market Analytics
- **Price history charts** for trend analysis
- **Market statistics** for strategic planning
- **Trading volume** indicators
- **Supply/demand** visualizations

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Related Documents: worker-design.md, plot-design.md*
