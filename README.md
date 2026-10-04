# PixelDot2D Core Framework Feature List
What’s Inside PixelDot2D Core Framework

**Available on the:** [Unity Asset Store](https://assetstore.unity.com/packages/tools/utilities/pixeldot2d-core-framework-370674)

> [!NOTE]
> **Living Architecture**
>
> PixelDot2D is actively developed, so this feature list focuses on the major systems and capabilities rather than attempting to document every API, utility, or internal implementation detail.
>
> Each library provides substantially more functionality than can reasonably be listed here. The goal of this document is to show **what PixelDot2D can do and how its systems fit together**, while the source code and XML documentation provide the deeper technical details when you need them.



---

## Table of Contents

* [Core](#core)
* [Combat](#combat)
* [Modular Character](#modular-character)
* [Items](#items)
* [Platformer](#platformer)


---


## Core

**The foundation behind PixelDot2D.**

Core is the only independent library in PixelDot2D. It has no dependency on the other framework libraries, so it can be used on its own or serve as the foundation for everything built above it.

Instead of providing only a collection of utilities, Core handles many of the repetitive infrastructure problems that appear across almost every 2D project — physics queries, collision detection, movement, input, animation, persistence, pooling, diagnostics, and more.

The architecture stays behind the scenes. You use the systems you need through small, focused APIs and extend them when your game requires something different.

### Extensions & Utilities

A broad collection of reusable C# and Unity extensions designed to remove repetitive code and common implementation work.

Includes utilities for:

* Strings, numbers, percentages, comparisons, and angle operations
* `Vector2`, `Transform`, and 2D rotation utilities
* Collections and common data operations
* Unity-specific helpers
* Performance-conscious operations for frequently executed code

Common operations are handled once and reused throughout the framework rather than repeatedly implemented in individual gameplay systems.

---

### Collision

A reusable, allocation-conscious 2D collision system used throughout PixelDot2D.

Instead of relying entirely on Unity's trigger callbacks, the system performs controlled physics queries and provides batched collision information through a consistent API.

* Box, Circle, and Capsule collision shapes
* ScriptableObject-configured collision shapes
* Runtime shape replacement
* `Enter`, `Stay`, and `Exit` lifecycle tracking
* Per-GameObject or per-Collider tracking
* Layer-based filtering
* Batched collision results
* Reusable collision infrastructure for other framework systems
* Custom collision shapes can be added without modifying the core collision system

The same infrastructure can be used for combat hitboxes, character interactions, sensors, hazards, and other gameplay systems.

---

### Sensors

Reusable environment-query infrastructure for systems that need to understand their surroundings.

Designed for high-frequency 2D physics queries such as:

* Ground detection
* Wall detection
* Ceiling detection
* Environment checks
* Component and interface detection
* Cached previous query results

Sensor configurations are reusable ScriptableObjects, allowing the same sensor definitions to be shared across different entities.

---

### Line of Sight

A lightweight 2D line-of-sight system built for frequent gameplay checks.

* Reusable ScriptableObject configurations
* Environmental obstacle filtering
* Early-exit obstacle detection
* Shared configurations across players, enemies, NPCs, or other entities
* Simple runtime API such as `HasLineOfSight(...)`

The system handles the physics-query work so gameplay code only needs to ask whether a target is visible.

---

### Sprite Animation

A lightweight alternative to using Mecanim for projects that need direct control over 2D sprite animation.

`AnimationPlayer2D` provides:

* ScriptableObject-driven animation configurations
* Runtime configuration swapping
* Sequenced animation playback
* Play, stop, insert, and query operations
* Update or FixedUpdate execution
* Direct `SpriteRenderer` integration

Animation data can be configured as reusable assets while runtime playback remains lightweight and code-driven.

---

### 2D Movement

A reusable `Rigidbody2D` movement system designed to handle far more than character movement.

The movement system uses composable movement sequences that can be combined into larger behaviors.

Useful for:

* Characters
* Enemies
* Projectiles
* Moving platforms
* Hazards
* Cinematic movement
* Automated movement patterns

Movement logic remains independent of the identity of the object using it, allowing the same infrastructure to be reused across very different gameplay systems.

---

### Input

A high-level input layer built on Unity's Input System.

`KeybindManager` provides a centralized way to evaluate gameplay input without scattering device-specific input checks throughout gameplay code.

* Unified keybind state evaluation
* Keyboard and Gamepad
* Explicit input-source registration
* Extensible input sources
* Centralized keybind mappings
* Optional save/load integration for persistent keybind configurations

Gameplay systems can work with framework-level input states instead of needing to know where the input originated.

---

### Save & Load

A complete binary save/load pipeline designed to handle the infrastructure around game persistence.

Register systems through `ISaveableAndLoadable`, and the framework handles the surrounding serialization machinery.

* Automatic registration and serialization ordering
* Background disk I/O to minimize frame-time disruption
* Safe main-thread hooks for interacting with Unity objects
* Defensive save directories and rolling backups
* Protection against overlapping save/load operations
* Version-aware serialization
* Support for hierarchical systems and nested saveable components
* Binary streaming designed for efficient game-state persistence

The save manager does not need to know what it is saving. A system simply implements the save/load contract and provides its serialization identity; the infrastructure handles the rest.

---

### Object Pooling

A type-safe generic object pooling system for frequently created and recycled objects.

* Generic, reusable pool infrastructure
* FIFO object retrieval
* Explicit activation control
* Automatic pool hierarchy management
* Repooling and bulk disabling
* Temporal rest handling to help prevent recycled objects from carrying unresolved state into their next use

Useful anywhere a project repeatedly creates and destroys runtime objects such as projectiles, effects, enemies, or gameplay entities.

---

### Diagnostics & Performance

Lightweight tools for measuring and understanding runtime behavior without introducing large profiling systems into gameplay code.

Includes utilities for:

* Measuring execution time
* Comparing repeated operations
* Debugging performance-sensitive code
* Inspecting runtime behavior during development

These tools are intended to make performance investigation practical while keeping the runtime infrastructure lightweight.

---

### Designed to Be Extended

Core systems are built around small contracts and reusable components rather than one fixed gameplay implementation.

You can:

* Use the included systems directly
* Replace individual configurations
* Extend existing behavior
* Create custom collision shapes
* Add custom input sources
* Build new movement behaviors
* Create new sensor configurations
* Compose Core systems into entirely new gameplay systems

Higher-level PixelDot2D libraries build on this same foundation, but Core remains independent and usable by itself.

**Core handles the infrastructure. You build the game.**


---


## Combat

**Build complex combat from reusable pieces instead of writing every weapon from scratch.**

Combat is a modular, configuration-driven combat system built around two simple contracts: `IWeaponizable` for things that can deal damage and `IDamageable` for things that can receive it.

Everything between those contracts is replaceable.

Weapons, aiming behavior, execution rules, projectiles, hitboxes, movement, animation timing, and damage processing can be composed into different combat behaviors without creating a new monolithic weapon system for every attack.

### Modular Weapons

The weapon system separates an attack into independent building blocks that can be mixed and matched.

A weapon can independently define:

* **Virtual Transform** — where the attack originates and how it is oriented
* **Aiming** — recoil, spread, offsets, patterns, and other trajectory adjustments
* **Execution Gate** — cooldowns, ammunition, heat, reloads, or any other condition that controls when an attack can fire
* **Execution** — what actually happens when the attack is released

Because these pieces are independent, the same weapon infrastructure can produce very different behaviors without changing the underlying system.

The framework includes execution types for:

* **Projectile**
* **Hitscan**
* **Hitbox / Cast-based attacks**

This makes the execution layer reusable rather than tying the weapon system to one specific type of combat.

---

### Build Weapons in the Inspector

Weapons are assembled through reusable ScriptableObject Blueprints rather than hard-coded into individual weapon classes.

A weapon Blueprint combines the four major parts of a weapon:

```text
Weapon Blueprint
│
├── Virtual Transform
├── Aiming
├── Execution Gate
└── Execution
```

Configure the pieces, save the Blueprint, and reuse it wherever that weapon behavior is needed.

This allows a single weapon framework to cover behaviors such as:

* Melee attacks
* Guns
* Spread weapons
* Recoil-based weapons
* Lasers
* Projectiles
* Area attacks
* Repeating attacks
* Charged or gated attacks
* Custom weapon behaviors

The goal is not to give you one predefined combat system.

It is to give you reusable pieces that can be assembled into your own.

---

### Stack Weapons Into One System

Combat also provides a higher-level `WeaponManager` for situations where a character needs multiple weapons operating together.

Instead of managing every `WeaponizedModule` individually, you can provide the manager with multiple weapon Blueprints and let it orchestrate them as a single weapon system.

```text
Weapon Manager
│
├── Weapon Blueprint A
├── Weapon Blueprint B
├── Weapon Blueprint C
└── Weapon Blueprint D
```

There is no fixed concept of how many weapons a single managed loadout must contain.

A character can use one weapon, combine several weapons into one attack configuration, or build large layered weapon systems from many Blueprints.

For example:

```text
Character Attack
│
├── Main Projectile
├── Secondary Projectile
├── Hitscan Effect
├── Area Hitbox
└── Additional Weapon
```

The manager handles the macro-level orchestration, allowing the individual weapons to remain focused on their own behavior.

This makes complex loadouts much easier to configure without forcing gameplay code to manually coordinate every weapon instance.

**Build the weapons individually. Combine them into whatever weapon system the game needs.**

---

### Animation-Synchronized Combat

Combat can build directly on Core's `RB2DMovementManager` and `AnimationPlayer2D`, allowing movement, animation, and weapon execution to operate as one configurable sequence.

Weapon execution can be tied to specific animation frames.

For example:

```text
Frame 0 → Begin attack
Frame 3 → Enable hitbox
Frame 5 → Execute damage
Frame 8 → Disable attack
```

The same system can also operate without animation timing. Leave the animation player unassigned and the weapon can execute normally.

This makes the system useful for everything from simple attacks to tightly synchronized attack animations.

---

### Combat Movement

`RB2DMovement_CombatManager` extends the Core movement infrastructure with weapon and animation awareness.

A single movement sequence can define:

* Movement behavior
* Weapon loadout
* Animation playback
* Weapon execution timing
* Weapon transitions
* Targeting
* Phase changes

Different movement phases can use different weapons, or no weapon at all.

For example:

```text
Phase 1 → Move + Weapon A
Phase 2 → Follow Target + Weapon B
Phase 3 → Wait
Phase 4 → Move + No Weapon
```

Weapon changes can occur automatically as the movement sequence transitions between phases.

The underlying movement system remains reusable, so the same infrastructure can drive enemies, players, projectiles, hazards, or other combat entities.

---

### Projectiles

Projectiles are not treated as disposable one-off weapon implementations.

The projectile system combines collision, movement, pooling, state-driven behavior, and weapon execution into a reusable projectile foundation.

A projectile can:

* Follow configurable movement sequences
* Use Core's `RB2DMovementManager`
* Be pooled and reused
* Detect and respond to collisions
* Execute weaponized behavior
* Change behavior through states
* Use configurable formations and trajectories
* Be extended with custom movement, collision, or visual behavior

A single projectile implementation can therefore represent many different projectile types.

**One projectile system. Many possible behaviors.**

---

### Damage Pipeline

Damage is passed between combat systems through a small interface-driven contract rather than requiring weapons and targets to know about each other's implementations.

`IWeaponizable` provides a centralized point where outgoing damage can be inspected or modified before it reaches the target.

This makes systems such as:

* Damage modifiers
* Elemental interactions
* Status effects
* Armor or resistance logic
* Damage-type conversions
* Lifesteal or on-hit behavior

possible without rewriting the weapon execution itself.

The result is a combat pipeline where the attack can remain generic while the entity using it can influence the final result.

---

### Reusable Hit Detection

Combat builds on Core's collision infrastructure rather than introducing a separate collision implementation for every attack type.

Hitbox and cast-based executions can therefore use the same underlying collision capabilities while remaining interchangeable at the weapon level.

This allows the same combat framework to handle different attack shapes and delivery methods without creating separate combat architectures for each one.

---

### Performance-Conscious Execution

Combat systems are designed to avoid doing work until it is actually needed.

Weapon spatial calculations, aiming calculations, and execution processing use lazy evaluation so inactive weapon systems do not continuously perform unnecessary spatial work.

Weapon management also prewarms its runtime weapon capacity where possible, reducing runtime allocation pressure during combat while still allowing loadouts to grow when necessary.

Editor-only trajectory visualization is separated from runtime execution, keeping development-time visualization from becoming part of the production execution path.

---

### Designed for Composition

Combat is intentionally built from small, replaceable modules.

You can combine:

**Virtual Transform + Aiming + Gate + Hitscan**

or

**Virtual Transform + Aiming + Gate + Projectile**

or

**Virtual Transform + Aiming + Hitbox**

Then those weapons can be:

* Used individually
* Combined into larger weapon loadouts
* Synchronized with animation frames
* Placed into combat movement sequences
* Extended with custom modules

The architecture handles the composition.

**You configure the behavior.**

---

### What This Gives You

Instead of creating a new weapon class every time your game needs something different, Combat gives you a reusable set of systems for assembling attacks from existing pieces.

A single weapon can be simple.

A weapon can also be composed from multiple independent systems.

And multiple weapons can be combined into a larger managed loadout that behaves as one coordinated combat system.

The same infrastructure can support a player, enemy, NPC, boss, projectile, hazard, or any other entity that implements the required combat contracts.

You build the weapon pieces once, combine them into larger behaviors, and reuse them wherever the game needs them.

**Build the pieces once. Combine them into the combat your game needs.**


---


## Modular Character

**Build characters by configuring behavior, not by building another giant controller.**

Modular Character is a data-driven character framework built around reusable states, configurable movement, state gates, passives, and scripted behavior.

The important part is what you **don't** have to build.

You create a **Modular Character Blueprint**, configure the character's attributes and available behaviors, and the framework handles the runtime orchestration behind it.

The controller manages state creation, recycling, transitions, lifecycle routing, passive processing, and movement integration so your character logic can stay focused on what the character actually does.

And despite the name, **"character" does not mean player**. The same system can drive players, NPCs, enemies, AI-controlled entities, scripted actors, or anything else that needs state-driven behavior.

---

### Build a Character from a Blueprint

A character starts with a `ModularCharacterBluePrint` ScriptableObject.

The Blueprint acts as the character's reusable configuration:

```text
Modular Character Blueprint
│
├── Character Attributes
│   ├── Stats
│   ├── Passives
│   ├── Immunities
│   └── World Orientation
│
└── Available States
    ├── Idle
    ├── Move
    ├── Jump
    ├── Dash
    └── Custom States
```

You configure the character in the Inspector rather than manually wiring its runtime architecture.

The Blueprint can define:

* Character stats
* Starting passives
* Passive immunities
* Buff and debuff immunities
* World-space orientation
* Available behavioral states
* The character's default state configuration

The framework validates and cleans the configuration for you, including duplicate and null entries where applicable.

Once the Blueprint is configured, the `ModularCharacterController` turns that data into the runtime systems the character needs.

**You configure the character. The framework builds the runtime structure.**

---

### State-Driven Characters

The character controller provides the lifecycle and orchestration while individual states contain the behavior.

A state can represent anything from:

* Idle
* Grounded
* Jumping
* Falling
* Dashing
* Attacking
* Climbing
* Stunned
* Knocked back
* Custom gameplay states

States are created from reusable ScriptableObject configurations and converted into lightweight runtime instances automatically.

The controller does not need to know what each state does. It provides the lifecycle, routes execution, and lets each state implement its own behavior.

Adding a new state therefore does not mean modifying a giant central character controller.

---

### Build States Without Rebuilding the Framework

Creating a new state follows the same three-layer structure used throughout PixelDot2D:

```text
ScriptableObject
      ↓
Data
      ↓
Runtime State
```

The ScriptableObject defines the configuration exposed in the Inspector.

The data layer stores the configuration used by the runtime.

The runtime state contains the actual behavior.

The framework handles runtime creation, recycling, lifecycle management, and integration with the controller automatically.

To add a new behavior, you create the state and add its configuration to the Blueprint.

No central state registry needs to be rewritten.

No giant controller needs another `if` statement.

No separate runtime allocation system needs to be built for the new state.

**You add the behavior. The framework handles the architecture around it.**

---

### States Use the Systems They Need

A state does not need to reinvent the systems it depends on.

For example, a Dash state does **not** implement its own movement system or collision detection.

It simply composes the systems already provided by Core:

```text
Dash State
    │
    ├── Movement ───→ Core Movement
    │
    ├── Collision ─→ Core Sensors
    │
    └── Conditions ─→ State Gates
```

The state focuses on the behavior:

> "Move this way for this long, under these conditions."

Core handles the physics and environment queries underneath it.

This same composition approach can be used to build entirely different behaviors without implementing a new movement or collision system for every state.

**Complex behavior can come from combining simple systems rather than rewriting them.**

---

### Configurable Movement

Modular Character includes a dedicated movement layer built on Core's `RB2DMovementManager`.

Movement is assembled from configurable sequences rather than being tied to one specific genre or controller implementation.

The same infrastructure can be used for:

* Platformer movement
* Top-down movement
* Dashes
* Flying
* Climbing
* Knockback
* Enemy movement
* Patrols
* Scripted movement
* Custom movement patterns

Because the movement layer does not depend on Combat, it can be used independently.

When Combat is added, its combat-aware movement layer can replace the character movement layer while preserving the same underlying movement architecture.

**One movement foundation. Many kinds of characters.**

---

### State Gates

States can use reusable **State Gates** to determine whether a behavior is currently allowed.

A gate can handle conditions such as:

* Cooldowns
* State combinations
* Resource availability
* Damage reactions
* Timing conditions
* Custom transition rules

These gates can go beyond simple timers.

For example, cooldown behavior can react to the character's current states and movement sequences, allowing mechanics such as controlled recovery pauses or rewards for performing specific sequences.

The result is that advanced movement and resource mechanics can be configured as reusable conditions instead of being hard-coded into individual character states.

---

### Modular Passives

Passives use the same composition philosophy as the rest of the framework.

Instead of creating a unique passive implementation for every effect, passives are assembled from three independent pieces:

```text
Gate → Execution → Exit
```

**Gate** determines **when** the passive activates.

**Execution** determines **what** the passive does.

**Exit** determines **when** the passive ends.

The same pieces can therefore be reused across different character mechanics.

For example:

```text
On Damage Taken
      ↓
Modify Stat
      ↓
Exit After Timer
```

Or:

```text
While In State
      ↓
Modify Movement
      ↓
Exit When State Changes
```

Passives can be configured as permanent traits or temporary effects, allowing the same system to cover character abilities, status effects, starting bonuses, equipment-driven behavior, and other gameplay mechanics.

The passive system is intentionally compositional.

**You combine the pieces. The framework handles the passive lifecycle.**

---

### Scripted Characters & AI Behavior

Not every character is controlled directly by the player.

Modular Character also provides a **Scripted State** for characters whose behavior comes from configuration rather than player input.

A scripted state can be composed from:

```text
Scripted State
│
├── Main Action
├── Sub-Actions
└── Exit Conditions
```

This can be used for:

* NPC behavior
* Enemy behavior
* AI movement
* Patrols
* Automated movement
* Environmental characters
* Scripted sequences
* Other non-player behavior

The same movement and sensing systems used by player-controlled characters can drive autonomous behavior.

For example, a scripted character can move through the environment, use Core sensors to detect its surroundings, and transition into another state or Blueprint when a configured condition is met.

The character controller does not need to understand what the behavior means.

It simply executes the configured behavior.

---

### Reusable Behavior, Not Giant Controllers

The traditional character controller tends to grow until one class is responsible for everything:

```text
Input
Movement
Jumping
Attacking
Cooldowns
Damage
Passives
AI
Animation
Collision
Special Abilities
...
```

Modular Character separates those responsibilities into reusable systems.

A state can use Core movement.

A state gate can control when it is allowed to execute.

A passive can use its own Gate, Execution, and Exit.

A scripted state can combine movement, environment checks, and transition conditions.

Combat can provide weapon behavior when the Combat library is present.

Each system does its own job while the controller orchestrates them.

This keeps individual behaviors replaceable and prevents the character controller from becoming the place where every gameplay rule eventually ends up.

---

### Debuggable Runtime States

The runtime states and passives are lightweight C# objects rather than Unity components, but that does not mean they disappear into a black box during development.

Modular Character provides Editor-only runtime snapshots of the currently active states and passives.

These snapshots are diagnostic views of the runtime system. They let you see what is currently active while keeping the actual runtime architecture lightweight.

The visualization is intentionally separate from the runtime state itself, so inspecting the snapshot does not become another source of gameplay behavior.

**Advanced runtime architecture, without sacrificing visibility while debugging.**

---

### Built for Extension

The Modular Character architecture is designed so new behavior can be added without modifying the controller itself.

You can:

* Create new states
* Create new state gates
* Create new passive gates
* Create new passive executions
* Create new passive exits
* Create custom scripted actions
* Create custom scripted exit conditions
* Replace movement configurations
* Compose existing systems into new mechanics
* Create entirely new character behaviors

Runtime creation and recycling are handled by the framework's Factory/Flyweight architecture, while the controller remains focused on orchestration.

You do not need to rebuild the character framework every time your game needs a new mechanic.

**Build the character you need, without rebuilding the character controller.**


---


## Items

**Build items from reusable components, then let the rest of the framework handle what those items do.**

Items are built on top of the same modular architecture used throughout PixelDot2D.

Instead of creating separate hard-coded systems for every potion, piece of equipment, consumable, material, or special item, an item is assembled from reusable components and requirements.

Inventory, equipment, loot, crafting, currency, and merchants then build on that same foundation.

The result is a relatively small Items library that can still support a surprisingly large range of gameplay systems.

---

### Build Items from Components

An item starts with an `SO_ItemBlueprint`.

The Blueprint defines the item's identity, requirements, and behavior through reusable ScriptableObject components.

```text id="j7bq1m"
Item Blueprint
│
├── Item ID
├── Requirements
└── Components
     ├── Modify Stats
     ├── Modify Current Stats
     ├── Apply Passives
     └── Custom Components
```

Components can be combined, reordered, and reused to create different item behaviors.

For example, a single item could:

* Increase maximum health
* Restore current mana
* Apply a passive effect
* Require a specific character condition
* Combine several of these behaviors together

A complex item does not require a new monolithic item class.

The item simply becomes a composition of reusable behaviors.

---

### Items Integrate With Modular Character

Items do not need to implement their own character-stat or passive architecture.

They use the systems already provided by Modular Character.

Equipment can apply structural stat changes or passive behavior directly to a `ModularCharacterController`, while the item itself remains focused on defining **what it provides**.

This means the same character systems can be used by:

* Equipment
* Consumables
* Buffs
* Debuffs
* Character abilities
* Other gameplay systems

The Items library does not duplicate those systems.

**Items use the character architecture instead of creating another one.**

---

### Requirements

Items can define reusable requirements that are evaluated before their components are applied.

Requirements can be used to control whether an item is allowed to affect a character based on the game's rules.

Because requirements are separate from item components, the same requirement logic can be reused across many different items.

The item itself does not need to know how the requirement works.

It simply asks:

> **Can this item be used here?**

If every requirement passes, the item's components are applied.

If one fails, the operation stops.

---

### Inventory

`InventoryManager` provides standalone inventory infrastructure that is not tied to a character.

The same inventory system can therefore be used by:

* Players
* NPCs
* Enemies
* Chests
* Containers
* Loot drops
* World objects
* Merchants
* Custom gameplay systems

Inventory slots support item stacking, insertion, removal, and querying without requiring the inventory itself to understand what owns it.

A higher-level `CharacterInventory` wrapper connects the same inventory infrastructure to Modular Character and adds equipment integration when a character needs it.

```text id="0k4p8x"
CharacterInventory
│
├── InventoryManager
└── EquipmentManager
```

This keeps the underlying inventory system reusable while still providing a convenient character-specific layer.

---

### Equipment

Equipment builds directly on the character and inventory systems.

`EquipmentManager` handles equipped item profiles while remaining encapsulated inside `CharacterInventory`.

Equipped items can therefore influence the character through the same modular item components used elsewhere.

The framework also provides Editor-only runtime snapshots of equipped items for debugging.

These snapshots are diagnostic views rather than the actual runtime data, allowing complex runtime equipment state to remain visible without turning the Inspector representation into the source of gameplay behavior.

---

### Persistence-Ready Inventory

Inventory data is already integrated with Core's `ISaveableAndLoadable` architecture.

Inventory containers and their individual item slots participate in the framework's existing save/load infrastructure rather than requiring Items to implement a separate persistence system.

This means inventories can participate in the same asynchronous, guarded save pipeline provided by Core.

The same infrastructure can therefore persist:

* Player inventories
* Character equipment
* Chests
* Containers
* Other inventory-bearing systems

Items remain focused on gameplay while Core handles the persistence machinery.

---

### Item IDs Without One Giant Enum

Large item databases can quickly become difficult to manage if every item shares one enormous enum.

PixelDot2D avoids that by splitting item identity into categories.

```text id="y1h5r2"
Item Categories
│
├── Consumables
│   ├── Potions
│   ├── Flasks
│   └── ...
│
├── Gear
│   ├── Helmets
│   ├── Chests
│   └── ...
│
└── Custom Categories
```

Each category has its own localized item ID collection while the framework maintains the larger category mapping.

This keeps the Inspector and codebase organized as the item database grows.

You can add an entire new category without turning one global enum into an unmanageable list.

---

### Item Library

`ItemLibrary` provides the centralized bridge between item identities and their actual `SO_ItemBlueprint` assets.

Items can still be inspected and worked with directly through serialized inventory data, but the Item Library provides a consistent runtime lookup and registration layer for projects that need a structured item database and persistence.

At initialization, item assets are validated and indexed into fast runtime lookups.

This gives the rest of the framework a consistent way to resolve:

```text
Category + Item ID
        ↓
Item Blueprint
```

The library is especially useful when items need to survive save/load cycles, because the saved data can identify the item rather than depending on a direct object reference.

**The inventory stores what the player has. The library provides a consistent identity for what that item is.**

---

### Loot Tables

Loot is handled through reusable `SO_LootTable` configurations rather than hard-coded drop logic.

Loot tables can be assigned to enemies, containers, world objects, or any other system that needs randomized item distribution.

More importantly, loot tables can be **nested**.

A loot table can contain other loot tables, which can themselves contain additional tables.

This allows probability structures to be built visually in the Inspector.

For example:

```text id="5g5n7a"
Main Loot Table
│
├── Junk
├── Junk
├── Junk
└── Rare Loot Table
      │
      ├── Common
      ├── Common
      ├── Common
      └── Extremely Rare
```

By controlling the composition and repetition of entries, you can create layered probability distributions without manually calculating and hard-coding every final outcome.

This makes sophisticated loot weighting possible while keeping the configuration understandable and editable.

You can build the probability structure visually rather than writing a custom loot algorithm for every enemy or container.

---

### Crafting

Crafting is driven by reusable crafting table configurations.

A `CraftingStation` can use a ScriptableObject crafting table and swap that configuration during runtime.

This makes progression-based crafting straightforward to represent.

For example:

```text
Basic Workbench
      ↓
Upgraded Workbench
      ↓
Advanced Workbench
```

The underlying crafting system does not need to change.

Only the active crafting configuration changes.

Crafting stations can therefore represent anything from a simple workbench to upgraded facilities with completely different available recipes.

---

### Craft From Multiple Inventories

Crafting is not limited to pulling materials from a single inventory.

`CraftingStation` can evaluate multiple `InventoryManager` instances as a combined material source.

For example:

```text
Chest A → 1 Wood
Chest B → 1 Wood
Chest C → 1 Wood
   ...
Chest Z → 1 Wood

            ↓

      Crafting Recipe
        Requires 26 Wood

            ↓

      Combined Resources
        26 Wood Available

            ↓

       Craft Successfully
```

The system searches across the supplied inventories and aggregates their available materials.

If a recipe requires 100 Wood and those 100 Wood are distributed across 100 different chests, the recipe can still be crafted.

The crafting system does not care which individual container holds the materials.

---

### Transaction-Safe Crafting

Resource consumption uses a validation-first process.

Before removing anything, the crafting system:

1. Checks that the destination inventory has enough space for the crafted result.
2. Performs a dry-run across all source inventories.
3. Verifies that **every recipe requirement** can be completely satisfied.
4. Only after validation succeeds does it begin deducting materials.
5. Consumes the required resources across the inventories until the recipe is fully satisfied.
6. Places the crafted item into the specified destination inventory.

If the combined inventories contain insufficient resources, the craft simply fails without partially consuming anything.

```text
Validate
   │
   ├── Output Space ──────── ✓
   ├── Requirement A ─────── ✓
   ├── Requirement B ─────── ✓
   └── Requirement C ─────── ✓
             │
             ▼
          Deduct
             │
             ▼
          Craft
             │
             ▼
      Add Output Item
```

The source and destination inventories are also explicitly separated.

This means you can choose exactly where materials are allowed to come from and where the crafted item should be placed.

For example, a player's inventory can be used as both a source and destination simply by passing it into both roles.

The important part is that the framework handles the multi-container search, validation, deduction, and failure safety for you.

**Give the crafting system the inventories. It handles the rest.**

---

### Currency

Currency is implemented as a lightweight, reusable ledger rather than being tied to a player.

The same currency system can be composed into:

* Players
* NPCs
* Merchants
* Chests
* World objects
* Custom economic systems

Currency state also participates in Core's save/load architecture.

The persistence system takes a safe snapshot of the currency data before handing the serialized representation to the background save process, keeping the live runtime collection isolated from the asynchronous persistence work.

This allows currency to participate in the same performance-conscious persistence architecture as the rest of the framework.

---

### Merchants

`MerchantManager` provides reusable buying, selling, and buy-back functionality without tying commerce to a specific character or scene object.

Merchant inventories can be configured through reusable merchant tables and swapped dynamically at runtime.

Most importantly, **items do not dictate their own universal merchant price.**

An item can define its general vendor value, but each merchant can establish its own pricing rules.

That means the same item can legitimately have different prices depending on where it is sold:

```text
Merchant A
Coal → 1 Gold

Merchant B
Coal → 1 Gem

Merchant C
Coal → 10 Gold
```

The item remains unaware of the merchant's economy.

This keeps item data reusable while allowing individual merchants, factions, locations, or progression systems to establish their own economies.

---

### Designed for Composition

Items follow the same architectural philosophy as the rest of PixelDot2D.

You can combine:

**Item + Stat Modifier**

or

**Item + Passive + Requirement**

or

**Item + Multiple Stat Changes + Passive + Requirements**

Then place those items into inventories, equipment systems, loot tables, crafting recipes, merchants, or completely custom gameplay systems.

The Items library does not need a giant class hierarchy to support this.

It reuses the systems already provided by Core and Modular Character.

---

### Built to Grow With Your Game

The Items library is intentionally small because it does not attempt to duplicate the architecture underneath it.

Core provides:

* Persistence
* Interfaces
* Runtime infrastructure
* Performance-conscious utilities

Modular Character provides:

* Stats
* Passives
* Character behavior
* State-driven systems

Items provides:

* Item composition
* Inventory
* Equipment
* Loot
* Crafting
* Currency
* Merchants

Each layer stays focused on its own responsibility.

That makes the Items library easy to extend without turning it into another monolithic gameplay system.

**The framework handles the systems around the item. You decide what the item does.**


---


## Platformer

**A straightforward example of putting the framework's architecture together.**

Platformer is a small, more approachable implementation of a 2D platformer / metroidvania-style player.

It uses the same core ideas found throughout PixelDot2D:

* Isolated state-driven behavior
* Core physics and sensor systems
* ScriptableObject-driven configuration
* Lightweight runtime state classes
* Centralized orchestration
* Queued state transitions
* Reusable animation and input systems

The difference is that Platformer keeps the implementation much closer to familiar Unity patterns.

`Actor_PlatformerPlayer` acts as the main MonoBehaviour hub, while individual player behaviors are separated into states such as movement, airborne behavior, attacks, wall movement, and other abilities.

This makes the codebase useful for seeing **how the same architectural structure can be applied without requiring the heavier abstractions of Modular Character.**

It also demonstrates practical integration with Core systems, including runtime key rebinding and saving/loading those configurations.

For example, keybinds can be changed at runtime, reset to defaults, and persisted through the existing `KeybindManager` and `SaveAndLoadManager` systems.

Platformer is therefore less about providing another large framework layer and more about showing the architecture in a concrete, easy-to-follow implementation.

**Same principles. Simpler surface.**


---
*Copyright 2026 - Present © PixelDot2D - All Rights Reserved | Contact: PixelDot2D@gmail.com*

