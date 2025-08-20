# UI/UX Design Document

## 1. Overview

The UI/UX design for Endless Empire focuses on a mobile-first, MVP approach with three core scenes and minimal complexity. The interface prioritizes clarity, touch-friendliness, and intuitive gameplay flow while maintaining the cute aesthetic and strategic depth of the game.

## 2. Core Concept

The UI is designed around a simple three-scene structure that guides players through the complete game loop. Each scene has a single, clear purpose with minimal navigation complexity. The design emphasizes touch-friendly interactions, clear visual feedback, and progressive disclosure of information.

---

## 3. MVP Scene Structure

### 3.1. Welcome Scene (Home)
**Purpose**: Entry point, shows achievements and best runs, starts/resumes game

**Components**:
- **Game Title**: "Endless Empire" with cute branding
- **New Game Button**: Large, prominent button to start fresh game
- **Resume Game Button**: Appears only if save exists, continues interrupted game
- **Previous Best Run Display**:
  - Longest run (turns survived)
  - Most profitable game (total credits earned)
  - Best achievement unlocked
- **Achievement Summary**: Grid of unlocked achievements
- **Help Button**: "?" icon for accessing help menu
- **Settings Button**: Simple settings (sound on/off)

**Layout**:
- **Header**: Game title and branding
- **Center**: Main action buttons (New Game, Resume)
- **Bottom**: Achievement display and settings
- **Corner**: Help button (always accessible)

### 3.2. Turn Phase Scenes (3 scenes, one per phase)

#### 3.2.1. Market & Economy Scene
**Purpose**: Resource trading and economic management

**Components**:
- **Phase Indicator**: Clear "Market & Economy" header
- **Resource Display**: Current credits and resource counts
- **Market Interface**: Buy/sell buttons for each resource
- **Resource Prices**: Current market prices for each resource
- **Next Phase Button**: Large button to advance to Worker Actions
- **Help Button**: "?" icon for help menu

**Layout**:
- **Header**: Phase indicator and turn counter
- **Center**: Market interface with resource grid
- **Bottom**: Next Phase button
- **Corner**: Help button

#### 3.2.2. Worker Actions Scene
**Purpose**: Main gameplay interface for worker management

**Components**:
- **Phase Indicator**: Clear "Worker Actions" header
- **Game Grid**: Central grid showing plots and workers
- **Worker Panel**: Selected worker information and actions
- **Plot Panel**: Selected plot information and development options
- **Resource Display**: Current credits and resources
- **Next Phase Button**: Large button to advance to Plot Production
- **Help Button**: "?" icon for help menu

**Layout**:
- **Header**: Phase indicator and turn counter
- **Center**: Game grid (largest area)
- **Side Panel**: Worker and plot information
- **Bottom**: Next Phase button
- **Corner**: Help button

#### 3.2.3. Plot Production Scene
**Purpose**: Display production results and advance turn

**Components**:
- **Phase Indicator**: Clear "Plot Production" header
- **Production Summary**: Results from all plots
- **Resource Changes**: New resources generated
- **Achievement Notifications**: Any new achievements unlocked
- **Next Turn Button**: Large button to start next turn
- **Help Button**: "?" icon for help menu

**Layout**:
- **Header**: Phase indicator and turn counter
- **Center**: Production results and notifications
- **Bottom**: Next Turn button
- **Corner**: Help button

### 3.3. Game Over Scene
**Purpose**: Shows current game results and comparison to previous best

**Components**:
- **Game Result**: Win/lose/achievement result
- **Current Game Results**:
  - Turns survived
  - Total profit earned
  - Achievements unlocked
- **Comparison to Previous Best**:
  - "New Record!" indicators if applicable
  - Side-by-side comparison with previous best
- **Play Again Button**: Start new game
- **Back to Welcome Button**: Return to welcome scene
- **Help Button**: "?" icon for help menu

**Layout**:
- **Header**: Game result and congratulations
- **Center**: Results comparison
- **Bottom**: Action buttons (Play Again, Back to Welcome)
- **Corner**: Help button

---

## 4. Help Menu System

### 4.1. Help Menu Structure
**Purpose**: Provide static help content accessible from any scene

**Components**:
- **Modal Overlay**: Semi-transparent background
- **Help Content**: Scrollable text and images
- **Topic Navigation**: List of help topics
- **Close Button**: Return to game
- **Topic Sections**:
  - Getting Started: Basic game concepts
  - Turn Phases: How the three phases work
  - Workers: Worker types and actions
  - Plots: Plot development and resources
  - Market: Buying and selling resources
  - Achievements: How to unlock achievements
  - Tips & Strategies: General gameplay advice

