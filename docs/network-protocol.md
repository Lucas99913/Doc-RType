# R-Type Network Protocol Documentation

## Overview

The R-Type game uses a UDP-based protocol optimized for real-time multiplayer gaming. The server runs at 60 FPS and handles game state synchronization, player actions, and enemy spawning.

## Message Types

### Client to Server Messages

1. `createPlayer`
   - Sent when a new player connects
   - Server responds with player position and ID
   - Also triggers a broadcast of all existing players

2. `playerMove`
   ```cpp
   struct vel_t {
       float velx;  // Range: -10 to 10
       float vely;  // Range: -10 to 10
   };
   ```
   - Server validates movement before broadcasting
   - Invalid movements are discarded

3. `createBullet`
   ```cpp
   struct pos_t {
       float posx;
       float posy;
       size_t id;
   };
   ```
   - Triggers bullet creation on server
   - Server broadcasts bullet creation to all clients

### Server to Client Messages

1. `createPlayer` (Response)
   ```cpp
   struct player_t {
       float posx;
       float posy;
       size_t id;
       int sprite_index;
   };
   ```

2. `connectedPlayer`
   - Sent to notify about existing players
   - Same format as createPlayer response

3. `createEnnemy`
   ```cpp
   struct pos_t {
       float posx;  // Random position between 1600-1900
       float posy;  // Random position between 200-800
       size_t id;
   };
   ```

4. `destroy`
   ```cpp
   struct destroy_t {
       size_t id;  // Entity ID to destroy
   };
   ```

5. `resyncPlayer`
   - Sent every 2 frames
   - Contains current positions of all players
   - Ensures state synchronization

## Game Loop & Timing

```cpp
const int FRAMERATE = 60;  // Server tick rate

// Main server loop
while (true) {
    1. Process network messages
    2. Update player positions
    3. Run collision systems
    4. Sync player states (every 2 frames)
    5. Sleep to maintain 60 FPS
}
```

## State Management

### Player State
- Position (x, y)
- Velocity (x, y)
- Collision box
- Health
- ID

### Enemy State
- Position (x, y)
- Velocity (-2, 0 for standard movement)
- Collision box (108x102 units)
- Health (100 points)
- ID

### Bullet State
- Position (x, y)
- Velocity (10 units horizontal)
- Collision box (72.5x30 units)
- Damage (25 points)
- ID

## Network Optimizations

### Movement Validation
```cpp
bool isMoveValid(vel_t vel) {
    return vel.velx >= -10 && vel.velx <= 10 &&
           vel.vely >= -10 && vel.vely <= 10;
}
```

### State Synchronization
- Full state resync every 2 frames
- Immediate broadcast of critical events (shooting, destruction)
- Authoritative server model with client prediction

### Error Handling
- Out of bounds entity cleanup
- Collision detection and resolution
- Health and damage management

## Game Start Sequence

1. Server waits for required number of players
2. Lobby starts when player count reached
3. Enemy spawn timer initializes (10 second intervals)
4. Player sync timer starts (50ms intervals)
5. Game systems activate

## Security Considerations

1. Movement Validation
   - Speed limits enforced
   - Position bounds checked

2. Entity Validation
   - Server validates all entity creation
   - Server authoritative over game state

3. Collision Detection
   - Server-side verification
   - Damage calculation server-authoritative

4. Resource Management
   - Automatic cleanup of out-of-bounds entities
   - Memory management through component system
