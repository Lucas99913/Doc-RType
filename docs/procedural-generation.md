# Procedural Level Generation

## Overview

The R-Type game engine now includes a procedural level generation system that creates unique game levels dynamically. This system generates random but playable level layouts using a binary grid system.

## Technical Implementation

### Random Number Generation

```cpp
std::random_device rd;
std::mt19937 gen(rd());
std::uniform_int_distribution<> dis(0, 99);
```

- Uses Mersenne Twister (MT19937) for high-quality random numbers
- Uniform distribution for balanced obstacle placement
- 1% probability for obstacle placement (value < 1)

### Map Generation Parameters

- Grid dimensions: 13x23 (MAP_WIDTH × MAP_HEIGHT)
- Binary representation: 0 = empty, 1 = obstacle
- Format: Text-based level file

### Level File Format

```
0000100000000
0001000000000
0000000100000
...
```

Each character represents a cell in the game grid:
- '0': Empty space
- '1': Obstacle placement

### Integration with Game Engine

The procedural generation system is integrated with:
- JsonManager for level data handling
- LuaManager for custom level scripting
- AssetManager for resource loading

### File Structure

- Generated levels stored in `assets/RType/levels/`
- Dynamic creation during server initialization
- Seamless loading into game engine

## Usage Example

```cpp
std::string generateRandomMap() {
    // Initialize random generator
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<> dis(0, 99);

    // Generate map
    std::stringstream mapStream;
    for (int y = 0; y < MAP_HEIGHT; y++) {
        for (int x = 0; x < MAP_WIDTH; x++) {
            mapStream << (dis(gen) < 1 ? '1' : '0');
        }
        if (y < MAP_HEIGHT - 1)
            mapStream << '\n';
    }
    return mapStream.str();
}
```

## Considerations

- Balance between randomness and playability
- Consistent obstacle spacing
- Performance optimization for real-time generation
- Easy modification of generation parameters