**Implementation**:
- **"?" Button**: Always visible in corner of each scene
- **Modal Popup**: Overlay with help content
- **Topic List**: Scrollable list of help topics
- **Content Area**: Text and images for selected topic
- **Back Button**: Return to game

---

## 5. Visual Design Principles

### 5.1. Mobile-First Design
- **Touch-Friendly**: Minimum 44px touch targets
- **Responsive Layout**: Works on various screen sizes
- **Portrait Optimized**: Primary orientation for mobile
- **Landscape Support**: Functional but not primary

### 5.2. Information Hierarchy
- **Primary Actions**: Most important buttons are largest and most prominent
- **Secondary Actions**: Less important actions are smaller or in menus
- **Contextual Information**: Show relevant info based on current state
- **Progressive Disclosure**: Hide complexity until needed

### 5.3. Visual Feedback
- **Clear States**: Plots show clear visual states (unsettled, settled, developed)
- **Worker Visualization**: Clear indication of worker locations and types
- **Resource Changes**: Visual feedback for resource gains/losses
- **Achievement Notifications**: Clear celebration of achievements

### 5.4. Accessibility
- **High Contrast**: Ensure text is readable on all backgrounds
- **Scalable Text**: Support for larger text sizes
- **Touch Targets**: Large enough for users with motor difficulties
- **Color Independence**: Information not conveyed by color alone

---

## 6. Interaction Patterns

### 6.1. Touch Interactions
- **Tap to Select**: Workers, plots, buttons
- **Tap to Act**: Perform actions on selected items
- **Swipe to Scroll**: Help menu, achievement lists
- **Long Press**: Additional options (if needed)

### 6.2. Navigation Flow
- **Linear Progression**: Welcome → Turn Phases → Game Over → Welcome
- **No Back Navigation**: Each scene has clear forward progression
- **Resume Option**: Welcome scene handles interrupted games
- **Help Access**: Always available via "?" button

### 6.3. State Management
- **Scene Transitions**: Smooth transitions between scenes
- **State Persistence**: Automatic save between phases
- **Error Handling**: Clear error messages and recovery options
- **Loading States**: Brief loading indicators for state changes

---

## 7. Technical Implementation

### 7.1. Scene Management
- **Phaser 3 Scenes**: Each scene is a separate Phaser scene
- **Scene Transitions**: Smooth fade transitions between scenes
- **State Passing**: GameState passed between scenes
- **Scene Cleanup**: Proper cleanup when leaving scenes

### 7.2. UI Components
- **Button System**: Consistent button styling and behavior
- **Modal System**: Reusable modal for help menu
- **Grid System**: Flexible grid for game board
- **Text System**: Consistent typography and sizing

### 7.3. Responsive Design
- **Flexible Layouts**: Adapt to different screen sizes
- **Touch Optimization**: Optimized for touch interaction
- **Performance**: Smooth 30fps on target devices
- **Memory Management**: Efficient asset loading and cleanup

---

## 8. MVP Considerations

### 8.1. Minimal Complexity
- **Three Scenes Only**: Welcome, Turn Phases, Game Over
- **No Pause System**: No real-time elements requiring pause
- **Simple Navigation**: Linear progression with clear next steps
- **Essential Features Only**: Core gameplay without extras

### 8.2. Record Tracking
- **Previous Best Display**: Show longest run and most profitable game
- **New Record Indicators**: Celebrate when players beat their records
- **Persistent Storage**: Save records in localStorage
- **Simple Metrics**: Focus on turns and profit

### 8.3. Help System
- **Static Content**: No dynamic tutorial logic
- **Always Available**: "?" button on every scene
- **Topic-Based**: Organized help content
- **Easy to Update**: Simple text and image content

---

## 9. Future Enhancements

### 9.1. Advanced UI Features
- **Animations**: Smooth animations for state changes
- **Sound Effects**: Audio feedback for interactions
- **Particle Effects**: Visual effects for achievements
- **Custom Themes**: Different visual themes

### 9.2. Enhanced Help System
- **Interactive Tutorial**: Step-by-step guided tutorial
- **Video Help**: Embedded video explanations
- **Community Tips**: User-generated tips and strategies
- **Contextual Help**: Help that appears based on current situation

### 9.3. Additional Scenes
- **Settings Scene**: More detailed game settings
- **Statistics Scene**: Detailed game statistics
- **Achievement Gallery**: Full achievement showcase
- **Community Scene**: Interaction with discovered communities

---

*Document Version: 1.0*  
*Last Updated: [Current Date]*  
*Related Documents: worker-design.md, plot-design.md, market-design.md, map-design.md, game-state-design.md*
