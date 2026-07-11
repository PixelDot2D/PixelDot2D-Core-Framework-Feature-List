# PixelDot2D Core Framework Feature List
What’s Inside PixelDot2D Core Framework

## Table Of Contents

- [Core Engine Layer](#core-engine-layer-the-autonomous-foundation)
  - [Core Key Technical Features](#core-key-technical-features)
- [Combat Sub-Library](#Combat-Sub-Library-The-Universal-2D-Execution-Engine)
  - [Combat Key Technical Features](#combat-key-Technical-features)

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



