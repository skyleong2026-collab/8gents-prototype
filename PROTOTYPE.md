# 8gents Prototype: Map Escalation + Combat Loop

A React Native / Expo vertical slice prototype testing the core loop of **observing escalation → intervening → seeing world-state consequences** as a unified game experience.

## Architecture

### Core Systems

- **MapScreen** (`src/screens/MapScreen.jsx`)
  - 8x8 grid visualization with real-time escalation cluster
  - Exponential spread algorithm that doubles attack radius every 2 seconds
  - Visual feedback: pulsing boundaries, intensity gradients, numeric spread display
  - Tap infected zones to enter combat

- **CombatScreen** (`src/screens/CombatScreen.jsx`)
  - Lightweight, partially automated combat
  - Parameters inherited from map state:
    - Enemy count scales with tile intensity
    - Hazard count increases nearby heavily-infected tiles
    - Difficulty caps at 5 to avoid overwhelming punishment
  - Player can tap enemies or let auto-attack handle it
  - Combat log with real-time feedback

- **Game Logic** (`src/gameLogic.js`)
  - `spreadCluster()`: Exponential spread every 2 seconds
  - `calculateCombatParams()`: Map state → combat difficulty/enemies/hazards
  - `applyBattleOutcome()`: Combat result → modified grid
  - Victory reduces spread 40% in combat radius
  - Defeat accelerates spread 50% and expands contaminated zones

### Data Flow

```
MapScreen (observe escalation)
    ↓
 [Player taps infected zone]
    ↓
CombatScreen (inherit map state → combat params)
    ↓
 [Player wins/loses]
    ↓
MapScreen (see world consequence)
    ↓
 [Spread continues evolving]
```

## Running the Prototype

### Web (for quick testing)
```bash
cd ~/Documents/8gents-prototype
EXPO_ROUTER_DISABLE_RN_NAVIGATION_CHECK=1 npm run web
# Open browser to http://localhost:8081
```

### iOS Simulator (on Mac)
```bash
cd ~/Documents/8gents-prototype
npm run ios
# Expo CLI will launch the simulator automatically
```

### Physical Device
1. Install Expo Go app from App Store
2. Run: `npm run start`
3. Scan QR code with Expo Go

## Game Flow

1. **Map Phase** (2-second ticks)
   - Observe 8x8 grid with escalation cluster
   - Cluster spreads outward; intensity increases at origin
   - Pulsing boundaries show active spread frontiers
   - Spread Intensity % and Velocity (MODERATE/HIGH/CRITICAL) displayed

2. **Engagement Decision**
   - Tap any infected zone to fight
   - Combat difficulty scales: 1-5 based on local intensity
   - Enemy count: 2-8 depending on map state
   - Hazard count: nearby heavily-infected tiles

3. **Combat** (partially automated)
   - Enemies appear as circles with health (0-3)
   - Auto-attack reduces enemy health every 1.2 seconds
   - Player can tap enemies for manual damage
   - Player takes 1 auto-damage every 1.5 seconds
   - Combat ends when all enemies fall or player health ≤ 0

4. **Outcome**
   - **Victory**: Spread reduced 40% in 5-tile radius, stabilizes area
   - **Defeat**: Spread accelerated 50%, new infection zones spawn
   - Return to Map to see consequence

## Key Design Decisions

### Exponential Spread Algorithm
- Spread distance = `floor(max_intensity / 35)`
- New distance increases every ~5 seconds (when max reaches 35, 70, 105...)
- Intensity decreases with distance: `intensity = max(5, 30 - distance * 8)`
- Creates visually readable "waves" of expanding contamination

### Combat Difficulty Cap
- Max difficulty = 5, never more than 8 enemies
- Goal: teach map interpretation, not punish with overwhelming fights
- Even a fully infected map remains moderately challenging (capped threat)

### Consequence Clarity
- Victory always reduces spread (player agency feels meaningful)
- Defeat has greater impact than victory (risk/reward tension)
- Modifies only combat tile + immediate radius to show localized consequence

### Automation vs. Agency
- Auto-attack ensures combat doesn't require perfect play
- Player taps add skill expression without being mandatory
- Combat log provides feedback loop for learning map state → difficulty

## Tuning Parameters

Edit `src/gameLogic.js` to adjust:

```javascript
const SPREAD_INTERVAL = 2000;        // Spread tick (ms)
const MAX_INTENSITY = 100;           // Cap for intensity scaling
const spreadDistance = Math.floor(intensity / 35); // Spread speed
const enemyCount = Math.min(8, ...) // Cap on enemy count
const playerHealth = Math.min(10, ...)  // Base player HP
```

## Testing Checklist

- [ ] Map loads with 8x8 grid, cluster at (3,3)
- [ ] Cluster visibly expands every 2 seconds
- [ ] Pulsing boundary on expansion frontier
- [ ] Spread Intensity % increments; Velocity updates
- [ ] Tapping infected zone enters Combat
- [ ] Combat inherits difficulty from map (compare coords vs. intensity)
- [ ] Player can see auto-attack reducing enemy health
- [ ] Player can manually tap enemies for damage
- [ ] Combat ends on all-enemies-dead or player-health-zero
- [ ] Victory reduces spread; Defeat expands it
- [ ] Return to Map shows modified grid state
- [ ] Loop repeats: escalation continues, player can re-engage

## Known Limitations (Intentional for MVP)

- Single escalation source (no multi-cluster scenarios)
- No progression, XP, or leveling
- No loadouts, inventory, or upgrades
- No narrative or lore
- Combat rendering is minimal (circles + numbers)
- No mobile gestures (swipe, pinch, drag)
- Web version only; iOS/Android untested until build phase

## Next Steps Post-Prototype

1. **Validate Loop Feel**: Does the consequence → escalation → re-engagement cycle feel like one game?
2. **Difficulty Curve**: At full map infection, is the combat still engaging or frustrating?
3. **Spread Visuals**: Are the pulsing boundaries and color gradients readable at a glance?
4. **Engagement Incentive**: Do players naturally want to intervene, or does escalation feel inevitable?
5. **Combat Clarity**: Can players quickly understand map state → combat difficulty mapping?

---

**Built with**: React Native, Expo, React Navigation
**Prototype Date**: 2026-05-22
