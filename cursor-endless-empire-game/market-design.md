# Cute Civilization Builder - Market System Design

## 1. Market Overview

### 1.1 Core Concept
The market system allows plots to buy and sell resources with a single central market. The market maintains dynamic pricing based on supply and demand, creating economic opportunities and resource management challenges.

### 1.2 Market Principles
- **Single Market**: One central market for all resource trading
- **Dynamic Pricing**: Prices adjust based on supply and demand
- **Always Available**: Market never runs out of resources or money
- **Spread Pricing**: Market buys low, sells high (like real markets)

---

## 2. Market Interface (Black Box)

### 2.1 Market Object Methods

**`market.getResource(resourceType)`**
- **Returns**: `{quantity: number, buyPrice: number, sellPrice: number}`
- **Purpose**: Get current market state for a resource
- **Parameters**: `resourceType` - the resource to check
- **Example**: `market.getResource("food")` returns current food availability and prices

### 2.2 Plot Object Methods

**`plot.buy(resourceType, quantity)`**
- **Purpose**: Plot buys resources FROM the market
- **Action**: Plot gains resources, plot.owner loses credits
- **Direction**: Market → Plot
- **Returns**: Success/failure, actual cost
- **Example**: `farmPlot.buy("coal", 5)` - farm gets 5 coal, owner pays credits

**`plot.sell(resourceType, quantity)`**
- **Purpose**: Plot sells resources TO the market
- **Action**: Plot loses resources, plot.owner gains credits
- **Direction**: Plot → Market
- **Returns**: Success/failure, actual payment
- **Example**: `farmPlot.sell("food", 10)` - farm loses 10 food, owner gets credits

---

## 3. Market Behavior

### 3.1 Price Dynamics
**Supply and Demand Effects:**
- **High Supply**: Market has lots of a resource → Lower buy/sell prices
- **Low Supply**: Market has little of a resource → Higher buy/sell prices
- **High Demand**: Players buying lots of a resource → Higher prices
- **Low Demand**: Players selling lots of a resource → Lower prices

### 3.2 Example Scenarios

**Food Surplus (Market has 100 food):**
- Market buys food at 1 credit, sells food at 2 credits
- Players can sell excess food cheaply, buy food cheaply

**Food Shortage (Market has 5 food):**
- Market buys food at 4 credits, sells food at 6 credits
- Players pay premium for food, get good prices for selling

### 3.3 Spread Pricing
- **Market always maintains a spread** between buy and sell prices
- **Buy price < Sell price** for all resources
- **Spread size** may vary based on supply/demand conditions

---

## 4. Transaction Mechanics

### 4.1 Atomic Transactions
- **All transactions are atomic** - either complete fully or fail completely
- **No partial trades** - you can't get credits without losing resources
- **Credits and resources change together** in a single operation

### 4.2 Plot Ownership
- **Credits flow to/from plot.owner** (the player)
- **Resources are stored in the plot** (or plot inventory)
- **Clear ownership chain** for all transactions

### 4.3 Transaction Flow
**Buying:**
1. Check if plot.owner has enough credits
2. Check if market has enough resources
3. Transfer credits from plot.owner to market
4. Transfer resources from market to plot
5. Update market supply/demand

**Selling:**
1. Check if plot has enough resources
2. Transfer resources from plot to market
3. Transfer credits from market to plot.owner
4. Update market supply/demand

---

## 5. Resource Types

### 5.1 Basic Resources
- **Food**: Agricultural products from farms
- **Stone**: Building materials from mines
- **Wood**: Building materials from forests
- **Coal**: Energy source from mines
- **Ore**: Raw metal source from mines

### 5.2 Processed Resources
- **Energy**: Generated from coal, wind, or sunlight
- **Metal**: Created from ore using energy
- **Building Materials**: Processed from stone or wood

### 5.3 Resource Flow
- **Plots consume resources** for development and production
- **Plots produce resources** for sale or use
- **Market facilitates transfer** between plots and players

### 5.4 Crop System
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

### 5.5 Mine System
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

### 5.6 Explorer System
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

## 6. Economic Balance

### 6.1 Starting Economy
- **Player starts with credits** for hiring workers and buying resources
- **Worker salaries**: 1 credit per turn per worker
- **Resource purchases**: Initial resources to start production

### 6.2 Economic Pressure
**Costs:**
- Worker salaries (ongoing)
- Resource purchases (as needed)
- Development costs (one-time)

**Income Sources:**
- Resource sales to market
- Building production
- Achievement rewards

### 6.3 Strategic Considerations
- **Sell when market has surplus** (get better prices)
- **Buy when market has surplus** (pay lower prices)
- **Hold resources when market is short** (prices will rise)
- **Balance expansion costs** with income generation

---

## 7. Game Integration

### 7.1 Turn Structure Integration
- **Market prices update** at the start of each turn
- **Transactions can occur** during any turn phase
- **Supply/demand changes** based on player actions

### 7.2 Plot Development
- **Plots need resources** to develop (soil, water, coal, etc.)
- **Plots produce resources** for sale or use
- **Market provides access** to resources plots can't produce

### 7.3 Worker Management
- **Workers cost credits** (1 credit per turn per worker)
- **Credits come from** resource sales and other income
- **Market enables** credit generation through resource trading

---

## 8. Future Enhancements

### 8.1 Community Markets
- **Separate markets** for discovered communities
- **Different pricing** based on community size and type
- **Trade routes** between communities

### 8.2 Advanced Trading
- **Futures contracts** for resource delivery
- **Bulk trading** discounts for large quantities
- **Price prediction** tools for strategic planning

### 8.3 Market Events
- **Price booms and crashes** for economic variety
- **Resource shortages** creating price spikes
- **Seasonal variations** affecting resource values

---

## 9. Implementation Notes

### 9.1 Black Box Design
- **Market implementation details** are hidden from game code
- **Game only calls** the defined interface methods
- **Market handles** all pricing, supply/demand, and transaction logic

### 9.2 Data Persistence
- **Market state** stored in game state
- **Transaction history** can be tracked for achievements
- **Price history** can be used for trend analysis

### 9.3 Performance Considerations
- **Price calculations** should be efficient
- **Transaction processing** should be fast
- **Market updates** should not impact game performance

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Next Review: [Date + 2 weeks]*
