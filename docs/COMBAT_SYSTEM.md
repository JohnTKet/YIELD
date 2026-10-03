# YIELD — Combat System

YIELD's combat is designed around the idea that **movement, momentum, technique, and power should function as one system**.

The goal is not to create a traditional Roblox battleground experience where combat revolves around repeating a fixed ground combo and cycling abilities off cooldown.

Instead, the Foundation combat system establishes a common language that can expand into contextual attacks, personalized IMPULSE techniques, aerial combat, movement attacks, counters, and environmental interaction.

> **Movement creates opportunities. Technique converts them into impact.**

---

## Combat Design Goals

YIELD combat is being built around several principles:

- Responsive controls
- Server-authoritative outcomes
- Skill-based movement
- Meaningful momentum
- Readable attack timing
- Full-body animation
- Contextual attacks
- Defensive counterplay
- Personalized techniques
- Strong impact feedback
- Expandable combat grammar

Character progression should increase capability without making player skill irrelevant.

---

# Current Combat Foundation

The current prototype supports:

- Four-hit light combo
- Heavy attacks
- Blocking
- Dodging
- Dodge invulnerability
- Hit stun
- Knockback
- Launch behavior
- Momentum contribution
- Server-side hit detection
- Server-authoritative attack sequencing
- Server-authoritative cooldowns
- Authoritative contact timing
- Input buffering
- Animation presentation
- Hit confirmation
- Hitstop
- Camera impact
- Impact VFX
- Contextual attack architecture

These systems form the **Foundation combat layer**.

They are not intended to represent YIELD's final combat depth.

---

# Combat Flow

A normal combat action follows this general pipeline:

```text
PLAYER INPUT
     │
     ▼
Semantic Action
     │
     ▼
Combat Request
     │
     ▼
────────── SERVER ──────────
     │
     ▼
Request Validation
     │
     ▼
Combat State Validation
     │
     ▼
Attack Selection
     │
     ▼
Attack Accepted
     │
     ├── AttackId
     ├── Action
     └── Combo / Context
     │
     ▼
────────── CLIENT ──────────
     │
     ▼
Animation Presentation
     │
     ▼
────────── SERVER ──────────
     │
     ▼
Contact Timing
     │
     ▼
State Revalidation
     │
     ▼
Hit Detection
     │
     ▼
Combat Outcome
     │
     ├── Damage
     ├── Knockback
     ├── Stun
     └── Defensive Resolution
     │
     ▼
HitConfirmed
     │
     ▼
────────── CLIENT ──────────
     │
     ▼
VFX / Hitstop / Camera
```

The client requests the action.

The server determines whether the action is valid and what gameplay outcome actually occurs.

---

# Foundation Light Combo

The current Foundation combo contains four attacks:

```text
Light 1
   ↓
Light 2
   ↓
Light 3
   ↓
Light 4
   ↓
Combo Reset
```

Each attack has its own animation, timing, impact characteristics, and server-owned combo state.

The four-hit sequence provides a reliable baseline for testing combat architecture.

It is intentionally **not the final form of YIELD's attack system**.

As contextual combat expands, movement state and technique selection can change what attack is performed.

---

# Heavy Attacks

Heavy attacks provide a higher-commitment alternative to the light sequence.

Compared with Foundation light attacks, Heavy attacks are designed around:

- Greater commitment
- Stronger impact
- Increased knockback
- Longer recovery
- More readable anticipation

Heavy attacks use the same authoritative combat pipeline as light attacks rather than operating as a separate combat system.

Future contextual states may allow different Heavy techniques depending on movement, momentum, IMPULSE, or player build.

---

# Server Authority

Combat-critical outcomes are owned by the server.

The client does not determine:

```text
Successful Hit
Damage
Knockback
Stun
Combo State
Attack Cooldown
Dodge Validity
Defensive Outcome
```

Instead, the client communicates intent.

Example:

```text
CLIENT
LightAttack requested

        ↓

SERVER
Is the character valid?
Is the attack allowed?
Is recovery complete?
What attack should occur?
When does contact happen?
What is inside the hitbox?
What outcome is valid?
```

This reduces reliance on client-provided combat information and creates a stronger multiplayer foundation.

---

# Attack IDs

Every accepted attack receives a server-generated **AttackId**.

Attack IDs allow presentation systems to associate animation and feedback with the correct authoritative action.

```text
Attack Request
     ↓
Accepted
     ↓
AttackId = 17
     ↓
Presentation for Attack 17
```

Rejected requests do not receive accepted attack presentation state.

Attack IDs also help prevent stale presentation information from being treated as current combat state.

---

# Contact Timing

Damage is not resolved simply because an attack button was pressed.

Each Foundation attack has a defined contact point:

