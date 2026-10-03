# YIELD — Development Log

This document tracks the technical evolution of **YIELD**, from its initial movement and combat prototypes through the current development of contextual attacks and individually authored superhuman techniques.

YIELD is being developed incrementally: establish a working foundation, test it, identify architectural weaknesses, stabilize those systems, and only then expand the game's scope.

> **Current Stage:** Foundation systems established — contextual combat and special-move development in progress.

---

## Development Overview

| Phase | Focus | Status |
|---|---|---|
| Initial Prototype | IMPULSE movement and basic combat | Complete |
| Foundation 1 | System gating and stabilization | Complete |
| Foundation 2 | Client initialization and HUD ownership | Complete |
| Foundation 3A | Server combat hardening | Complete |
| Foundation 3B | Combat acceptance protocol | Complete |
| Foundation 4 | Four-hit Foundation combo | Complete |
| Foundation 5A | Animation runtime architecture | Complete |
| Foundation 5B | Combat animation development | Complete |
| Foundation 5B.1 | Combat grammar and semantic input | Complete |
| Foundation 5B.2 | Full-body combat motion set | Complete |
| Foundation 5B.3 | Runtime animation integration | Complete |
| Foundation 5B.3.1 | Locomotion/combat blending | Complete |
| Foundation 5C | Authoritative contact timing | Complete |
| Foundation 5D | Impact presentation | Complete |
| Foundation 5D.1 | Combat audio architecture | Architecture Complete / Assets Deferred |
| Contextual Combat | Movement-dependent attacks | In Development |
| Move Lab | Individually authored special techniques | In Development |

---

# Initial Prototype

YIELD began by establishing the game's most important design pillar: **superhuman movement should be enjoyable before the player ever enters combat**.

The first playable prototype established:

```text
IMPULSE Resource
      +
Smooth Acceleration
      +
Sprinting
      +
Directional Burst
      +
High-Speed Dash
      +
Camera Feedback
      +
Movement VFX
```

Early movement work included acceleration/deceleration, sprinting, directional Impulse Burst, sustained high-speed traversal, IMPULSE consumption/regeneration, dynamic FOV, speed trails, afterimages, dash particles, and ground-impact presentation.

A basic combat prototype was developed alongside movement.

This proved that the core concept was playable, but it also revealed that YIELD needed a stronger technical foundation before additional abilities, progression, NPCs, destruction, or world systems were added.

---

# Foundation 1 — System Stabilization

The first Foundation pass focused on controlling initialization and preventing unfinished systems from interfering with development.

Deferred systems were gated while the essential movement, combat, HUD, and player-state systems remained active.

This established an important development rule for YIELD:

> **Incomplete future systems should not destabilize the systems currently being tested.**

Rather than expanding the prototype immediately, development shifted toward making the existing foundation predictable.

**Result:** Stable base for continued engineering.

---

# Foundation 2 — Client Initialization & HUD Ownership

The next phase addressed client lifecycle and interface ownership.

The HUD and client controllers were reorganized so the runtime interface referenced the actual active player controllers rather than duplicate or disconnected instances.

Initialization became safer across character spawning and respawning.

Camera handling was also updated to resolve the current Roblox camera dynamically.

The goal was not to add visible gameplay features, but to ensure that the client architecture remained stable as the character lifecycle changed.

**Result:** More reliable client initialization, HUD ownership, and respawn behavior.

---

# Foundation 3A — Server Combat Hardening

Combat was then reviewed from the perspective of **server authority and exploit resistance**.

The server became responsible for validating combat requests and maintaining authoritative combat state.

This included attack recovery, combo state, dodge timing, invulnerability windows, blocking, stun behavior, character lifecycle handling, and bounded momentum contribution.

Clients were prevented from supplying authoritative combat values such as damage.

Conceptually:

```text
CLIENT
"I want to attack."
        │
        ▼
SERVER
"Is this request valid?"
        │
        ▼
SERVER
"What attack is actually allowed?"
        │
        ▼
SERVER
"What happens as a result?"
```

**Result:** Combat requests became intent rather than authority.

---

# Foundation 3B — Combat Acceptance Protocol

Once the server owned attack validation, the client needed a reliable way to know which attacks the server had actually accepted.

A server-to-client combat presentation protocol was introduced.

Accepted attacks receive a server-generated **AttackId** and presentation state.

Conceptually:

```text
LightAttack Request
       ↓
Server Validation
       ↓
Attack Accepted
       ↓
AttackId Assigned
       ↓
Approved Attack State
       ↓
Client Presentation
```

Rejected requests do not generate an accepted attack state.

Attack IDs also provide a mechanism for rejecting stale presentation information.

**Result:** Client combat presentation became tied to server-approved attacks.

---

# Foundation 4 — Four-Hit Combat Sequence

The Foundation light combo was expanded into a four-hit sequence.

