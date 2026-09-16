# PixelDot2D Core Framework Feature List
What’s Inside PixelDot2D Core Framework

**Available on the:** [Unity Asset Store](https://assetstore.unity.com/packages/tools/utilities/pixeldot2d-core-framework-370674)

> [!NOTE]
> **Living Architecture Notice:** This feature list serves as a high-level technical overview of the framework's primary infrastructure. PixelDot2D is under continuous, active development, with new subsystems, optimizations, and utility modules being integrated regularly. To prevent this document from expanding into an exhaustive API manual, we have focused exclusively on the most critical engineering milestones and performance metrics per sub-library. The framework contains substantial internal utility suites beyond what is outlined below.


## Table Of Contents

- [Core Engine Layer](#core-engine-layer-the-autonomous-foundation)
- [Combat Sub-Library](#Combat-Sub-Library-The-Universal-2D-Execution-Engine)
- [Modular Character Sub-Library](#modular-character-Sub-Library-Full-Runtime-Reconfigurability)
- [Items Sub-Library](#items-Sub-Library)
- [Platformer Sub-Library](#Platformer-Sub-Library)

---

## Core Engine Layer (The Autonomous Foundation)

An isolated, self-sustaining foundation housed within its own explicit Assembly Definition (`.asmdef`). Core is engineered to drop seamlessly into any project as a zero-dependency architecture. While it serves as the essential backbone for all downstream sub-libraries, it is entirely opt-in—allowing developers to leverage individual utility suites independently without installing or coupling the broader framework. The engine streamlines complex, high-frequency Unity engine overhead into high-performance operations without introducing project clutter.

### Core Key Technical Features:

- **Asynchronous Binary Serialization:** A robust, zero-bloat binary save/load engine featuring native Unity Object tracking that writes payloads to disk on a separate worker thread to completely eliminate runtime frame hiccups. Designed for friction-free integration, developers can map serializable data targets directly via Inspector drop zones without refactoring class structures. System registration requires only a single interface implementation and a local enum assignment; the underlying architecture handles the rest.
- **Data-Driven Mover Orchestrator:** Drive and control any `Rigidbody2D` context entirely from the editor utilizing interchangeable `ScriptableObject` modular blocks. Developers can dynamically re-arrange, stack, and chain movement arrays on the fly to craft complex locomotion profiles with zero structural code changes.
- **Decoupled Cross-Device Input Wrapper:** A hardware-agnostic input singleton that abstracts and routes Unity’s New Input System. The wrapper evaluates all active peripheral streams in parallel, allowing seamless, mid-session device hot-swapping without the overhead of nested conditional state machines.
- **Ergonomic Extension Suite:** A comprehensive library of high-utility extension methods designed to eliminate daily boilerplate. Includes highly optimized bit-shifting operations, vector mathematics, deterministic `LookAt2D` calculations, and streamlined component cache operations.
- **Type-Safe Object Pooling:** Highly reusable runtime memory pools powered by F-Bounded Polymorphism (CRTP). This enforces strict compile-time type safety, eliminating human configuration error, casting overhead, and heap fragmentation.
- **Lightweight Sprite Animator:** A streamlined, code-driven sprite animation engine that completely bypasses the heavy memory and evaluation overhead of Unity’s native Mecanim state-machine graphs while supporting advanced sequence-based frame animations out of the box.
- **Polymorphic Sensor Utilities:** Highly optimized, sensor-mimicking `ScriptableObjects` designed to streamline structural environmental diagnostics (including Ground, Wall, Ledge, and Component-interface checks) without requiring codebase rewrites.
- **Low-Overhead Native Collision Matrix:** Bypasses Box2D simulation overhead and C++ marshalling costs by routing operations directly through raw native casting lines, managed by a plain C# orchestration manager. The system guarantees absolute zero runtime garbage collection while perfectly emulating Unity's complete physics lifecycle (`OnTriggerEnter`, `OnTriggerStay`, `OnTriggerExit`) using batched payload buffers to return all frame results simultaneously for highly informed, performant gameplay logic.
  
- **Inversion-Driven Line of Sight System:** A high-performance line-of-sight architecture that completely centralizes spatial visibility checking and permanently eliminates manual physics boilerplate across your projects. Implemented via a polymorphic factory pattern, the architecture decouples execution from native game loops using a standardized 3-tier triad (Data, ScriptableObject, and State components), allowing developers to add or swap custom visibility shapes seamlessly without modifying host code. To maximize runtime performance, the execution pipeline utilizes an inverted logic model: rather than querying dynamic target entities, it isolates evaluation strictly to environmental obstacle masks and maps queries to a fixed, single-slot results buffer. This inversion enables developers to reuse a single ScriptableObject asset across completely unique entities even as their individual Player, Enemy, or NPC targets change, since the core obstacle layers (such as walls, floors, and doors) remain uniform across all actors. This architectural shift allows the underlying physics engine to instantly short-circuit, terminating processing calculations the exact moment a single piece of cover geometry is encountered. The entire system is engineered for zero-garbage runtime execution, caches squared distance bounds at initialization to completely bypass expensive distance square-root operations, and features a comprehensively formatted header document detailing step-by-step implementation, usage, the underlying architectural reasoning, and extension guidelines.

---

## Combat Sub-Library (The Universal 2D Execution Engine)

Weapon systems are frequently a bloated labyrinth of deeply nested GameObjects, complex hierarchy configurations, and fragile native physics constraints. PixelDot2D fundamentally restructures this paradigm by treating combat as pure mathematical data, granting developers absolute authority over execution windows without any hierarchy overhead. The result is a highly comprehensive, genre-agnostic engine engineered to handle any 2D combat requirement with deterministic accuracy.

### Combat Key Technical Features:

- **100% Virtual Architecture:** Weapons exist strictly as logic-only, plain C# classes evaluated via manual update pumps. This keeps your hierarchy pristine and drops CPU overhead to near-zero margins.
- **Interface-Driven Pipeline:** The combat subsystem is entirely decoupled from core framework code; any custom entity can seamlessly deploy, register, and utilize this system by implementing a single, lightweight interface.
- **Live Visual Debugging:** Includes integrated Editor Gizmos that render weapon range boundaries, sweeping attack arcs, and active hitboxes in real-time inside the Unity Scene View for instantaneous visual feedback during gameplay tuning.
- **Custom Colliderless Projectiles:** Hyper-optimized to squeeze out maximum hardware performance when coordinating 1,000+ active objects simultaneously. Easily author advanced projectile behaviors without the memory footprint of Box2D or the architectural complexity of Unity's ECS.
- **Independent Trigger Lifecycles:** Recreates a complete `OnTriggerEnter`, `OnTriggerStay`, and `OnTriggerExit` simulation matrix completely outside of Unity's native physics loop, supplying your custom gameplay systems with all required lifecycle hooks via a lightweight, high-speed data pipeline.
- **Frame-Driven Combat Synchronization:** Unify raw entity locomotion, offensive actions, and active animation sequences frame-by-frame utilizing a clean, centralized `ScriptableObject` workflow. Lock execution triggers to strict, frame-perfect timing windows, or leave constraint frames empty to permit unconstrained, rapid-fire multi-hit offensive chains.
  
- **Lean Weapon Management API:** A streamlined, high-level API enabling entities to safely equip, register, and stack multiple distinct weapon profiles simultaneously with zero structural codebase modifications.
- **Runtime Component Swapping:** Gain granular, atomic control over the core weaponized module's internal configuration, allowing you to hot-swap individual mechanical weapon components, firing rules, and execution logic on the fly at runtime.

---

## Modular Character Sub-Library (Full Runtime Reconfigurability)

Engineered explicitly for entities requiring real-time architectural evolution, dynamic status mutation, and total runtime restructuring. The framework enforces atomic control over individual isolated behaviors—enabling developers to seamlessly inject, hot-swap, or strip character logic on the fly across any genre utilizing a standard 2D physics plane, including side-scrollers, 2.5D hybrids, top-down shooters, space simulators, and more.

To ensure absolute stability during complex, multi-state reconfigurations, all state transitions and behavior mutations are deferred through an isolated Queue System that completely neutralizes race conditions and volatile null-reference exceptions. This decoupled architecture enables absolute, zero-code-change extensibility: entirely new operational states can be introduced and integrated into existing character behaviors without altering a single line of pre-existing code inside the master orchestrator. By leveraging the data-driven input pipeline and automated flyweight factory, developers can map complex transitions into newly authored states directly from the Unity Inspector using pre-built blueprint templates engineered for any movement profile a native `Rigidbody2D` can execute.

### Modular Character Key Technical Features:

- **Plain C# Architecture:** Evaluates via centralized manual update pumps completely outside of traditional `MonoBehaviour` hierarchy overhead to guarantee absolute zero runtime garbage collection spikes.
- **Frictionless Extensibility:** Inject entirely new structural logic seamlessly using built-in Flyweight and Factory design patterns without ever touching or modifying the core controller codebase.
- **Scripted Composition States:** Mix, match, and orchestrate complex entity state lifecycles directly inside the editor utilizing interchangeable, data-driven `ScriptableObject` configurations.
- **Intelligent Removal & Fallback:** Safely strip operational states or individual behaviors at runtime with automated fallback safeguards that smoothly revert the entity back to a default Master Blueprint state.
- **Interface-Driven Pipeline:** Requirements are restricted to a single, thin interface (`IModularCharacter`), completely eliminating the restrictive constraints and engineering clutter of forced class inheritance.
- **Virtual Lazy State Pooling:** Advanced internal memory management that dynamically tracks usage metrics, keeping only actively required states allocated while recycling dormant instances to optimize CPU cache locality.
- **Modular "Lego-Style" Passives:** Deconstruct passive mechanics into interchangeable `ScriptableObject` functional cogs, allowing you to assemble intricate gameplay synergies with zero manual script modifications.
  
- **Infinite Entity Reusability:** Deploy one unified, universal controller profile to drive player characters, hostile enemies, AI companions, or any arbitrary 2D entity in your project layout.

---

### Items Sub-Library

Built as an optional, high-performance extension package for entities requiring robust item, storage, and equipment lifecycles. 

### Assembly & Dependency Structure

- **Decoupled Architecture:** Built directly on top of the `ModularCharacter` sub-library, ensuring that while it enhances character capabilities, the core character framework remains completely independent.
- **Optional Integration:** Isolated entirely within its own Assembly Definition (`.asmdef`).
- **Safe Deletion Safeguards:** True to the framework's strict rule of open extensibility, the entire sub-library folder can be safely deleted from your project structure without breaking or throwing compilation errors in any pre-existing `ModularCharacter` setups.

### Core Sub-System Components

- **ItemLibrary:** Acts as the centralized, optimized database and lookup registry for item asset definitions.
- **InventoryManager:** A pure, standalone plain C# engine completely decoupled from Unity lifecycles, character containers, or specific UI view logic. It can be universally deployed across players, AI agents, NPCs, world stashes, or loot containers.
- **CharacterInventory:** A specialized, character-bound wrapper component that orchestrates an `InventoryManager` alongside an `EquipmentManager` to cleanly process gear loadouts, provide inventory access, and apply equipment data modifications to any active `ModularCharacter`.

### Advanced Transaction Engine and UI Safety

- **UI-Safe Mutation Architecture:** Internal slot items never swap raw memory references. Instead, slots swap their internal structural data definitions, ensuring external UI elements and view caches remain perfectly valid, tracked, and completely safe from stale data bugs.
- **Comprehensive Transaction Suite:** Built-in native support for high-utility storage methods out of the box:
    - *Swap Slots:* Seamless intra-container item repositioning.
    - *Inter-Inventory Swap:* Smooth, cross-container slot-to-slot transfers (e.g., Player to Bank).
    - *Take All:* Automated sequential bulk allocation from external stashes or loot drops.
    - *Quick Stack:* Smart, multi-stack aggregation that consolidates partial piles left-to-right into existing layout groupings without creating visual clutter or fragmented slot footprints.
- **Duplication Exploit Protection:** Packed with robust internal validation routines, unique guard clauses, and diverse method overloads to ensure every transaction is completely guarded against item duplication exploits, allowing developers to safely drive inventory operations through a unified, high-level API.

### Composition-Based Item Design (Open/Closed Principle)

- **Zero Rigid Logic:** The core item class acts strictly as an empty structural container that performs no hardcoded gameplay calculations, protecting your codebase from architectural bloating.
- **Frictionless Extension:** To create entirely new item behaviors, developers simply inherit from the base abstract component class. New scripts can be dropped directly into an item asset configuration within the Unity Inspector without ever modifying the core item source files.
- **Requirement Validation:** Restrictive validation rules can be plugged into any asset configuration to safely verify baseline character attributes or active status traits before allowing an item to be used or equipped.
- **Infinite Item Variations:** By mixing, matching, and stacking modular data-driven pieces, you can orchestrate limitless item combinations out of the box, including:
    - *Consumables:* Simple health-restoration or mana-restoration potions.
    - *Stat Buffs:* Flasks that grant temporary attribute multipliers for a specified duration.
    - *Character Passives:* Equipment assets that directly alter character properties and status states.
    - *Ability Unlocks:* Complex artifacts that inject entirely new capabilities into a `ModularCharacter` (such as dashing, wall-climbing, or extra air-jumps).

> [!NOTE]
> **Combat Cross-Library Integration:** If developers choose to merge the Combat and Modular Character sub-libraries, they can leverage an identical architectural pipeline. The combat system's `WeaponManager` reads a ScriptableObject blueprint (`SO_MultiWeaponizedModule_BluePrint`) and dynamically alters the internal weapon structure to match. Because of this shared design pattern, items can completely transform weapon behaviors on the fly, just as they inject character abilities like hovering, dashing, or wall-climbing.

### Pre-Wired Data Persistence (Serialization)

- **Seamless Pass-Through Design:** Every inventory container features built-in serialization handling pre-wired to the native `PixelDot2D.Core` saving architecture.
- **Zero Inventory Code Modification:** Developers never need to write custom save file handlers, parse file streams, or modify the underlying inventory engine.
- **Frictionless Interface Implementation:** To save any inventory, simply add the `ISaveableAndLoadable` interface to your preferred parent GameObject or controller, and execute a quick forward-call to invoke the underlying inventory's save and load pipeline routines internally.

### Layered Loot Tables Sub-System

- **Quad-Stage Loot Table Engine:** Included within the items extension is a specialized quad-stage loot table engine that allows developers to completely bypass flat, linear probability drop lists and weights by introducing deep, multi-tiered roll isolation. Designers can establish an explicit gatekeeper entry chance on a single index row, then pack its internal array with heavy filler drops surrounding exactly one ultra-rare jackpot item to create intense game-loot tension.
- **Structural Priority-Based Trapping:** The execution pipeline evaluates data configurations sequentially from Index 0 upward. Because the runtime automatically executes a hard, deterministic short-circuit the moment the running item count hits the maximum allowed threshold, array positioning natively dictates statistical priority. This allows designers to balance drop priority purely through the visual order of the inspector list, without requiring complex script overrides or heavy external logic blocks. 
- **State-Blind Reusable Processors:** The core calculation manager is completely state-blind and decoupled from specific character controllers. It can be composed natively into any game entity—including enemies, procedural containers, breakable objects, merchants, and world chests—allowing a single database configuration asset to be safely shared and re-used across an endless amount of active entities.
- **Low API Friction:** Introducing loot drops into any entity takes only a few lines of code, and transferring a rolled payload into a target inventory requires a single line of code. The processor drops results natively into a pre-allocated container, allowing the main framework to instantly absorb, validate, and clear the data packet with a zero runtime garbage memory footprint, seamlessly handling drops from dead enemies, randomly spawned chests, and world stashes out of the box.


---

## Platformer Sub-Library

The Platformer sub-library delivers a highly optimized, streamlined movement controller engineered to demonstrate the seamless, practical integration of the core framework's primary infrastructure—including Asynchronous Serialization, Cross-Device Input wrappers, and the Interaction suite. This module acts as an approachable, lightweight entry point into the broader ecosystem architecture, providing a production-ready baseline that strictly enforces the framework’s high-performance architectural rules without the steep learning curve of fully virtualized or abstract systems.

To preserve strict codebase purity, systems involving highly subjective or design-dependent trade-offs—such as moving platform algorithms or combat mechanics—have been intentionally omitted. This delivers a pristine, unbloated "white-box" architecture, providing developers with a rock-solid, predictable foundation that is instantly ready for custom mechanical extensions.

### Platformer Key Technical Features:

- **Deterministic States:** Out-of-the-box support for precise, frame-perfect genre mechanics, including Grounded, Airborne (with integrated Coyote Time and multi-jump support), Gliding, Wall Climbs, and Ledge Grabs.
- **Frictionless Onboarding Architecture:** Designed with a lean, visible implementation footprint that avoids dense abstraction layers, making it highly readable and exceptionally easy to debug or modify.
- **Native Engine Harmony:** Striking an ideal balance for rapid prototyping, this sub-library interfaces directly with standard Unity physics components while still benefiting from the framework's decoupled data structures.
- **Plug-and-Play Systems Validation:** Serves as a live, functional blueprint displaying exactly how to map runtime data to the save/load pipeline and pass mechanical inputs through the central wrapper.
- **Zero-Allocation Environment Sampling:** Utilizes the Core Physics Module's native casting arrays to evaluate structural boundaries (floors, walls, ledges) with absolute zero runtime garbage collection overhead.
  
- **Encapsulated Extension Hooks:** Features clean, explicit virtual method hooks, allowing you to easily inject custom gameplay behaviors or specialized physics calculations without breaking the core movement loop.

---
*Copyright 2026 - Present © PixelDot2D - All Rights Reserved | Contact: PixelDot2D@gmail.com*

