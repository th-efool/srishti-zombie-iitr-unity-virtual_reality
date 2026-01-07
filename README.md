## Zombies IITR — VR Survival Experience
![Status](https://img.shields.io/badge/status-prototype-yellow.svg)
![Engine](https://img.shields.io/badge/engine-Unity-black.svg)
![XR](https://img.shields.io/badge/XR-VR%20%7C%20XR%20Toolkit-blueviolet.svg)
![Platform](https://img.shields.io/badge/platform-PC%20VR-lightgrey.svg)
![Event](https://img.shields.io/badge/showcased%20at-Srishti%20IITR-orange.svg)
![Domain](https://img.shields.io/badge/domain-VR%20Game%20Development-blue.svg)
![Architecture](https://img.shields.io/badge/architecture-modular%20gameplay-informational.svg)


Developed for **Srishti 2025 — the Annual Technical Exhibition of IIT Roorkee**, this project was showcased under the **Tinkering Lab – Metaverse section**, representing an immersive intersection of virtual reality, game systems, and spatial storytelling.

**Srishti**, established in **1958**, has long served as a platform for practical innovation and creative engineering at IIT Roorkee. Our project aligns with this legacy by transforming a historically significant real-world structure into an interactive virtual environment.

<table>
  <tr>
    <td colspan="2" align="center">
      <img src="https://github.com/th-efool/srishti-zombie-iitr-unity-virtual_reality/blob/main/Packages/screenshot20260107231347.png" width="720">
    </td>
  </tr>

  <tr>
    <td align="center">
      <img src="https://github.com/th-efool/srishti-zombie-iitr-unity-virtual_reality/blob/main/Packages/screenshot20260107231536.png" width="350">
    </td>
    <td align="center">
      <img src="https://github.com/th-efool/srishti-zombie-iitr-unity-virtual_reality/blob/main/Packages/screenshot20260107231304.png" width="350">
    </td>
  </tr>
</table>

### Concept

We built a **VR zombie survival game** set inside a **digitally reconstructed version of IIT Roorkee’s iconic James Thomson Building**. The objective was not merely to create a game, but to demonstrate how **virtual reality can reimagine familiar physical spaces**, enabling experiential learning, exploration, and engagement through interactive systems.

Players navigate the James Thomson Building in VR, encountering hostile AI entities, managing limited resources, and making real-time survival decisions. The environment retains the architectural identity of the original structure while introducing gameplay-driven tension and immersion.


### Technical Focus

* Full **VR locomotion and interaction**
* Real-time **AI-driven enemies**
* Spatially accurate level design inspired by a real campus landmark
* Emphasis on immersion, presence, and player agency

### Purpose

The project demonstrates how **metaverse-style experiences** can extend beyond abstract worlds into **contextual, location-based virtual spaces**, blending gaming, architecture, and emerging XR technologies.

By situating gameplay within a recognizable IIT Roorkee landmark, we aimed to bridge **technical creativity with institutional identity**, making the experience both engaging and meaningful for exhibition visitors.


<table>

  <tr>
    <td align="center">
      <img src="https://github.com/th-efool/srishti-zombie-iitr-unity-virtual_reality/blob/main/Packages/screenshot20260107231218.png" width="350">
    </td>
    <td align="center">
      <img src="https://github.com/th-efool/srishti-zombie-iitr-unity-virtual_reality/blob/main/Packages/screenshot20260107231206.png" width="350">
    </td>
  </tr>

  <tr>
    <td colspan="2" align="center">
      <img src="https://github.com/th-efool/srishti-zombie-iitr-unity-virtual_reality/blob/main/Packages/screenshot20260107231004.png" width="720">
    </td>
  </tr>
</table>
----

## VR Interaction System

### Custom VR Hands & Input Mapping

The game uses **custom animated VR hands** driven directly by controller input via Unity’s **Input System**.

* Pinch and grip inputs are mapped using `InputActionProperty`
* Input values are streamed every frame to an Animator
* Hand animations respond continuously to trigger pressure, enabling natural interaction

**Key characteristics**

* No hardcoded gestures
* Fully analog input → animation blending
* Compatible with XR Interaction Toolkit bindings

This provides responsive, low-latency hand feedback essential for immersion.

---

## Combat & Skill System
I’ll treat this as a **README content architecture + missing-system documentation task**, not a rewrite.
You already have strong technical sections; what’s missing is **formalizing the remaining systems so the README feels complete and intentional**.

Below are **clean, drop-in sections** that match the depth and tone of what you’ve already written.

---

## Weapon System

### Modular Weapon Architecture

The game implements a **modular weapon system** designed to support multiple firearms with distinct behaviors, stats, and UI bindings.

Each weapon encapsulates:

* Fire rate
* Damage per hit
* Magazine size
* Reload time
* Projectile behavior (hitscan vs physical projectile)

Weapons are treated as **data-driven entities**, allowing easy addition or balancing without rewriting combat logic.

---

### Ammo & Inventory Management

Ammo is managed through a **centralized inventory system** rather than per-weapon isolation.

**Core mechanics**

* Each weapon pulls ammo from a shared inventory pool
* Magazine ammo and reserve ammo are tracked separately
* Inventory values update UI in real time

This allows:

* Weapon switching without ammo duplication
* Global ammo rewards or penalties
* Future expansion into pickups, upgrades, or economy systems

---

### Advanced Reloading Logic

Reloading is implemented as a **stateful process**, not a simple ammo refill.

**Reload pipeline**

1. Reload input detected
2. Reload state entered (weapon disabled)
3. Reload animation / delay plays
4. Ammo transferred from inventory → magazine
5. Weapon re-enabled

**Key constraints**

* Reload is blocked if inventory ammo is insufficient
* Reload cannot be interrupted by firing
* Prevents reload spamming via cooldown/state lock

This mirrors real-world firearm logic while maintaining gameplay balance.

---

## User Interface (UI) System

### In-Game Combat UI

The game features a **diegetic-style HUD** optimized for VR readability and minimal obstruction.

Displayed information includes:

* Current wave number
* Active weapon name
* Magazine ammo / reserve ammo
* Weapon damage stats
* Player health bar

UI elements are positioned to remain **within the player’s natural field of view** without requiring head tilting or excessive eye movement.

---

### Weapon Comparison UI

When multiple weapons are available, the UI supports **side-by-side weapon stat visualization**, allowing players to make quick tactical decisions.

Shown attributes include:

* Ammo capacity
* Damage per shot
* Inventory availability

This reinforces **skill-based decision making** rather than hidden stats.


### Skill-Based Damage Model

Combat is implemented using a **skill-driven damage system**, allowing both projectile and non-projectile attacks.

* Skills are spawned via controller input
* Each skill carries its own damage metadata
* Damage is applied on collision or particle hit

This decouples **input, visuals, movement, and damage logic**, making the system extensible.

---

### Skill Spawning (`SkillSpawner`)

* Uses XR input actions for button-based activation
* Spawns skill prefabs at a defined controller transform
* Supports cooldown-based spawning to prevent input spamming
* Can optionally spawn skills as projectiles

Skills are destroyed automatically after a configurable lifetime to ensure scene cleanliness.

---

### Projectile System

Projectile-based skills use rigidbody-driven motion:

* Direction is calculated at spawn time
* Velocity is applied directly for deterministic movement
* Lifetime-based destruction prevents runaway physics objects

This ensures predictable behavior in VR while maintaining performance.

---

### Enemy Health & Damage Handling

Enemies use a **cooldown-gated health system**:

* Supports particle-based and collider-based hits
* Prevents rapid damage stacking via internal cooldowns
* Optional VFX can be triggered on death
* Enemies self-destruct cleanly once health reaches zero

This avoids unintended burst damage while keeping combat readable.

---

### UI Design Considerations for VR

* Large, high-contrast typography for readability
* Minimal animation to avoid motion discomfort
* Transparent backgrounds to preserve environmental awareness
* UI updates are event-driven (not per-frame polling)

This ensures comfort during extended VR sessions.

---

## Death & Leaderboard System

### Player Death Handling

On player death:

* Input is disabled
* Active combat systems are halted
* Enemy AI is frozen or despawned
* A results overlay is triggered

This cleanly transitions the experience from **active gameplay to post-run evaluation**.

---

### Leaderboard System

The game includes a **session-based leaderboard** displayed immediately after player death.

Tracked metrics include:

* Highest wave reached
* Total enemies eliminated
* Survival duration

The leaderboard provides:

* Immediate performance feedback
* Replay motivation
* Competitive comparison during exhibitions

This was especially effective in the **Srishti IITR showcase setting**, encouraging repeated attempts by visitors.

## Design Rationale

* **Direct input → animation → gameplay loop** minimizes latency
* Modular scripts allow easy tuning of damage, cooldowns, and speed
* Systems are intentionally lightweight to maintain VR performance

Together, these systems enable **responsive hand-driven combat**, reinforcing immersion while remaining scalable for future mechanics.

## AI System

### 1. Wave System Architecture

The game uses a **data-driven wave spawning system** that allows controlled escalation of difficulty and enemy variety over time.

#### `WaveSpawner` (Timed Sequential Spawning)

* Manages **time-based spawning** using a countdown and coroutine.
* Each wave contains:

  * An ordered list of enemy prefabs
  * A configurable delay between consecutive spawns
* Enemies are instantiated as children of a fixed spawn point, ensuring spatial consistency.

**Key mechanics**

* Uses `Time.deltaTime` for frame-independent countdown.
* Uses `IEnumerator` + `WaitForSeconds` for non-blocking sequential spawning.
* Designed for simple, linear wave progression.

This system is suited for **scripted or tutorial-style encounters**.

---

### 2. Advanced Wave Controller (`WaveMaster`)

For large-scale encounters, the game uses a more flexible **wave composition system**.

#### `WaveMaster`

* Each wave is defined as a **count vector** (`ZombieWave`), mapping:

  ```
  zombie type → number to spawn
  ```
* Supports multiple zombie archetypes via indexed prefabs.
* Spawns enemies across **randomized spawn points with spatial offsets**, preventing clustering and predictability.

**Design advantages**

* Wave difficulty scales by composition, not just count.
* Easy balancing through inspector-editable arrays.
* Designed to support future scaling via animation curves (health, strength, speed multipliers).

This system enables **arena-style survival gameplay** with controlled randomness.

---

### 3. Zombie AI State System

Each zombie operates using a **state-driven AI loop** built on Unity’s `NavMeshAgent`.

#### Detection & State Evaluation

* Uses physics sphere checks to determine:

  * Player visibility (`visionRadius`)
  * Attack range (`attackingRadius`)
* States transition implicitly based on spatial conditions:

  * **Patrolling**
  * **Alert / Screaming**
  * **Chasing**
  * **Attacking**
  * **Dead**

No explicit FSM class is used; behavior emerges from ordered condition checks.

---

### 4. Navigation & Movement

* Navigation is handled via Unity **NavMeshAgent**.
* Movement speed dynamically switches between:

  * Walking (patrol)
  * Running (chase)
* Rotation is manually smoothed using `Quaternion.Slerp` to avoid animation snapping.

Animator root motion is intentionally disabled to maintain deterministic navigation.

---

### 5. Combat & Interaction

* Attacks are proximity-based and validated using:

  * Minimum distance checks
  * Forward raycasts from an attack point
* Supports **multiple attack animations** selected randomly.
* Damage is applied through a centralized `PlayerHealth` interface.

Cooldowns are enforced to prevent animation or damage spam.

---

### 6. Animation System Integration

* Uses a blend-tree-driven animator controller.
* Parameters such as:

  * `Speed`
  * `IsAttacking`
  * `IsScreaming`
  * `AttackType`
  * `DeathType`
    drive transitions.
* Death animations are randomized for variation and visual realism.

---

### 7. Health & Death Handling

* Zombies maintain internal health state.
* On death:

  * Navigation is disabled
  * Detection radii are cleared
  * Death animation is triggered
  * Object is destroyed after a delay to preserve scene performance

This ensures **clean teardown** without leaving active AI logic behind.

---

### 8. Stability Considerations

* Floor collision re-enables the NavMeshAgent to recover from spawn or physics glitches.
* Defensive checks prevent logic execution after death.
* Coroutines and invokes are isolated to avoid race conditions.
