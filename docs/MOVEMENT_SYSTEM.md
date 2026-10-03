# YIELD — Movement System

Movement is one of YIELD's primary gameplay systems.

It is not intended to function only as transportation between combat encounters. Movement should be enjoyable on its own and should directly influence combat, exploration, abilities, and player mastery.

> **In YIELD, how you move is part of how you fight.**

---

## Movement Philosophy

YIELD is built around superhuman mobility that still gives the player meaningful control.

The objective is not simply to increase `WalkSpeed`.

Movement should communicate:

- Acceleration
- Momentum
- Weight
- Direction
- Commitment
- Power
- Player skill

The player should feel the difference between standing still, accelerating, sprinting, bursting, flying, and moving at extreme speed.

---

# Current Movement Foundation

The prototype has established systems for:

```text
Standard Movement
      ↓
Acceleration / Deceleration
      ↓
Sprint
      ↓
IMPULSE Consumption
      ↓
Directional Burst
      ↓
High-Speed Movement
      ↓
Momentum
      ↓
Camera + VFX Presentation
```

Current movement work includes smooth acceleration, sprinting, directional Impulse Burst, sustained high-speed traversal, IMPULSE resource interaction, dynamic FOV, camera feedback, speed trails, afterimages, particles, ground-impact effects, and early flight support.

These systems form the movement Foundation that later abilities will build upon.

---

# Acceleration

YIELD does not treat high-speed movement as an instant transition between two fixed speeds.

Movement uses acceleration and deceleration so the character can visibly and mechanically build speed.

Conceptually:

```text
Idle
  ↓
Movement Input
  ↓
Acceleration
  ↓
Running
  ↓
Sprint
  ↓
High-Speed Movement
```

When movement input changes or ends, velocity transitions back toward the appropriate target rather than always stopping immediately.

This creates a stronger sense of momentum and gives movement state meaning to the combat system.

---

# Sprinting

Sprint is the first major movement escalation above standard locomotion.

The current Foundation uses semantic input for sprinting rather than embedding a physical key directly into movement logic.

Default input currently maps:

```text
Left Shift
     ↓
Sprint
```

The semantic action can later be rebound or mapped to other devices without changing the core movement behavior.

Sprint increases traversal speed while interacting with acceleration, animation, camera presentation, and combat context.

---

# IMPULSE Burst

**Impulse Burst** is a short, high-speed movement action that allows the player to rapidly accelerate in a chosen direction.

Current default input:

```text
Q
↓
ImpulseBurst
```

Burst is intended to serve multiple purposes:

```text
Traversal
   +
Evasion
   +
Positioning
   +
Combat Entry
   +
Momentum Generation
```

This is important to YIELD's design.

Movement abilities should not exist only to move around the map. They should create new combat opportunities.

A future attack immediately following a Burst may therefore differ from the same attack performed while standing still.

---

# IMPULSE Resource

Superhuman movement interacts with the player's **IMPULSE** resource.

The prototype includes:

```text
IMPULSE Capacity
      ↓
Movement Consumption
      ↓
Remaining Resource
      ↓
Regeneration
```

This prevents powerful movement from becoming completely disconnected from the game's larger power system.

Long-term, movement efficiency may also interact with progression attributes such as:

```text
Impulse Capacity
Control
Efficiency
Recovery
Output
```

The complete progression implementation remains under development.

---

# Momentum

Momentum connects movement directly to combat.

The current combat Foundation already includes a bounded server-calculated momentum contribution.

Conceptually:

```text
Character Velocity
       ↓
Server Evaluation
       ↓
Bounded Momentum
       ↓
Combat Calculation
```

The client does not simply report its own damage multiplier.

This allows movement to contribute to combat while keeping authoritative outcomes controlled by the server.

---

# Movement + Combat

One of YIELD's defining goals is for movement state to influence attack selection.

The long-term relationship is:

```text
Movement State
      │
      ▼
Attack Context
      │
      ▼
Technique Selection
      │
      ▼
Combat Outcome
```

For example:

