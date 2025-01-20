# Accessibility Features

## System Architecture

### Audio System Architecture

The following diagram shows how the audio system is structured and how it interacts with different parts of the game:

```mermaid
classDiagram
    class SoundManager {
        -soundBuffers: Map<string, SoundBuffer>
        -sounds: Map<string, Sound>
        +getInstance() SoundManager
        +loadSound(name: string, filepath: string) bool
        +playSound(name: string) void
        -SoundManager()
    }

    class GameClient {
        -window: RenderWindow
        +updateGame(player: Entity)
        +destroyEntity(destroy: destroy_t, player: Entity)
    }

    class AudioConfig {
        +checkAudio() int
        +setMaxVolume() void
    }

    GameClient --> SoundManager : uses
    AudioConfig --> SoundManager : configures

    note for SoundManager "Singleton pattern\nManages all game audio"
    note for AudioConfig "Handles audio settings\nand configuration"
```

### Accessibility Feature Flow

This diagram illustrates how accessibility features are initialized and managed during game execution:

```mermaid
flowchart TD
    A[Game Start] --> B{Check Command Line Args}
    B -->|"-audio"| C[Enable Audio Description]
    B -->|"-colors"| D[Enable Colorblind Mode]
    B -->|"-sound"| E[Set Max Volume]

    C --> F[Create is_audio.txt]
    D --> G[Run color_settings.sh]
    E --> H[Configure System Volume]

    F --> I[Initialize SoundManager]
    G --> J[Register Cleanup Handlers]

    I --> K[Load Sound Files]
    K --> L[Game Loop]
    J --> L
    H --> L

    L --> M{Game Event}
    M -->|"Player Movement"| N[Play Movement Sound]
    M -->|"Shooting"| O[Play Weapon Sound]
    M -->|"Enemy Spawn"| P[Play Alert Sound]

    N --> L
    O --> L
    P --> L
```

### Event Sound Sequence

The following sequence diagram shows how game events trigger audio feedback:

```mermaid
sequenceDiagram
    participant User
    participant GameClient
    participant SoundManager
    participant AudioSystem

    User->>GameClient: Game Action
    GameClient->>GameClient: Process Event
    GameClient->>SoundManager: playSound(eventType)

    alt Audio Description Enabled
        SoundManager->>AudioSystem: Load Sound Buffer
        AudioSystem->>AudioSystem: Process Sound
        AudioSystem-->>User: Play Audio Feedback
    else Audio Description Disabled
        SoundManager-->>GameClient: Skip Audio
    end

    Note over GameClient,SoundManager: Sound types:<br/>1. Movement<br/>2. Shooting<br/>3. Enemy Spawn<br/>4. Ambient
```

## Audio Features

### Audio Description Mode

- Activated via `-audio` command line flag
- Provides audio cues for game events:
  - Shooting sounds
  - Movement feedback
  - Enemy appearance notifications
  - Lobby ambiance

### Enhanced Audio Support

- Maximum volume setting via `-sound` flag
- Clear distinct sounds for different game events
- Audio feedback for all major game actions

## Visual Accessibility

### Colorblind Mode

- Activated via `-colors` command line flag
- Modifies game colors for better visibility
- Automatic restoration of default colors on game exit
- Graceful cleanup handling for unexpected termination

## Command Line Options

```bash
./r-type_client [options]
Options:
  -audio    Enable audio descriptions
  -colors   Enable colorblind mode
  -sound    Set maximum volume
```

## Implementation Details

### Audio System

- Uses SFML Audio system
- Managed through SoundManager singleton
- Sound files located in `sound_board/`:
  - Tire.wav: Shooting effects
  - Déplacement.wav: Movement sounds
  - Devant.wav: Enemy appearance
  - Lobby.wav: Lobby ambiance

### Audio Configuration

- State stored in `is_audio.txt`
- Runtime toggling of audio features
- Persistent settings between sessions

### Visual Modifications

- Color scheme adjustments through shell script
- Signal handling for proper cleanup
- Support for multiple color vision deficiency types
