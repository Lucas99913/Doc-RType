# Game Engine API Reference

## Core Classes

### Registry

The main container and manager for the ECS system.

```cpp
class Registry {
public:
    // Entity Management
    Entity spawnEntity();
    Entity entityFromIndex(std::size_t index);
    void killEntity(Entity const& entity);

    // Component Management
    template<typename Component>
    sparseArray<Component>& registerComponent();
    
    template<typename Component>
    sparseArray<Component>& getComponents();
    
    template<typename Component>
    void removeComponent(Entity const& from);
    
    template<typename Component>
    typename sparseArray<Component>::reference_type addComponent(Entity const& to, Component& c);
    
    template<typename Component, typename... Params>
    typename sparseArray<Component>::reference_type emplaceComponent(Entity const& to, Params&&... p);

    // System Management
    template<class... Components, typename Function>
    void addSystem(Function&& f);
    
    void runSystems();
    
    template<class... Components, typename Function>
    void runSingleSystem(Function&& f);
};
```

### SparseArray

A sparse container for components.

```cpp
template<typename Component>
class sparseArray {
public:
    // Types
    using value_type = Component;
    using reference_type = value_type&;
    using const_reference_type = value_type const&;
    
    // Element Access
    reference_type operator[](std::size_t idx);
    const_reference_type operator[](std::size_t idx) const;
    bool has(size_type pos) const;
    
    // Iterators
    iterator begin();
    iterator end();
    const_iterator cbegin() const;
    const_iterator cend() const;
    
    // Capacity
    size_type size() const;
    
    // Modifiers
    reference_type insertAt(size_type pos, Component const&);
    reference_type insertAt(size_type pos, Component&&);
    template<class... Params>
    reference_type emplaceAt(size_type pos, Params&&...);
    void erase(size_type pos);
};
```

## Components

### Transform Component
Position, rotation and scale in 3D space.
```cpp
struct transform {
    float _posx, _posy, _posz;       // Position
    float _rotx, _roty, _rotz;       // Rotation
    float _scalex, _scaley, _scalez; // Scale
};
```

### Velocity Component
Movement speed in 3D space.
```cpp
struct velocity {
    float _velx, _vely, _velz;
};
```

### Drawable Component
Marks an entity as renderable.
```cpp
struct drawable {
    bool _isDrawable;
};
```

### Sprite Component
SFML sprite with rectangle definition.
```cpp
struct sprite {
    sf::Sprite _sprite;
    sf::IntRect _rect;
};
```

### Texture Component
SFML texture with offset capabilities.
```cpp
struct texture {
    sf::Texture _texture;
    float _offsetX;
    float _offsetY;
};
```

### Collision Component
3D collision box dimensions.
```cpp
struct colisionBox {
    float _width;
    float _height;
    float _depth;
};
```

### Control Component
Marks an entity as player-controllable.
```cpp
struct controllable {
    bool _isControllable;
};
```

### Other Components
- `unmovable`: Marks an entity as static
- `toDelete`: Marks an entity for deletion

## Systems

### Core Systems
```cpp
// Position Update System
void updatePosSystem(GameEngine& ge,
                    sparseArray<component::transform>& transform,
                    sparseArray<component::velocity>& velocity);

// Sprite Management
void loadSpriteTexture(GameEngine& ge,
                    sparseArray<component::sprite>& sprite,
                    sparseArray<component::texture>& texture);

// Rendering
void drawSprite(GameEngine& ge,
                sparseArray<component::sprite>& sprite,
                sparseArray<component::drawable>& drawable,
                gameEngine::RenderWindow& window);

// Collision Detection
void colisionSystem(GameEngine& ge,
                    sparseArray<component::transform>& transform,
                    sparseArray<component::colisionBox>& colisionBox,
                    sparseArray<component::velocity>& velocity,
                    sparseArray<component::unmovable>& unmovable);

// Memory Management
void garbageCollectorSystem(GameEngine& ge,
                            sparseArray<component::toDelete>& toDelete);
```

### Debug Systems
```cpp
// Debug Logging
void transformLog(GameEngine& ge,
                sparseArray<component::transform> const& transform);
void velocityLog(GameEngine& ge,
                sparseArray<component::velocity> const& velocity);
```

## Usage Example

```cpp
// Create registry
Registry registry;

// Register components
registry.registerComponent<component::transform>();
registry.registerComponent<component::velocity>();
registry.registerComponent<component::drawable>();

// Create entity
auto entity = registry.spawnEntity();

// Add components
registry.addComponent(entity, component::transform{0, 0, 0, 0, 0, 0, 1, 1, 1});
registry.addComponent(entity, component::velocity{1, 0, 0});
registry.addComponent(entity, component::drawable{true});

// Add systems
registry.addSystem<component::transform, component::velocity>(updatePosSystem);

// Run game loop
while (running) {
    registry.runSystems();
}
```