```text
Standing + LightAttack
        ↓
GroundLight

Sprinting + LightAttack
        ↓
SprintLight

ImpulseBurst + LightAttack
        ↓
BurstLight

Airborne + LightAttack
        ↓
AirLight
```

This means movement mastery can eventually expand the player's available combat vocabulary.

---

# SprintLight

`SprintLight` is the first contextual attack designed specifically around movement state.

Instead of forcing a normal standing attack animation to play while the character continues sprinting, SprintLight is designed around forward movement.

Its animation emphasizes:

- Running carry
- Forward drive
- Compact anticipation
- Kinetic transfer
- Readable contact
- Recovery toward locomotion

SprintLight has been authored and locally validated as a contextual animation candidate.

Its production animation publication/integration remains under development.

The experiment helped establish the workflow now being used for YIELD's broader contextual and special-move development.

---

# Locomotion / Combat Blending

Movement created an important animation challenge during Foundation development.

Full-body Action-priority combat animations could override locomotion while the character continued physically translating.

The result was:

```text
Character Moving
       +
Legs Playing Attack Animation
       ↓
Visible Foot Sliding
```

Rather than removing movement during attacks, YIELD introduced velocity-aware animation blending.

Conceptually:

```text
Low Velocity
    ↓
Greater Combat Animation Influence

Increasing Velocity
    ↓
Combat + Locomotion Blend

High Velocity
    ↓
Greater Locomotion Influence
```

This allows the player to continue moving while attack animations remain visually compatible with locomotion.

---

# Camera Presentation

Speed should be communicated visually as well as numerically.

The movement Foundation includes dynamic camera presentation such as:

- FOV changes
- Speed feedback
- Burst feedback
- Camera response

As the character accelerates, camera presentation can reinforce the perception of increasing speed.

The objective is to create the sensation of superhuman movement without making the camera unnecessarily difficult to control.

---

# Movement VFX

Movement presentation currently includes prototype effects such as:

```text
Speed Trails
Afterimages
Dash Particles
Ground Shockwaves
```

These effects communicate movement state and IMPULSE usage.

Long-term presentation can also respond to:

```text
Affinity
Output
Velocity
Technique
Character Customization
```

This allows two high-speed characters to eventually feel visually distinct even when both are moving quickly.

---

# Flight

Flight is part of YIELD's broader movement architecture.

The current controller foundation includes semantic actions for:

```text
Flight
FlightAscend
FlightDescend
```

Flight is intended to eventually support more than simply toggling gravity off.

The target design includes:

```text
Takeoff
Hover
Acceleration
Boost
Directional Flight
Ascend / Descend
Air Braking
Combat
Landing
```

Flight should ultimately follow the same design principle as ground movement:

> **Traversal should contain enough control and mastery to be enjoyable by itself.**

Flight remains an area for continued development and refinement.

---

# Extreme-Speed Traversal

YIELD is intended to support characters capable of moving significantly faster than ordinary Roblox locomotion.

Extreme speed introduces technical and design challenges:

```text
Camera Readability
Animation
Collision
Networking
Combat Detection
World Streaming
Environment Scale
Player Control
```

For this reason, extreme-speed gameplay is being developed incrementally rather than simply increasing maximum velocity.

The player must still be able to:

- Steer
- Understand their surroundings
- Enter combat
- Exit combat
- Avoid obstacles
- Control acceleration
- Interpret visual feedback

Raw speed without control would not satisfy YIELD's movement goals.

---

# Vertical Movement

Ground traversal is only one part of the planned movement system.

YIELD is intended to support vertical mobility through systems such as:

```text
Enhanced Jumping
Airborne Techniques
Flight
Launch Attacks
Vertical Bursts
Future Wall Movement
```

Vertical movement also creates new combat states.

For example, the same player can transition through:

```text
Ground Combat
     ↓
Launcher
     ↓
Airborne State
     ↓
Aerial Technique
     ↓
Recovery / Landing
```

This allows movement and combat to evolve together.

---

# Soaring Uppercut

The **Soaring Uppercut** is the first special technique being developed in the dedicated YIELD Move Lab.

It demonstrates an important separation between animation and movement physics.

