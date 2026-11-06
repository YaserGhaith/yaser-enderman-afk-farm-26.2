# Yaser Enderman AFK FARM V1

A **[Meteor Client](https://meteorclient.com) addon** that automatically AFK-farms
Endermen (or any mobs you whitelist): it scans nearby entities, picks the best
melee weapon in your hotbar, and attacks on Minecraft's **vanilla attack
cooldown** — no lag-switch style spam, fully client-side timing.

Built for the modern date-versioned Minecraft generation.

| | |
|---|---|
| **Mod name** | Yaser Enderman AFK FARM V1 |
| **Module id** | `yaser-enderman-afk-farm-v1` (Combat category) |
| **Minecraft** | 26.2 |
| **Fabric Loader** | 0.19.5 |
| **Mappings** | Official Mojang mappings (no Yarn) |
| **Loom** | 1.18-SNAPSHOT |
| **Gradle** | 9.8.1 (wrapper included) |
| **JDK** | 25 |
| **Meteor Client** | 26.2-SNAPSHOT |
| **Fabric API** | **Not required** (Meteor's new generation doesn't use it) |

## Building

```bash
./gradlew build        # Windows: gradlew.bat build
```

> Requires **JDK 25** (the project uses Gradle toolchains; install Temurin 25
> or let your IDE provision it).

The finished jar appears at:

```
build/libs/yaser-enderman-afk-farm-v1-1.0.0.jar
```

## Installing

1. Install **Fabric Loader 0.19.5+** for Minecraft 26.2.
2. Download the **Meteor Client build for MC 26.2** (`26.2-SNAPSHOT` or newer)
   and put it in your `.minecraft/mods` folder.
3. Put `yaser-enderman-afk-farm-v1-1.0.0.jar` in the same folder.
4. Launch the game → Meteor GUI (Right Shift) → **Combat** →
   *Yaser Enderman AFK Farm*.

> Do **not** keep both the old 1.20.x jar and this jar installed at once.

## Module settings

### General
| Setting | Default | Description |
|---|---|---|
| `range` | 4.5 | Maximum scan distance (blocks) for targets. |
| `attack-range` | 3.0 | Vanilla-safe reach for the actual hit (measured eye → target). |
| `cooldown-threshold` | 1.0 | Attack only when the vanilla attack cooldown is at least this charged (1.0 = fully vanilla-timed). |
| `pause-while-using-item` | true | Stops attacking while eating/drinking/drawing a bow/using an item. |

### Targeting
| Setting | Default | Description |
|---|---|---|
| `entities` | Enderman | Whitelist of entities to hunt (`onlyAttackable` list; add more mobs freely). |
| `ignore-creative-players` | true | Skips creative/spectator players. |

### Weapon
| Setting | Default | Description |
|---|---|---|
| `switch-mode` | Switch | *None* – keep your current item. *Switch* – actually swap to the best weapon. *Silent* – swap only for the attack itself. |
| `favor-enchantments` | true | Scores Sharpness / Fire Aspect weapons higher. |
| `require-weapon` | false | Only attack while holding a sword / axe / trident. |
| `switch-back` | true | Restores your previous hotbar slot when no target is around or the module is disabled. |

### Rendering
| Setting | Default | Description |
|---|---|---|
| `highlight` | true | Draws a box around the current target. |
| `shape-mode` | Both | Sides / Lines / Both. |
| `side-color` | purple fill | Highlight fill color. |
| `line-color` | purple outline | Highlight outline color. |

The module also shows HUD info while active: current target name and your
attack-cooldown percentage.

## How the weapon picker works (MC 26.x)

Item classes like `SwordItem` no longer exist in modern Minecraft, so the
weapon score is computed from the **data-driven item components**:

1. Base category via item tags: `#minecraft:swords` → `#minecraft:axes` →
   `Trident`, otherwise a minimal generic-melee score.
2. Melee damage read from the `minecraft:attribute_modifiers` component
   (`Attributes.ATTACK_DAMAGE` in the MAINHAND slot).
3. Enchantment bonus levels read via Meteor's
   `Utils.getEnchantmentLevel(stack, Enchantments.SHARPNESS / FIRE_ASPECT)`.

## Project layout

```
src/main/java/com/example/addon/EndermanAddon.java        # addon entrypoint
src/main/java/com/example/addon/modules/EndermanKiller.java # the module
src/main/resources/fabric.mod.json
gradle/libs.versions.toml                                 # version catalog
build.gradle.kts / settings.gradle.kts / gradle.properties
```

## License

[MIT](LICENSE)
