# API Reference

## Registry

### Core Functions

```cpp
class Registry {
public:
    // Entity management
    Entity spawn_entity();
    void kill_entity(Entity const& e);
    
    // Component management
    template<typename Component>
    Component& add_component(Entity const& e, Component&& c);
    
    template<typename Component>
    void remove_component(Entity const& e);
    
    // System management
    template<typename... Components, typename Function>
    void add_system(Function&& f);
    
    void run_systems();
};
```

## Components

### Position

```cpp
struct Position {
    float x;
    float y;
};
```

### Velocity

```cpp
struct Velocity {
    float dx;
    float dy;
};
```

### Drawable

```cpp
struct Drawable {
    std::string sprite;
    Rectangle rect;
    Vector2f scale;
};
```

## Systems

### Movement System

```cpp
void movement_system(Registry& registry,
    sparse_array<Position>& positions,
    sparse_array<Velocity>& velocities);
```

### Collision System

```cpp
void collision_system(Registry& registry,
    sparse_array<Position>& positions,
    sparse_array<Collider>& colliders);
```

## Network

### Client

```cpp
class Client {
public:
    bool connect(std::string_view host, uint16_t port);
    void disconnect();
    void update();
    void send_input(InputPacket const& input);
};
```

### Server

```cpp
class Server {
public:
    bool start(uint16_t port);
    void stop();
    void update();
    void broadcast(GameState const& state);
};
```