The animation communicates:

```text
Compression
     ↓
Explosive Upward Drive
     ↓
Uppercut
     ↓
Rising Extension
     ↓
Apex
```

However, the animation itself should not be responsible for authoritative vertical displacement.

Instead:

```text
ANIMATION
Shows the superhuman launch

        +

MOVEMENT / COMBAT SYSTEM
Controls actual launch physics
```

This allows the technique's physical behavior to eventually respond to gameplay variables such as IMPULSE, Output, technique properties, and combat state without requiring the animation itself to determine character position.

---

# Future Wall Movement

Wall movement is planned as another extension of YIELD's mobility system.

Potential applications include:

```text
Wall Running
Wall Launching
Vertical Traversal
Momentum Preservation
Combat Transitions
```

The final implementation has not yet been established.

Wall movement will be introduced only after the current movement and contextual combat foundation is stable enough to support it cleanly.

---

# Movement Skill

Increasing character power should not eliminate the need for movement skill.

A high-speed player should still need to learn:

```text
Acceleration
Turning
Braking
Positioning
Momentum
Resource Management
Combat Entry
Combat Exit
Vertical Control
```

This creates room for mastery.

Two players with similar movement statistics may still perform very differently depending on how effectively they control those systems.

---

# Movement Progression

Future progression may influence attributes such as:

```text
Speed
Acceleration
Stamina
Impulse Capacity
Output
Control
Recovery
Efficiency
```

However, progression should not simply turn movement into:

```text
Higher Level
     =
Automatically Better Movement
```

Player control and technique should remain important.

Increasing capability should ideally create **more opportunities for mastery**, not remove the need for it.

---

# Movement Architecture

The broader movement relationship can be represented as:

```text
                  PLAYER INPUT
                       │
                       ▼
                Movement Intent
                       │
                       ▼
               Movement Controller
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
     Acceleration    IMPULSE      Context
          │            │            │
          └────────────┼────────────┘
                       ▼
                 Character Motion
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
      Animation      Camera         VFX
          │
          ▼
       Momentum
          │
          ▼
        Combat
```

Movement therefore feeds multiple systems rather than existing as an isolated controller.

---

# Current vs. Planned

## Implemented / Prototyped

- Smooth acceleration/deceleration
- Sprinting
- Directional Impulse Burst
- Sustained high-speed movement
- IMPULSE drain/regeneration
- Momentum tracking
- Dynamic FOV
- Camera feedback
- Speed trails
- Afterimages
- Dash particles
- Ground shockwave presentation
- Locomotion/combat blending
- Semantic movement input
- Flight foundation
- Contextual movement/combat architecture

## In Development

- SprintLight integration
- Movement-dependent attacks
- Special techniques
- Soaring Uppercut
- Expanded flight behavior

## Planned

- Enhanced jumping expansion
- Wall movement
- Air braking refinement
- Aerial combat
- Burst attacks
- Momentum Heavy attacks
- Advanced flight combat
- IMPULSE Output integration
- Affinity-specific movement
- Extreme-speed world traversal
- Additional movement challenges
- Races and traversal activities

---

# Design Principle

YIELD's movement system ultimately follows one rule:

> **Movement should be gameplay, not downtime between gameplay.**

Running across the city, accelerating into combat, launching into the air, changing direction at speed, flying between buildings, and converting momentum into an attack should feel like parts of the same system.

The long-term goal is for players to enjoy moving through YIELD's world even when they have no mission marker telling them where to go.

---

## Current Status

**Ground Movement Foundation:** Operational  
**IMPULSE Burst:** Operational  
**Movement Presentation:** Operational / Expanding  
**Flight:** Foundation / In Development  
**Contextual Movement Attacks:** In Development  
**Advanced Traversal:** Planned

---

[← Combat System](COMBAT_SYSTEM.md)  
[← IMPULSE System](IMPULSE_SYSTEM.md)  
[← Development Log](DEVELOPMENT_LOG.md)  
[← Technical Architecture](ARCHITECTURE.md)  
[← YIELD Overview](../README.md)
