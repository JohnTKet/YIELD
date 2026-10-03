# YIELD

**Original Roblox superhuman action RPG featuring IMPULSE powers, momentum-driven combat, skill-based movement, and server-authoritative systems.**

YIELD is an original open-world superhuman action RPG/sandbox built in **Roblox Studio with Luau**. The project focuses on satisfying movement, momentum-driven combat, personalized abilities, progression, environmental destruction, PvE/PvP, bosses, and dynamic world events.

At the center of the game is **IMPULSE**, an original power system designed around player expression rather than predefined superhero classes.


<p align="center">
  <img src="https://github.com/user-attachments/assets/72346709-b625-4680-bd90-5b4efd11b6f3" width="720" alt="YIELD game vision concept">
</p>

<p align="center">
  <em>Concept visualization of YIELD's target gameplay experience. Not representative of current gameplay footage.</em>
</p>

<p align="center">
  <strong>Movement • Combat • IMPULSE • Personalized Powers • Open World</strong>
</p>

---

## Core Design

YIELD is designed around the idea that **superhuman abilities should feel personal**.

Rather than selecting predefined superheroes, players develop their own abilities through an original energy system called **IMPULSE**.

Combat connects character attributes, velocity, IMPULSE Output, technique, and ability modifiers so that movement and combat are mechanically connected.

### Conceptual Impact Model

> **Base Power + Velocity/Momentum + Impulse Output + Technique + Ability Modifiers = Final Impact**

---

## IMPULSE Affinities

| Affinity | Specialization |
|---|---|
| **DRIVE** | Physical enhancement, strength, durability, regeneration and raw power |
| **VECTOR** | Movement, acceleration, momentum, flight and force |
| **PROJECTION** | Blasts, beams, shockwaves and ranged energy |
| **FORGE** | Armor, weapons, constructs, barriers and drones |
| **ALTERATION** | Changing properties of energy or the body |
| **ANOMALY** | Unconventional abilities that don't naturally fit another Affinity |

An Affinity defines a player's natural relationship with IMPULSE — **not a predefined character class**.

Two VECTOR users should not necessarily play anything alike.

---

## Development Status

> **YIELD is currently in active prototype development.**

The current development focus is establishing the game's core technical foundation before expanding into the full open world.

### Implemented / Prototyped

- Smooth acceleration and sprint movement
- Directional Impulse Burst
- High-speed traversal
- IMPULSE resource management
- Dynamic camera/FOV feedback
- Four-hit server-authoritative combat combo
- Heavy attacks
- Blocking and dodging
- Hit stun, knockback and launch behavior
- Momentum-influenced combat
- Server-authoritative contact timing
- Input buffering and contextual attack architecture
- R15 combat animation pipeline
- Locomotion/combat animation blending
- Hit confirmation, hitstop, impact VFX and camera feedback
- Special-move animation authoring pipeline

---

## Current Development

Foundation combat and movement systems are operational.

Development is now expanding into **contextual attacks and individually authored special techniques**, including movement-dependent attacks and superhuman abilities.

The current animation workflow uses a dedicated **YIELD Move Lab**, where special techniques are authored and validated individually before being integrated into the production game.

---

## Technical Philosophy

YIELD uses a modular **client/server architecture**.

Gameplay-critical systems such as combat validation, damage, character state, progression and anti-exploit validation are designed to remain **server authoritative**.

The client primarily handles responsive systems such as:

**Input • Camera • UI • Animation Presentation • Local VFX**

This prevents the client from being trusted with critical gameplay outcomes such as damage or successful hits.

---

## Development Approach

YIELD is independently designed and developed by **JT Ketelsen** using Roblox Studio and Luau.

Development uses AI-assisted engineering workflows for architecture analysis, code review, debugging, documentation, animation iteration, and development support. Final design decisions, implementation direction, testing, validation, and project management remain developer-directed.

Development follows an incremental process:

**Design → Implement → Test → Diagnose → Refine → Validate → Document**

---

## Project Vision

YIELD's long-term goal is an open-world environment where superhuman movement, combat, abilities, progression, destruction, NPC encounters, bosses, PvP, and dynamic world events operate as parts of the same interconnected sandbox.

The objective isn't simply to become stronger.

**It's to become your own superhuman.**

---

### Project Status

**Early Development / Prototype**

Built with **Roblox Studio • Luau • Git/GitHub**

© 2026 JT Ketelsen. YIELD and its original concepts are proprietary. All rights reserved.
