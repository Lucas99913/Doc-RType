# Getting Started

## Prerequisites

- C++ Compiler with C++17 support
- CMake (3.x+)
- Package Manager (choose one):
  - Conan
  - Vcpkg
  - CMake CPM
- SFML Library

## Building the Project

```bash
# Clone the repository
git clone https://github.com/yourusername/r-type.git
cd r-type

# Create build directory
mkdir build
cd build

# Configure with CMake
cmake ..

# Build
make
```

## Running the Game

### Server

```bash
./r-type_server [port]
```

Default port: 4242

### Client

```bash
./r-type_client [server-ip] [port]
```

Example: `./r-type_client localhost 4242`

## Controls

- Arrow keys: Move ship
- Space: Shoot
- Tab: Detach/attach Force module
- Escape: Pause game

## Troubleshooting

### Common Issues

1. CMake not finding dependencies:
   - Ensure your package manager is properly configured
   - Check CMAKE_PREFIX_PATH

2. Build errors:
   - Verify C++17 compiler availability
   - Update SFML to latest version

3. Connection issues:
   - Check firewall settings
   - Verify correct IP/port configuration