```text
Light 1
   ↓
Light 2
   ↓
Light 3
   ↓
Light 4
   ↓
Reset
```

The sequence introduced progressively stronger impact characteristics while remaining controlled by server-side combo state.

Heavy attacks remained a separate higher-commitment action.

This combo was intentionally treated as the **Foundation combat language**, not the final form of YIELD's combat.

The long-term objective remained contextual combat where movement and player technique influence which attack occurs.

**Result:** Stable four-hit baseline combat sequence.

---

# Foundation 5A — Animation Runtime

With authoritative combat established, development moved into animation.

A modular animation runtime was introduced with dedicated combat animation slots:

```text
Light1
Light2
Light3
Light4
Heavy
```

Server-approved attacks could now map into client presentation without allowing animation state to determine authoritative combat outcomes.

Animations were configured as non-looping Action-priority tracks.

**Result:** Combat logic and combat animation became connected without merging their responsibilities.

---

# Foundation 5B — Combat Animation Prototyping

The first combat animations were developed as technical prototypes.

These early animations proved that the runtime architecture worked, but visually they were too basic for YIELD's intended combat identity.

Rather than treating the prototypes as finished assets, they became a validation step for the animation pipeline.

This reinforced another project principle:

> **Technical success and visual quality are separate milestones.**

A system can function correctly while still requiring substantial artistic iteration.

---

# Foundation 5B.1 — Combat Grammar & Semantic Input

Combat input was reorganized around **semantic actions** rather than hard-coded physical controls.

Instead of gameplay systems being designed around individual keys or mouse buttons, the architecture began using actions such as:

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

An input buffer was also introduced for combat actions.

A combat grammar was defined around phases such as:

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

This established the vocabulary needed for future chaining, branching, cancel windows, and contextual attacks.

**Result:** Input architecture became more scalable for future abilities and control schemes.

---

# Foundation 5B.2 — Full-Body Combat Motion

The original technical animation prototypes were replaced with a more deliberate full-body Foundation combat set.

The new animations emphasized kinetic chains, torso rotation, guard positioning, footwork, readable contact silhouettes, recovery, and better transitions between attacks.

The four Foundation light attacks developed distinct identities rather than functioning as repeated versions of the same punch.

A dedicated Heavy attack was also authored with greater commitment and follow-through.

**Result:** Foundation combat reached an acceptable visual baseline for continued development.

---

# Foundation 5B.3 — Published Animation Integration

The approved Foundation animations were published as Roblox animation assets and integrated into the runtime animation system.

The complete sequence became:

```text
Semantic Input
      ↓
Server Validation
      ↓
Attack Accepted
      ↓
Combat Presentation
      ↓
Animation Slot
      ↓
AnimationController
      ↓
Published R15 Animation
```

Runtime testing verified combo sequencing, Heavy attacks, rejected requests, combat-state interruption, respawn behavior, and continued movement operation.

**Result:** Production combat was running on the approved Foundation animation set.

---

# Foundation 5B.3.1 — Locomotion / Combat Blending

Runtime testing exposed a visual problem.

Full-body Action-priority combat animations could overpower the normal locomotion animation while the character continued physically moving.

The result was visible **foot sliding**.

The issue was not character velocity itself. The problem was animation blending.

A velocity-aware presentation system was introduced so combat-animation influence could change based on actual character movement.

Conceptually:

```text
Standing
   ↓
Strong Combat Animation

Walking
   ↓
Combat + Locomotion Blend

Running
   ↓
Greater Locomotion Influence

High-Speed Movement
   ↓
Locomotion Remains Readable
```

This preserved attack readability while allowing the character's legs to continue communicating movement.

**Result:** The Foundation animation set became compatible with YIELD's movement-heavy gameplay.

---

# Foundation 5C — Authoritative Contact Timing

One of the most important combat architecture changes occurred during Foundation 5C.

Previously, successful attack resolution occurred too close to attack acceptance.

This meant gameplay damage could occur before the animation visually reached contact.

The system was redesigned around **authoritative contact timing**.

```text
Attack Request
      ↓
Server Validation
      ↓
Attack Accepted
      ↓
Animation Begins
      ↓
Contact Delay
      ↓
Server Revalidation
      ↓
Current-Position Hit Detection
      ↓
Damage / Knockback / Stun
```

Foundation contact timings became:

| Attack | Contact Time |
|---|---:|
| Light 1 | 0.10 s |
| Light 2 | 0.13 s |
| Light 3 | 0.15 s |
| Light 4 | 0.19 s |
| Heavy | 0.29 s |

At contact, the server re-evaluates relevant character and combat state rather than assuming that conditions at attack acceptance are still valid.

Hit detection also evaluates current positions at contact.

This allows situations such as a target entering or leaving attack range during the animation to resolve more naturally.

**Result:** Visual contact and authoritative gameplay contact became meaningfully synchronized.

