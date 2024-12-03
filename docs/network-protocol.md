# Network Protocol

## Overview

Binary UDP protocol designed for real-time game state synchronization.

## Packet Structure

### Header

```cpp
struct PacketHeader {
    uint8_t type;      // Packet type
    uint16_t sequence; // Sequence number
    uint32_t timestamp;// Timestamp
};
```

### Packet Types

1. Connection (0x01)

   ```cpp
   struct ConnectionPacket {
       PacketHeader header;
       uint32_t clientId;
       uint8_t version;
   };
   ```

2. Game State (0x02)

   ```cpp
   struct GameStatePacket {
       PacketHeader header;
       uint16_t entityCount;
       EntityState entities[];
   };
   ```

3. Player Input (0x03)

   ```cpp
   struct InputPacket {
       PacketHeader header;
       uint32_t clientId;
       uint8_t inputs;  // Bitfield
   };
   ```

## Reliability

- Sequence numbers for packet ordering
- Acknowledgment system
- Client-side prediction
- Server reconciliation

## Network Flow

1. Connection Establishment
   - Client sends Connection Request
   - Server validates and responds
   - Client acknowledges

2. Game Loop
   - Client sends inputs (60Hz)
   - Server processes
   - Server broadcasts state (20Hz)

## Error Handling

- Packet validation
- Timeout management
- Reconnection procedure
- State recovery

## Security

- Input validation
- Rate limiting