| Attack | Contact |
|---|---:|
| Light 1 | 0.10 s |
| Light 2 | 0.13 s |
| Light 3 | 0.15 s |
| Light 4 | 0.19 s |
| Heavy | 0.29 s |

The server schedules contact after accepting the attack.

At contact, relevant state is checked again.

```text
Attack Accepted
      ↓
Animation Begins
      ↓
Contact Time Reached
      ↓
Character Still Valid?
      ↓
Combat State Still Valid?
      ↓
Current Hitbox Query
      ↓
Resolve Outcome
```

This allows gameplay contact to better correspond with visual contact.

It also means the server evaluates target positions at the time the strike reaches its contact point rather than relying entirely on positions from the original input frame.

---

# Hit Detection

Foundation melee hit detection is performed server-side.

The server evaluates the attack hitbox at the authoritative contact point.

This supports situations such as:

```text
Target outside range at attack start
               ↓
Target enters range before contact
               ↓
Potential Hit
```

and:

```text
Target inside range at attack start
               ↓
Target escapes before contact
               ↓
Whiff
```

Targets are prevented from receiving duplicate hits from the same resolved contact.

---

# Defensive States

Combat currently includes defensive mechanics such as:

### Blocking

Blocking provides a defensive state that can affect incoming combat resolution.

### Dodging

Dodging uses server-controlled cooldown and temporary invulnerability behavior.

### Hit Stun

Successful attacks can temporarily restrict the target's combat behavior.

### Barrier / Defensive Resolution

The combat architecture supports defensive outcomes that can prevent normal damage while still producing appropriate server-confirmed feedback.

These systems are designed to create counterplay rather than allowing offense to operate without interaction.

---

# Momentum

Momentum is one of YIELD's defining combat concepts.

Movement should influence impact.

The conceptual model is:

> **Base Power + Velocity/Momentum + IMPULSE Output + Technique + Ability Modifiers = Final Impact**

This is a design model, not the final literal damage equation.

The current Foundation already includes a bounded server-calculated momentum contribution.

The server captures relevant movement information rather than trusting the client to report its own combat multiplier.

Momentum is capped so extreme velocity cannot create unlimited damage.

---

# Why Momentum Matters

Traditional combat can treat these situations identically:

```text
Standing Punch
      =
Full-Speed Sprinting Punch
```

YIELD's long-term design does not.

Instead:

```text
Standing Attack
      ↓
Base Technique

Accelerating Attack
      ↓
Movement-Enhanced Technique

High-Momentum Attack
      ↓
Potential Contextual Technique
```

Eventually, sufficient momentum may affect not only numerical impact but also **which technique is selected**.

---

# Combat Grammar

YIELD uses a shared motion and combat vocabulary:

```text
Anticipation
     ↓
Acceleration
     ↓
Contact
     ↓
Follow-Through
     ↓
Recovery
```

These phases help coordinate:

- Animation
- Attack timing
- Hit detection
- Combo windows
- Branch windows
- Cancel opportunities
- Impact feedback
- Future special techniques

This creates a common structure that can be used by both standard attacks and IMPULSE abilities.

---

# Semantic Input

Combat logic operates on actions rather than specific physical buttons.

Examples include:

```text
LightAttack
HeavyAttack
Block
Dodge
```

This allows physical input mapping to remain separate from gameplay behavior.

The architecture can therefore support future:

- Control rebinding
- Gamepad layouts
- Touch controls
- Alternate control schemes
- Context-sensitive actions

without requiring the core combat system to be redesigned around each input device.

---

# Input Buffering

The Foundation includes a short combat input buffer.

This allows an attack input made slightly before the previous action becomes available to be remembered briefly rather than immediately discarded.

The goal is to make combat feel responsive without allowing unrestricted attack queuing.

Input buffering will become increasingly important as YIELD introduces branching combos and special techniques.

---

# Contextual Attacks

The next major combat expansion is **contextual attack selection**.

Instead of always producing the same attack from the same input, the system can consider how the character is currently moving.

Planned contexts include:

```text
GroundLight
SprintLight
BurstLight
AirLight
Launcher
DownwardAttack
MomentumHeavy
```

Example:

```text
LightAttack
    │
    ├── Standing ──────► GroundLight
    │
    ├── Sprinting ─────► SprintLight
    │
    ├── After Burst ───► BurstLight
    │
    └── Airborne ──────► AirLight
```

This connects YIELD's movement system directly to its combat system.

---

# Branching Combat

The four-hit Foundation combo provides the baseline, but the long-term combat design is intended to branch.

Conceptually:

```text
Light
  │
  ├── Light
  │     └── Continue Sequence
  │
  ├── Heavy
  │     └── Heavy Branch
  │
  ├── Dodge
  │     └── Defensive Cancel
  │
  └── Movement
        └── Context Change
```

