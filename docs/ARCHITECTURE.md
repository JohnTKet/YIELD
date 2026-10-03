# YIELD — Technical Architecture

YIELD is an original Roblox superhuman action RPG built in **Roblox Studio and Luau**.

The project uses a modular client/server architecture designed around one central rule:

> **The client requests actions. The server determines authoritative gameplay outcomes.**

This keeps combat responsive while preventing clients from directly controlling critical values such as damage, successful hits, cooldowns, or combat state.

---

## High-Level Architecture

```text
YIELD
│
├── CLIENT
│   ├── Input
│   ├── Movement
│   ├── Combat Requests
│   ├── Animation
│   ├── Camera
│   ├── HUD
│   └── Local VFX
│
├──────────── NETWORK BOUNDARY ────────────
│
└── SERVER
    ├── Request Validation
    ├── Combat State
    ├── Attack Selection
    ├── Cooldowns
    ├── Contact Timing
    ├── Hit Detection
    ├── Damage
    ├── Knockback
    └── Stun / Defensive States
```

---

## Client Responsibilities

The client is responsible primarily for systems that need immediate responsiveness or are presentation-oriented.

These include:

- Player input
- Semantic action requests
- Camera behavior
- HUD updates
- Local animation presentation
- Movement presentation
- Local visual effects
- Hitstop presentation
- Impact feedback
- Input buffering

The client does **not** determine authoritative damage or successful combat hits.

For example, pressing the light-attack input communicates the player's intent to attack.

It does not send:

```text
Damage = 50
Target = Player2
HitSuccessful = true
```

Those outcomes belong to the server.

---

## Server Responsibilities

The server owns gameplay-critical combat state and validates requests before accepting them.

Server authority currently includes:

- Attack validation
- Attack sequencing
- Combo state
- Attack cooldowns
- Character state
- Block state
- Dodge state
- Dodge invulnerability
- Hit stun
- Attack selection
- Contact timing
- Hit detection
- Damage
- Knockback
- Launch behavior
- Momentum contribution
- Defensive outcomes
- Combat lifecycle/reset behavior

This architecture reduces the amount of critical information that must be trusted from the client.

---

# Combat Request Pipeline

A normal attack travels through the following architecture:

```text
Player Input
     │
     ▼
Semantic Combat Request
     │
     ▼
──────────── NETWORK BOUNDARY ────────────
     │
     ▼
Server Validation
     │
     ▼
Attack Selection
     │
     ▼
Attack Accepted
     │
     ├──── AttackId Assigned
     │
     └──── Presentation State Sent to Client
     │
     ▼
Client Animation Begins
     │
     ▼
Authoritative Contact Delay
     │
     ▼
Server Revalidation
     │
     ▼
Hit Detection at Contact
     │
     ▼
Damage / Knockback / Stun
     │
     ▼
HitConfirmed
     │
     ▼
──────────── NETWORK BOUNDARY ────────────
     │
     ▼
Impact VFX / Hitstop / Camera Feedback
```

This separates **player intent**, **visual presentation**, and **authoritative gameplay resolution**.

---

# Server-Authoritative Contact Timing

An important architectural change during development was moving damage resolution away from the moment an attack button is pressed.

An early combat implementation effectively behaved like:

```text
Attack Input
     ↓
Server Accepts Attack
     ↓
Hit Detection / Damage
     ↓
Animation Plays
```

This could cause gameplay contact to occur before the visual attack reached its target.

YIELD now uses:

```text
Attack Accepted
     ↓
Animation Begins
     ↓
Contact Timing
     ↓
Server Revalidation
     ↓
Hit Detection
     ↓
Damage
     ↓
HitConfirmed
```

Foundation combat currently uses attack-specific contact timing:

| Attack | Contact |
|---|---:|
| Light 1 | 0.10 s |
| Light 2 | 0.13 s |
| Light 3 | 0.15 s |
| Light 4 | 0.19 s |
| Heavy | 0.29 s |

The server waits for the appropriate contact point and then evaluates the world **as it exists at contact**, rather than assuming a target hit based on the world state when the input occurred.

---

# Attack IDs and Presentation State

Accepted attacks receive a server-generated **AttackId**.

The AttackId allows presentation systems to associate client feedback with a specific server-approved attack and reject stale presentation information.

Conceptually:

```text
Client:
"I want to perform LightAttack."

Server:
"Request valid.
AttackId = 17
Attack = Light
Combo = 2"

Client:
"Present approved attack 17."
```

Rejected attack requests do not receive an accepted attack state.

This keeps visual presentation synchronized with authoritative combat decisions.

---

# Semantic Input Architecture

Gameplay systems are designed around **actions rather than physical keys**.

Instead of combat logic asking:

```text
Was MouseButton1 pressed?
```

the system works with concepts such as:

```text
LightAttack
HeavyAttack
Block
Dodge
Sprint
ImpulseBurst
Flight
FlightAscend
FlightDescend
```

Physical controls are mapped to those actions separately.

This makes the architecture easier to expand for different input devices, rebinding, contextual attacks, and future abilities.

---

# Contextual Combat

YIELD's combat architecture is being expanded beyond a fixed four-hit combo.

Attack context can eventually distinguish situations such as:

```text
GroundLight
SprintLight
BurstLight
AirLight
Launcher
DownwardAttack
MomentumHeavy
```

The purpose is to connect **movement and combat** rather than treating them as independent systems.

A player attacking after accelerating, sprinting, bursting, flying, or becoming airborne can eventually produce a different technique from the same basic combat input.

---

# Momentum

Momentum is part of YIELD's combat philosophy.

Conceptually:

> **Base Power + Velocity/Momentum + IMPULSE Output + Technique + Ability Modifiers = Final Impact**

The server—not the client—determines the momentum contribution used by authoritative combat calculations.

Momentum scaling is bounded so extreme or manipulated velocity cannot produce unlimited damage.

This allows movement skill to influence combat without allowing velocity alone to determine every encounter.

---

# Animation Architecture

Combat animation is presentation driven by server-approved combat state.

```text
Input
   ↓
Combat Request
   ↓
Server Acceptance
   ↓
Attack Selection
   ↓
Animation Slot
   ↓
AnimationController
   ↓
R15 Animation
```

The current Foundation animation set contains:

```text
Light1
Light2
Light3
Light4
Heavy
```

Animations use **Action priority** and are non-looping.

Special and contextual attacks are being developed individually through a separate animation-authoring workflow before production integration.

---

# Locomotion + Combat Blending

Full-body combat animations initially created a visual problem during movement.

The character could continue translating through the world while a high-priority combat animation overrode the locomotion animation, causing visible foot sliding.

The presentation architecture was updated to account for character velocity.

Conceptually:

```text
Standing
   ↓
High Combat Animation Influence

Walking
   ↓
Blended Combat + Locomotion

Running
   ↓
Greater Locomotion Contribution

High-Speed Movement
   ↓
Movement Remains Visually Readable
```

This allows combat animation to retain its upper-body impact while locomotion continues communicating actual character movement.

---

# Impact Presentation

Successful authoritative contacts can generate a server-confirmed presentation event.

```text
Server Hit Detection
      ↓
Valid Combat Outcome
      ↓
HitConfirmed
      ↓
Client Presentation
      ├── Impact VFX
      ├── Hitstop
      └── Camera Feedback
```

Whiffed attacks do not receive the same confirmed impact presentation.

This prevents the client from producing full hit feedback merely because the player pressed an attack button.

---

# Audio Architecture

The impact-audio architecture has been prepared but audible combat assets are intentionally deferred.

This allows development to continue on gameplay-critical systems without allowing placeholder audio work to block combat, movement, animation, or ability development.

The architecture supports future impact categories and defensive outcomes while remaining silent until approved assets are introduced.

---

# IMPULSE Architecture

IMPULSE is YIELD's original superhuman energy system.

Its planned architecture connects multiple gameplay systems:

```text
                     IMPULSE
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
     Capacity          Output         Control
        │               │               │
        ▼               ▼               ▼
    Abilities        Power Level     Efficiency
        │               │               │
        └───────────────┼───────────────┘
                        ▼
             Movement + Combat
```

IMPULSE is designed to influence more than ability activation.

It will eventually affect movement, combat, techniques, energy consumption, visual presentation, risk, and personalized ability development.

---

# Affinity Architecture

The planned IMPULSE Affinities are:

| Affinity | Primary Domain |
|---|---|
| **DRIVE** | Physical enhancement |
| **VECTOR** | Movement and force |
| **PROJECTION** | Ranged energy |
| **FORGE** | Constructs, armor and weapons |
| **ALTERATION** | Property and body/energy modification |
| **ANOMALY** | Unconventional IMPULSE behavior |

Affinity is not intended to function as a rigid character class.

The architecture is being designed so players sharing the same Affinity can eventually develop substantially different techniques and builds.

---

# Development Principles

YIELD's technical development follows several core principles:

**Modularity**  
Systems should have clear responsibilities instead of accumulating unrelated functionality inside giant scripts.

**Server Authority**  
Gameplay-critical outcomes should be validated and resolved by the server.

**Responsive Clients**  
Input, camera, animation and presentation should remain responsive without granting the client authority over critical gameplay state.

**Incremental Development**  
Major systems are implemented and validated before additional complexity is layered on top.

**Preserve Working Systems**  
New development should extend stable systems rather than repeatedly replacing functional architecture.

**Test Before Expansion**  
Runtime behavior, lifecycle handling and edge cases are validated before a feature is considered part of the stable foundation.

---

# Current Architecture Status

The current prototype has established the foundation for:

```text
Movement
   +
Server-Authoritative Combat
   +
Animation
   +
Contextual Input
   +
Contact Timing
   +
Impact Presentation
   ↓
Personalized Superhuman Combat
```

Current development is expanding this foundation into **contextual attacks and individually authored special techniques**.

These systems will later support IMPULSE Output, Affinity techniques, NPC combat, progression, environmental destruction, bosses, PvP, and dynamic world events.

---

## Technology

**Engine:** Roblox Studio  
**Language:** Luau  
**Platform:** Roblox  
**Version Control / Documentation:** Git + GitHub  
**Architecture:** Modular Client/Server  
**Current Stage:** Prototype / Foundation Development

---

[← Back to YIELD Overview](../README.md)
