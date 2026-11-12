# VERDANT: The Pixel Wound — Game Design Document

**Version:** Prototype 0.1 · **Genre:** Third-person action RPG / ecological choice adventure · **Platform:** Browser (desktop and touch)

## High concept

The Pixel Virus is converting a living forest into a luminous, geometric ecosystem. As the Wayfarer, fight the creatures it mutates, recover fragments of digital matter, and decide the fate of each infection wound. Purification restores the forest and ends the threat; planting a Pixel Seed halts the spread but preserves a strange, productive sanctuary. Neither choice is a simple upgrade: restoration protects familiar life, while stabilization accepts permanent change in exchange for rare resources and a living frontier.

## Player promise and pillars

1. **Readable, responsive melee:** make space, choose a weapon, recognize enemy attack tells, and commit to a short combo or a timed defense.
2. **A world that visibly reacts:** infection has a radius and lifecycle, animals can mutate inside it, and vegetation, ground effects, and HUD indicators react to its state.
3. **A meaningful ecological decision:** after defeating a zone's guardians, purify it or spend a scarce seed to preserve a contained pixel biome.
4. **Immediate, low-friction play:** no install or account; keyboard/mouse and touch alternatives are available from the same browser build.

## Core gameplay loop

1. Approach the Mosswound and observe its orange infection boundary, expanding voxel flora, and monster count.
2. Switch between sword, spear, and shield to suit the engagement. Use movement and dodge to avoid attacks; parry on the wind-up or block while facing an attacker.
3. Defeat the guardians. They drop floating Pixel Shards that can be harvested at close range.
4. Enter the wound and choose **Purify** or **Plant a Pixel Seed**. Purification takes several seconds and shrinks the radius to nothing. Seeding immediately arrests expansion but leaves pixel flora, periodic shard deposits, and occasional returning beasts.
5. Explore the resulting ecosystem and use the recovered fragments as a future crafting currency (crafting is a planned extension, not implemented in the prototype).

## Controls

| Action | Desktop | Touch |
|---|---|---|
| Move | WASD / arrow keys | Direction pad |
| Attack / combo | Space, J, or left mouse | HIT |
| Dodge | Shift | DODGE |
| Block | Hold right mouse button | — (use dodge as the mobile defense) |
| Parry | Q | — |
| Switch weapon | 1 / 2 / 3 or weapon buttons | Weapon buttons |
| Purify | E | ACT when the zone is clear |
| Plant a seed | F | Keyboard/compatible external controls; shown in field manual |
| Harvest nearby shard | R | ACT near a shard |
| Field manual | ? button | ? button |

## Combat design

- **Sword — Ashen Sword:** balanced range and high damage; its three-hit chain rewards continuous attacks during the combo window.
- **Spear — March Spear:** long reach and moderate damage; slower recovery and stamina cost favor spacing.
- **Shield — Bulwark Shield:** short-range bash; holding right mouse reduces incoming frontal damage substantially. A block drains stamina. A block from behind does not protect the player.
- **Parry:** Q opens a brief timing window. A successful frontal parry negates the hit and staggers the attacker, creating a punish opportunity. A mistimed parry leaves the player vulnerable.
- **Dodge:** a short burst with a temporary invulnerability window, paid for with stamina.
- **Stamina:** regenerates outside of strenuous actions. Attacks, blocks, and dodges spend it; empty stamina discourages defensive spamming.
- **Recovery:** defeat restores the Wayfarer at the trailhead with partial stamina, keeping the prototype playable without a hard reset.

## Infection-zone state model

`InfectionZone` is the authoritative simulation object. Its state is separate from Three.js meshes; render objects read radius and state and update their indicators.

| State | Spread | Visual treatment | Gameplay |
|---|---|---|---|
| `SPREADING` | Radius expands to its cap; mutation checks can convert animals in the boundary. | Warm orange ring and halo, grid, voxel growth, floating blocks. | Guardians defend the zone. Once cleared, either outcome is available. |
| `PURIFYING` | Expansion stops; the radius contracts over a short channel. | Ring shifts toward green; voxel growth recedes with the boundary. | The Wayfarer must remain safe long enough for recovery to finish. |
| `PURIFIED` | Permanently stopped; radius is zero. | Boundary and pixel field vanish; organic clearing is revealed. | The infection cannot repopulate. The restorative outcome is permanent. |
| `SEEDED` | Permanently stopped. | Cool cyan boundary and retained pixel sanctuary. | Periodic shard deposits and a limited number of returning stalkers sustain the resource loop. |

