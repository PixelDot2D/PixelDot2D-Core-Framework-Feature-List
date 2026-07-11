# PixelDot2D Core Framework Feature List
What’s Inside PixelDot2D Core Framework

> [!NOTE]
> **Living Architecture Notice:** This feature list serves as a high-level technical overview of the framework's primary infrastructure. PixelDot2D is under continuous, active development, with new subsystems, optimizations, and utility modules being integrated regularly. To prevent this document from expanding into an exhaustive API manual, we have focused exclusively on the most critical engineering milestones and performance metrics per sub-library. The framework contains substantial internal utility suites beyond what is outlined below.


## Table Of Contents

- [Core Engine Layer](#core-engine-layer-the-autonomous-foundation)
- [Combat Sub-Library](#Combat-Sub-Library-The-Universal-2D-Execution-Engine)
- [Modular Character Sub-Library](#modular-character-Sub-Library-Full-Runtime-Reconfigurability)
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

To ensure absolute stability during complex, multi-state reconfigurations, all state transitions and behavior mutations are deferred through an isolated Queue System that completely neutralizes multi-threaded race conditions and volatile null-reference exceptions. This decoupled architecture enables absolute, zero-code-change extensibility: entirely new operational states can be introduced and integrated into existing character behaviors without altering a single line of pre-existing code inside the master orchestrator. By leveraging the data-driven input pipeline and automated flyweight factory, developers can map complex transitions into newly authored states directly from the Unity Inspector using pre-built blueprint templates engineered for any movement profile a native `Rigidbody2D` can execute.

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

## Platformer Sub-Library

The Platformer sub-library delivers a highly optimized, streamlined movement controller engineered to demonstrate the seamless, practical integration of the core framework's primary infrastructure—including Asynchronous Serialization, Cross-Device Input wrappers, and the Interaction suite. This module acts as an approachable, lightweight entry point into the broader ecosystem architecture, providing a production-ready baseline that strictly enforces the framework’s high-performance architectural rules without the steep learning curve of fully virtualized or abstract systems.

To preserve strict codebase purity, systems involving highly subjective or design-dependent trade-offs—such as moving platform algorithms or combat mechanics—have been intentionally omitted. This delivers an pristine, unbloated "white-box" architecture, providing developers with a rock-solid, predictable foundation that is instantly ready for custom mechanical extensions.

### Platformer Key Technical Features:

- **Deterministic States:** Out-of-the-box support for precise, frame-perfect genre mechanics, including Grounded, Airborne (with integrated Coyote Time and multi-jump support), Gliding, Wall Climbs, Ledge Grabs, and One-Way Platform pass-throughs.
- **Frictionless Onboarding Architecture:** Designed with a lean, visible implementation footprint that avoids dense abstraction layers, making it highly readable and exceptionally easy to debug or modify.
- **Native Engine Harmony:** Striking an ideal balance for rapid prototyping, this sub-library interfaces directly with standard Unity physics components while still benefiting from the framework's decoupled data structures.
- **Plug-and-Play Systems Validation:** Serves as a live, functional blueprint displaying exactly how to map runtime data to the save/load pipeline and pass mechanical inputs through the central wrapper.
- **Zero-Allocation Environment Sampling:** Utilizes the Core Physics Module's native casting arrays to evaluate structural boundaries (floors, walls, ledges) with absolute zero runtime garbage collection overhead.
- **Encapsulated Extension Hooks:** Features clean, explicit virtual method hooks, allowing you to easily inject custom gameplay behaviors or specialized physics calculations without breaking the core movement loop.

---
*Copyright 2026 © PixelDot2D - All Rights Reserved | Contact: PixelDot2D@gmail.com*