---

# Foundation 5D — Impact Feedback

Once authoritative contact timing existed, impact presentation could be connected to confirmed hits rather than button presses.

A server-confirmed `HitConfirmed` presentation path was introduced.

```text
Authoritative Contact
       ↓
Valid Hit
       ↓
HitConfirmed
       ↓
Client Presentation
       ├── Impact VFX
       ├── Hitstop
       └── Camera Feedback
```

Impact intensity can vary according to attack tier and bounded momentum contribution.

Whiffed attacks do not receive the same confirmed hit presentation.

This was important because YIELD's combat feedback should communicate **actual gameplay outcomes**, not merely player input.

**Result:** Attacks began to feel more physically connected to confirmed contact.

---

# Foundation 5D.1 — Audio Architecture

Combat audio architecture was prepared after impact presentation.

The system supports different impact categories and defensive outcomes, but audible assets were intentionally deferred.

This was a deliberate scope decision.

Audio infrastructure should not block development of movement, combat, abilities, animation, NPCs, or progression.

The architecture remains available for approved audio assets later.

**Result:** Audio system prepared; asset implementation intentionally deferred.

---

# Contextual Combat

With Foundation combat operational, development began shifting toward one of YIELD's defining goals:

> **Movement and combat should influence each other.**

A permanent four-hit ground combo alone would not satisfy that goal.

The contextual attack architecture is intended to eventually support states such as:

```text
GroundLight
SprintLight
BurstLight
AirLight
Launcher
DownwardAttack
MomentumHeavy
```

The same combat input can therefore produce different techniques depending on how the player is moving and fighting.

This is the beginning of YIELD moving away from a conventional Roblox combat loop and toward a more expressive superhuman combat system.

---

# SprintLight — First Contextual Attack Experiment

`SprintLight` became the first dedicated contextual attack candidate.

The technique was designed as a forward-driving strike performed while sprinting, with animation characteristics built specifically around maintaining movement.

The animation itself validated successfully inside Studio.

However, the first publishing workflow exposed a separate problem: the locally valid KeyframeSequence produced invalid cloud animation assets.

Investigation verified the local animation structure, timing, R15 hierarchy, contact marker, and runtime playback.

The problem appeared to be associated with the reconstructed Animation Editor save/export workflow rather than the authored motion itself.

Instead of repeatedly publishing potentially invalid assets, development changed direction.

**Result:** The animation problem led directly to a safer dedicated authoring workflow.

---

# YIELD Move Lab

Special-move animation development has now been separated from the production YIELD place.

A blank Roblox Studio environment is being used as a dedicated **YIELD Move Lab**.

The workflow is:

```text
Technique Design
      ↓
Clean R15 Authoring Rig
      ↓
Native Animation Editor
      ↓
Author Motion
      ↓
Contact Marker
      ↓
Visual Review
      ↓
Native Save
      ↓
Reopen / Validate
      ↓
Publish
      ↓
Production Integration
```

This keeps experimental animation work isolated from the working game and provides a cleaner path for producing native Roblox animation assets.

Each special technique can now be developed individually before being introduced into YIELD's production combat architecture.

---

# Special-Move Development

The first dedicated Move Lab special technique is:

## Soaring Uppercut

The Soaring Uppercut is designed as an explosive rising strike that transitions from grounded compression into a superhuman upward attack.

The animation is intended to communicate:

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

Actual vertical character movement will eventually be handled by YIELD's gameplay physics rather than baked directly into animation root translation.

This maintains separation between:

```text
ANIMATION
Visual motion and technique presentation

GAMEPLAY
Movement, launch force, hit detection and combat outcome
```

The Soaring Uppercut represents the beginning of YIELD's individually authored special-technique library.

**Status:** In development.

---

# Current Development Direction

YIELD's core Foundation now connects:

```text
Movement
    +
Server Authority
    +
Combat
    +
Animation
    +
Semantic Input
    +
Contact Timing
    +
Impact Presentation
    ↓
Contextual Superhuman Combat
```

The next development stage expands this foundation through contextual attacks and special techniques before larger systems are layered on top.

Future systems include IMPULSE Output, personalized Affinity techniques, NPC combat, progression, environmental destruction, bosses, PvP systems, dynamic world events, and the larger open world.

---

# Development Philosophy

YIELD is intentionally being built **from the core outward**.

The project does not need a massive world filled with content if moving and fighting inside that world are not already enjoyable.

The development priority is therefore:

> **Make movement feel good. Make combat feel good. Make powers feel personal. Then build the world that gives those systems meaning.**

---

## Current Project Stage

**Prototype / Foundation Development**

Core movement and combat architecture are operational.

Contextual combat and individually authored special techniques are currently in development.

---

[← Technical Architecture](ARCHITECTURE.md)  
[← YIELD Overview](../README.md)
