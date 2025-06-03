# Design Document: Snub Disphenoid Visualizer - Current Issue Resolution

## Current Problem

**Error:** `Uncaught ReferenceError: isSeparated is not defined`

This error occurs because during the refactoring from a hierarchical separation system to an independent control system, there are remaining references to the old `isSeparated` variable that was not completely replaced with the new `areCoresSeparated` variable.

## Background: System Refactoring

### **Previous System (Hierarchical)**
```
Unified Model
    ↓ "Separate Pieces"
2-Piece Separation (isSeparated = true)
    ↓ "Separate Caps" 
4-Piece Separation (areCapsSeparated = true)
    ↓ "Join Cores"
3-Piece Configuration (areCoresJoined = true)
```

### **New System (Independent)**
```
Unified Model
    ↓ "Separate Cores" OR "Separate Caps" (any order)
4 Possible States:
1. Cores Joined + Caps Joined (Unified)
2. Cores Separated + Caps Joined (2 pieces)  
3. Cores Joined + Caps Separated (3 pieces)
4. Cores Separated + Caps Separated (4 pieces)
```

## Technical Architecture

### **State Variables**

#### **Core Separation State**
- `areCoresSeparated` (boolean) - replaces old `isSeparated`
- `areCoresSeparatingAnimating` (boolean)
- `targetCoreSeparationState` (boolean)
- `coreSeparationLerpFactor` (0-1)

#### **Cap Separation State**  
- `areCapsSeparated` (boolean)
- `areCapsSeparatingAnimating` (boolean)
- `targetCapSeparationState` (boolean)
- `capSeparationLerpFactor` (0-1)

#### **Removed Variables**
- ❌ `isSeparated` (replaced with `areCoresSeparated`)
- ❌ `isSeparatingAnimating` (replaced with `areCoresSeparatingAnimating`)
- ❌ `targetSeparationState` (replaced with `targetCoreSeparationState`)
- ❌ `separationLerpFactor` (replaced with `coreSeparationLerpFactor`)
- ❌ `areCoresJoined` (no longer needed - cores are either separated or joined)
- ❌ `areCoresJoiningAnimating` (no longer needed)
- ❌ `targetCoreJoinState` (no longer needed)
- ❌ `coreJoinLerpFactor` (no longer needed)

### **Control Mapping**

#### **Old Controls**
```html
<button id="toggleSeparationButton">Separate Pieces</button>
<button id="toggleCapSeparationButton">Separate Caps</button>  
<button id="toggleCoreJoinButton">Join Cores</button>
```

#### **New Controls**
```html
<button id="separateCoresButton">Separate Cores</button>
<button id="separateCapsButton">Separate Caps</button>
```

## Issue Resolution Strategy

### **Step 1: Complete Variable Replacement**
Search and replace all instances of:
- `isSeparated` → `areCoresSeparated`
- `isSeparatingAnimating` → `areCoresSeparatingAnimating`
- `targetSeparationState` → `targetCoreSeparationState`
- `separationLerpFactor` → `coreSeparationLerpFactor`

### **Step 2: Logic Simplification**
Remove complex hierarchical dependencies:
- Eliminate "join cores" logic (cores are simply separated or not)
- Simplify state transitions
- Remove conditional resets between separation types

### **Step 3: Animation Flow**
```
Core Animation:
areCoresSeparated = false → piece groups at (0,0,0)
areCoresSeparated = true → piece groups at ±separation distance

Cap Animation:  
areCapsSeparated = false → caps joined to cores
areCapsSeparated = true → caps separated from cores
```

### **Step 4: Visibility Logic**
```javascript
showUnifiedModel = !areCoresSeparated && !areCapsSeparated
showSeparatedPieces = areCoresSeparated && !areCapsSeparated  
showCapSeparatedPieces = areCapsSeparated
```

## Implementation Checklist

### **Variable References to Fix**
- [ ] Animation loop conditions
- [ ] Label positioning logic  
- [ ] Material coloring conditions
- [ ] Visibility control logic
- [ ] Event handler conditions
- [ ] UI button text updates

### **Functions to Update**
- [ ] `updateShapeAndColorControls()`
- [ ] `animate()` - main animation loop
- [ ] Label positioning in animation loop
- [ ] Event listeners for new button IDs

### **Logic to Simplify**
- [ ] Remove core joining animation code
- [ ] Simplify cap animation (no special case for joined cores)
- [ ] Remove hierarchical state dependencies
- [ ] Clean up button visibility logic

## Expected Behavior After Fix

### **Independent Control System**
1. **"Separate Cores"** button works independently
2. **"Separate Caps"** button works independently  
3. Both can be used in any order
4. Smooth animations between all 4 possible states

### **State Combinations**
| Cores | Caps | Result | Pieces |
|-------|------|--------|---------|
| Joined | Joined | Unified Model | 1 |
| Separated | Joined | Two Main Pieces | 2 |
| Joined | Separated | Cores + 2 Caps | 3 |
| Separated | Separated | 2 Cores + 2 Caps | 4 |

### **Animation Quality**
- No discontinuities or jumps
- Smooth transitions between any states
- Labels track vertices correctly
- Proper occlusion handling

## Testing Strategy

1. **Load Test:** Page loads without JavaScript errors
2. **Core Separation:** Smooth animation between joined/separated cores
3. **Cap Separation:** Smooth animation between joined/separated caps  
4. **Combination Tests:** All 4 state combinations work correctly
5. **Label Tracking:** Vertices labels follow geometry in all states
6. **Animation Reversibility:** Can animate back and forth smoothly

## Success Criteria

✅ **No JavaScript Errors:** All `isSeparated` references resolved  
✅ **Independent Controls:** Both buttons work in any order  
✅ **Smooth Animations:** No visual discontinuities  
✅ **Complete Flexibility:** All 4 configurations accessible  
✅ **Proper Label Behavior:** Labels track vertices correctly  
✅ **Consistent Performance:** No animation slowdowns or glitches