The state transition is one-way in the prototype: `SPREADING → PURIFYING → PURIFIED` or `SPREADING → SEEDED`. A seed is a limited inventory choice. Zone HUD state, radius, threat count, progress, prompt, color, and scene visibility all derive from the same state to avoid contradictory presentation.

## Pixel Monsters

These are the prototype's three readable combat archetypes. The game uses procedural primitive meshes; the silhouettes and palette are intentionally distinct enough to support later replacement with authored models.

| Monster | Behavior | Prototype combat profile | Drop |
|---|---|---|---|
| **Moss Stalker** | Hunts the Wayfarer and closes at a steady pace; a balanced baseline foe. | 45 HP, medium speed, 14 damage, 2.25 m/s pursuit. | 2 Pixel Shards. |
| **Brambleback** | Heavy, broad-bodied guardian; slow approach, high impact, and more health. A mutated stag can become one. | 70 HP, slow pursuit, 23 damage, 1.65 m/s pursuit. | 4 Pixel Shards. |
| **Shardling** | Small, low silhouette with a fast approach and quick, low-damage attacks. | 45 HP, fast pursuit, 9 damage, 3.35 m/s pursuit. | 1 bright Pixel Shard. |

All foes use a short wind-up before an attack so the player can read, dodge, block, or parry. Prototype values are tuning starting points, not final balance targets.

## World and visual direction

- **Setting:** a quiet, moss-rich woodland trail around one central wound, framed by natural conifers and a clearing.
- **Organic state:** subdued fern greens, warm bark, soft daylight, distant atmospheric fog, and small wandering animals.
- **Infected state:** hard-edged cuboid growths and floating fragments in mint, cyan, and acid green; a warm boundary makes the danger radius legible against the cooler infection palette.
- **Seeded state:** the cyan ring communicates containment rather than cure. Pixel geometry remains visible, and the zone becomes a repeatable resource frontier.
- **Purified state:** the infected geometry, field, grid, and boundary disappear; the clearing returns to the surrounding organic palette.
- **Interface:** compact dark glass panels, monospaced field labels, clear resource counts, contextual prompts, and an optional field manual. The HUD is kept within safe areas on narrow screens.
- **Asset policy for this prototype:** procedural Three.js geometry and built-in materials only; no external model dependency.

## Audio direction (planned)

A restrained forest ambience should give way to a soft digital shimmer near infection. Monster wind-ups need distinct, short tells. Purification should resolve with a warm, organic swell; seeding should use a crystalline locking tone. Audio is not yet implemented.

## Scope and implementation notes

**Implemented prototype:** one forest clearing and one infection zone; spreading/mutation hooks; three monster archetypes; simple pursuit and telegraphed melee; weapon switching, combos, block/parry/dodge; shard drops and collection; purify/seed outcomes; responsive HUD, field manual, keyboard/mouse and touch movement/action controls.

**Planned beyond this prototype:** multiple connected zones, branching narrative, crafting, permanent save data, authored character/monster assets, richer animal mutation and restoration behaviors, audio, mobile touch parry/block buttons, and accessibility settings. Seeded-zone population is a lightweight repeating encounter loop, not a full ecology simulation.

## Prototype acceptance checks

- The scene renders and updates continuously in a modern browser.
- Player can move, switch all three weapons, chain attacks, dodge, block, and parry.
- Monsters pursue, attack after a readable wind-up, and drop harvestable shards on defeat.
- The active zone grows and updates its indicators; animals inside can mutate into beasts.
- With guardians defeated, purify completes and removes the pixel wound, or planting a seed arrests spread while retaining a resource-generating sanctuary.
- HUD and controls remain legible at desktop and mobile widths.