Future technique and Affinity systems can expand this further.

The objective is for combat decisions to matter more than simply reaching the final hit of a fixed combo.

---

# Special Techniques

Special techniques will use the same combat language rather than becoming an unrelated ability system.

A technique can define:

```text
Animation
Contact Timing
Movement Context
IMPULSE Cost
Output Interaction
Hit Behavior
Recovery
Conditions
Presentation
```

while authoritative gameplay resolution remains on the server.

---

# Soaring Uppercut

The first dedicated special technique currently being developed in the YIELD Move Lab is the **Soaring Uppercut**.

Its intended motion structure is:

```text
Compression
     ↓
Explosive Drive
     ↓
Uppercut Contact
     ↓
Rising Extension
     ↓
Apex
     ↓
Recovery
```

The animation communicates the superhuman launch.

The gameplay system will eventually control the actual physical movement.

```text
ANIMATION
Technique presentation

        +

GAMEPLAY
Launch force
Movement
Hit detection
Combat outcome
```

This prevents animation root motion from becoming responsible for authoritative character physics.

**Current Status:** Animation development / not yet integrated into production combat.

---

# Animation and Gameplay Separation

YIELD intentionally separates visual animation from authoritative gameplay.

An animation can show:

- Wind-up
- Strike
- Launch
- Follow-through
- Recovery

but it does not independently decide:

- Whether an attack is allowed
- Whether a target was hit
- How much damage occurred
- How much knockback occurred
- Whether the attacker actually receives gameplay movement

This separation allows animations to remain expressive without making visual assets responsible for game-state authority.

---

# Impact Feedback

Confirmed combat contact can produce client presentation such as:

- Impact flash
- Directional streaks
- Particles
- Shock effects
- Hitstop
- Camera impulse

The server generates confirmation only after a valid combat outcome.

```text
Whiff
  ↓
No HitConfirmed
  ↓
No Full Impact Feedback
```

versus:

```text
Valid Contact
      ↓
HitConfirmed
      ↓
Impact Presentation
```

This helps feedback correspond to actual combat events.

---

# Environmental Combat

Long-term YIELD combat is intended to interact with the environment.

The planned destruction model uses staged state changes:

```text
Normal
   ↓
Damaged
   ↓
Heavily Damaged
   ↓
Destroyed
   ↓
Rebuilt
```

Powerful techniques may eventually create different environmental responses depending on impact, Output, technique, and world-object properties.

Environmental destruction is **planned and not currently part of the stable combat Foundation**.

---

# PvE and PvP

The same core combat architecture is intended to support both PvE and PvP.

Future opponents may include:

- Standard enemies
- Elite enemies
- Superhuman NPCs
- Bosses
- World-event threats
- Other players

The goal is to avoid building entirely separate combat rules for every type of encounter.

Server-authoritative attack resolution provides a common foundation that can expand across these systems.

---

# Current vs. Planned

## Implemented / Prototyped

- Four-hit light combo
- Heavy attack
- Blocking
- Dodging
- Dodge invulnerability
- Hit stun
- Knockback
- Launch behavior
- Momentum contribution
- Server attack validation
- Server combo sequencing
- Server hit detection
- Contact timing
- Input buffering
- Combat animation runtime
- Locomotion/combat blending
- Hit confirmation
- Hitstop
- Camera impact
- Impact VFX
- Contextual attack architecture

## In Development

- SprintLight
- Individually authored special techniques
- Soaring Uppercut
- Expanded contextual attack selection

## Planned

- Aerial combat expansion
- Movement attack branches
- Launch follow-ups
- Counters / parries
- Grabs
- Additional cancel/branch behavior
- IMPULSE Output integration
- Affinity-specific techniques
- Player-created build specialization
- Environmental combat interaction
- NPC combat
- Boss mechanics
- PvP expansion

---

# Combat Philosophy

YIELD should reward players for understanding:

```text
Movement
Position
Timing
Momentum
Technique
Defense
IMPULSE
Opponent Behavior
```

rather than only:

```text
Level
+
Ability Cooldowns
+
Raw Stats
```

Progression matters.

Power matters.

But neither should completely replace skill.

> **The goal is not simply to unlock stronger attacks. The goal is to learn how to fight like the superhuman you are building.**

---

## Current Status

**Foundation Combat:** Operational  
**Contextual Combat:** In Development  
**Special Techniques:** In Development  
**Full IMPULSE Combat Integration:** Planned

---

[← IMPULSE System](IMPULSE_SYSTEM.md)  
[← Development Log](DEVELOPMENT_LOG.md)  
[← Technical Architecture](ARCHITECTURE.md)  
[← YIELD Overview](../README.md)
