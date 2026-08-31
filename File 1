 (cd "$(git rev-parse --show-toplevel)" && printf '%s' 'diff --git a/README.md b/README.md
new file mode 100644
index 0000000000000000000000000000000000000000..644fb89c8409cb809050a18d39c7c4491b913b40
--- /dev/null
+++ b/README.md
@@ -0,0 +1,44 @@
+# The Pantheon: Echoes of the Divine — v0.1.0 BETA
+
+This repository is the first playable foundation for **The Pantheon** SMP: Ares armour, Ares'\''s Saber, Ares Axe, and a reusable divine-equipment/scripting framework. `0.x.x` denotes beta development. **`1.0.0` is reserved for the first CurseForge/MCPEDL public release.**
+
+## Install
+
+Import `dist/The-Pantheon-v0.1.0-BETA.mcaddon` into a modern Minecraft Bedrock installation, activate both contained packs for a world, and enable the experiment(s) required by that installation for custom items and Script API. The add-on targets `min_engine_version` **1.21.110** and declares `@minecraft/server` **2.1.0**. The behaviour pack declares an explicit dependency on the included resource pack.
+
+Give items with `/give @s pantheon:ares_saber`, `/give @s pantheon:ares_axe`, and the four Ares armour identifiers in `behavior_pack/items/`.
+
+## Equipment and placeholders
+
+All six IDs are genuine `pantheon:*` custom items, not renamed vanilla items. Saber uses a vanilla **stick** icon. The axe uses a vanilla netherite-axe icon. Ares helmet, chestplate, leggings, and boots use vanilla **netherite** inventory/attached-armour placeholders. Their exact display names are dark-red, bold Ares names. Every divine item has a durability component with a one-point maximum and a `0..0` damage chance; unlike a very large durability value, Bedrock never consumes durability under normal use.
+
+Custom armour has protection values matching the Netherite armour slots (3/8/6/3). Custom items cannot inherit vanilla mining tool classes wholesale, so the weapons use `hand_equipped` and native custom-item `damage` (Saber 8; Axe 10) rather than pretending to be vanilla swords/axes.
+
+## Ares rules
+
+* **Full set rule:** Bloodlust and ability activation require all four Ares armour pieces equipped. Breaking the set is detected within one second and immediately resets Bloodlust. Possession alone never activates Ares.
+* **Bloodlust:** A landed melee hit with the Ares Saber or Axe increments one stack (maximum 20), refreshes a 5-second expiry, and applies an extra linear damage portion: `baseDamage × previousStacks × 0.025`. Therefore native + extra damage is `baseDamage × (1 + stacks × .025)` for subsequent hits. A hit at 20 stacks remains capped at 20. Frenzy sets (not adds) stacks to 10, then the ordinary 5-second timer applies.
+* **Movement speed limitation:** Bedrock Script API currently has no supported player movement-attribute setter. Exact 1.0–3.0× Bloodlust speed is therefore not implemented; the stack state, damage calculation, UI, and expiry are implemented accurately. This is intentionally documented rather than replaced with an unrelated effect.
+* **Rage:** use Saber while full-set-equipped for 20 seconds. It adds Resistance II (`amplifier: 1`) and uses the entity-hurt event bridge to make incoming raw damage 50% before armour (`5/6` replacement damage multiplied by Resistance II'\''s 60%). Cooldown begins only when active duration ends, lasts 60 seconds, and total availability time is 80 seconds.
+* **Frenzy:** use Axe while full-set-equipped for 30 seconds. It grants Haste II (`amplifier: 1`), sets Bloodlust to 10, then starts a 180-second cooldown after expiry (210 seconds activation-to-ready).
+* **Weaving limitation/workaround:** Bedrock has no literal Weaving effect/cobweb collision modifier. During Frenzy only, a one-tick scoped loop removes cobweb blocks intersecting the player’s feet/head cells. This produces practical cobweb traversal but changes those cobweb blocks; it is not represented as a fake status effect.
+* **Death/leave:** death removes temporary effects, resets stacks and cooldowns. Leave deletes only the primitive state record and active-Frenzy reference; rejoin starts clean.
+
+## Combat and Blurry'\''s Java PvP
+
+The pack deliberately does **not** implement a separate Java-style swing cooldown. Bedrock exposes custom item damage and landed-hit events but not Java attack-strength/cooldown mechanics. The requested external Blurry integration could not be verified in this build environment: outbound access to Microsoft Learn, GitHub, and the web tool was denied (HTTP 401/403), and no Blurry pack/version/API files were supplied in this repository. No proprietary Blurry files were copied or redistributed.
+
+Consequently this version uses native custom-item attacks and does not claim a verified Blurry bridge. Before deploying alongside Blurry'\''s Java PvP, set the target Blurry release and use its documented custom-weapon registration mechanism for `pantheon:ares_saber` damage 8 and `pantheon:ares_axe` damage 10. That integration should own attack timing, avoiding two combat cooldown systems.
+
+## Architecture
+
+* `scripts/core/registry.js`: expandable god/equipment registry and divine-item predicate.
+* `scripts/core/playerState.js`, `cooldownManager.js`: isolated player records and tick-based active/cooldown lifecycle.
+* `scripts/core/equipment.js`, `actionBar.js`, `debug.js`: reusable equipment test, once-per-second UI, and opt-in diagnostics.
+* `scripts/ares/ares.js`: event subscriptions and Ares-specific abilities/passive. The only per-tick operation is restricted to currently Frenzy-active players for cobweb removal; general maintenance and action-bar countdowns run once per second.
+
+The action bar shows Rage/Frenzy as `READY`, `ACTIVE Ns`, or a rounded remaining cooldown plus Bloodlust `0/20` through `20/20`.
+
+## Validation boundary
+
+JSON, UUID format, JavaScript syntax, local import paths, manifest resource dependency, and packaged archive contents are validated by repository checks. Minecraft Bedrock could not be launched in this environment, so in-game API compatibility and gameplay have **not** been falsely claimed as executed tests.
diff --git a/behavior_pack/items/ares_axe.json b/behavior_pack/items/ares_axe.json
new file mode 100644
index 0000000000000000000000000000000000000000..1570666e01086720f51aebc66ed10e8c4cd1c6f7
--- /dev/null
+++ b/behavior_pack/items/ares_axe.json
@@ -0,0 +1,25 @@
+{
+  "format_version": "1.21.110",
+  "minecraft:item": {
+    "description": {
+      "identifier": "pantheon:ares_axe"
+    },
+    "components": {
+      "minecraft:icon": "pantheon_ares_axe",
+      "minecraft:display_name": {
+        "value": "\u00a74\u00a7lAres Axe"
+      },
+      "minecraft:max_stack_size": 1,
+      "minecraft:durability": {
+        "max_durability": 1,
+        "damage_chance": {
+          "min": 0,
+          "max": 0
+        }
+      },
+      "minecraft:rarity": "epic",
+      "minecraft:damage": 10,
+      "minecraft:hand_equipped": true
+    }
+  }
+}
diff --git a/behavior_pack/items/ares_boots.json b/behavior_pack/items/ares_boots.json
new file mode 100644
index 0000000000000000000000000000000000000000..6f4ca0ee8b3b49689699c955ce12d5fed283acc7
--- /dev/null
+++ b/behavior_pack/items/ares_boots.json
@@ -0,0 +1,27 @@
+{
+  "format_version": "1.21.110",
+  "minecraft:item": {
+    "description": {
+      "identifier": "pantheon:ares_boots"
+    },
+    "components": {
+      "minecraft:icon": "pantheon_ares_boots",
+      "minecraft:display_name": {
+        "value": "\u00a74\u00a7lAres Boots"
+      },
+      "minecraft:max_stack_size": 1,
+      "minecraft:durability": {
+        "max_durability": 1,
+        "damage_chance": {
+          "min": 0,
+          "max": 0
+        }
+      },
+      "minecraft:rarity": "epic",
+      "minecraft:wearable": {
+        "slot": "slot.armor.feet",
+        "protection": 3
+      }
+    }
+  }
+}
diff --git a/behavior_pack/items/ares_chestplate.json b/behavior_pack/items/ares_chestplate.json
new file mode 100644
index 0000000000000000000000000000000000000000..0ed7014cbf541ce22e752fbc1c4def27eaa53db2
--- /dev/null
+++ b/behavior_pack/items/ares_chestplate.json
@@ -0,0 +1,27 @@
+{
+  "format_version": "1.21.110",
+  "minecraft:item": {
+    "description": {
+      "identifier": "pantheon:ares_chestplate"
+    },
+    "components": {
+      "minecraft:icon": "pantheon_ares_chestplate",
+      "minecraft:display_name": {
+        "value": "\u00a74\u00a7lAres Chestplate"
+      },
+      "minecraft:max_stack_size": 1,
+      "minecraft:durability": {
+        "max_durability": 1,
+        "damage_chance": {
+          "min": 0,
+          "max": 0
+        }
+      },
+      "minecraft:rarity": "epic",
+      "minecraft:wearable": {
+        "slot": "slot.armor.chest",
+        "protection": 8
+      }
+    }
+  }
+}
diff --git a/behavior_pack/items/ares_helmet.json b/behavior_pack/items/ares_helmet.json
new file mode 100644
index 0000000000000000000000000000000000000000..f1f27ac65c065109b39c43cbba8367c3125cd806
--- /dev/null
+++ b/behavior_pack/items/ares_helmet.json
@@ -0,0 +1,27 @@
+{
+  "format_version": "1.21.110",
+  "minecraft:item": {
+    "description": {
+      "identifier": "pantheon:ares_helmet"
+    },
+    "components": {
+      "minecraft:icon": "pantheon_ares_helmet",
+      "minecraft:display_name": {
+        "value": "\u00a74\u00a7lAres Helmet"
+      },
+      "minecraft:max_stack_size": 1,
+      "minecraft:durability": {
+        "max_durability": 1,
+        "damage_chance": {
+          "min": 0,
+          "max": 0
+        }
+      },
+      "minecraft:rarity": "epic",
+      "minecraft:wearable": {
+        "slot": "slot.armor.head",
+        "protection": 3
+      }
+    }
+  }
+}
diff --git a/behavior_pack/items/ares_leggings.json b/behavior_pack/items/ares_leggings.json
new file mode 100644
index 0000000000000000000000000000000000000000..b6add87352522e71b6855b17bc376710377090f7
--- /dev/null
+++ b/behavior_pack/items/ares_leggings.json
@@ -0,0 +1,27 @@
+{
+  "format_version": "1.21.110",
+  "minecraft:item": {
+    "description": {
+      "identifier": "pantheon:ares_leggings"
+    },
+    "components": {
+      "minecraft:icon": "pantheon_ares_leggings",
+      "minecraft:display_name": {
+        "value": "\u00a74\u00a7lAres Leggings"
+      },
+      "minecraft:max_stack_size": 1,
+      "minecraft:durability": {
+        "max_durability": 1,
+        "damage_chance": {
+          "min": 0,
+          "max": 0
+        }
+      },
+      "minecraft:rarity": "epic",
+      "minecraft:wearable": {
+        "slot": "slot.armor.legs",
+        "protection": 6
+      }
+    }
+  }
+}
diff --git a/behavior_pack/items/ares_saber.json b/behavior_pack/items/ares_saber.json
new file mode 100644
index 0000000000000000000000000000000000000000..2e49bfb7e873e2ca8b1c0316e3a6445ff6460d2b
--- /dev/null
+++ b/behavior_pack/items/ares_saber.json
@@ -0,0 +1,25 @@
+{
+  "format_version": "1.21.110",
+  "minecraft:item": {
+    "description": {
+      "identifier": "pantheon:ares_saber"
+    },
+    "components": {
+      "minecraft:icon": "pantheon_ares_saber",
+      "minecraft:display_name": {
+        "value": "\u00a74\u00a7lAres\u2019s Saber"
+      },
+      "minecraft:max_stack_size": 1,
+      "minecraft:durability": {
+        "max_durability": 1,
+        "damage_chance": {
+          "min": 0,
+          "max": 0
+        }
+      },
+      "minecraft:rarity": "epic",
+      "minecraft:damage": 8,
+      "minecraft:hand_equipped": true
+    }
+  }
+}
diff --git a/behavior_pack/manifest.json b/behavior_pack/manifest.json
new file mode 100644
index 0000000000000000000000000000000000000000..9ddadcee963ba907088047433912c5bab0a1beb9
--- /dev/null
+++ b/behavior_pack/manifest.json
@@ -0,0 +1,13 @@
+{
+  "format_version": 2,
+  "header": {"name":"The Pantheon: Echoes of the Divine BP","description":"The Pantheon v0.1.0 BETA — Ares gameplay","uuid":"2d22bf83-3d24-4d3e-af1a-53646d4c17c2","version":[0,1,0],"min_engine_version":[1,21,110]},
+  "modules": [
+    {"type":"data","uuid":"d6b4ed45-6d68-4b91-a919-d206137ddff2","version":[0,1,0]},
+    {"type":"script","language":"javascript","uuid":"ef926d38-103c-410c-b946-6077572a52e3","version":[0,1,0],"entry":"scripts/main.js"}
+  ],
+  "dependencies":[
+    {"uuid":"31fae8e0-e0f4-4177-a85a-7ea7bbd41115","version":[0,1,0]},
+    {"module_name":"@minecraft/server","version":"2.1.0"}
+  ],
+  "metadata":{"authors":["The Pantheon"],"product_type":"addon"}
+}
diff --git a/behavior_pack/scripts/ares/ares.js b/behavior_pack/scripts/ares/ares.js
new file mode 100644
index 0000000000000000000000000000000000000000..d055d0081ce8495ec84fa870d6152bfea8106d2b
--- /dev/null
+++ b/behavior_pack/scripts/ares/ares.js
@@ -0,0 +1,141 @@
+import { system, world } from "@minecraft/server";
+import { ARES } from "../core/registry.js";
+import { getState, clearState, deleteState } from "../core/playerState.js";
+import { hasFullAresSet, heldItemIs } from "../core/equipment.js";
+import { activate, advance, isReady, reset } from "../core/cooldownManager.js";
+import { updateActionBar } from "../core/actionBar.js";
+import { debug } from "../core/debug.js";
+
+const BLOODLUST_TIMEOUT = 100; // 5 seconds at 20 game ticks per second.
+const MAX_BLOODLUST = 20;
+const DAMAGE_PER_STACK = 0.025;
+const activeFrenzyPlayers = new Map();
+const rageReapplication = new Set();
+
+function resetBloodlust(player, state = getState(player)) {
+  state.bloodlustStacks = 0;
+  state.bloodlustExpiresAt = 0;
+}
+function setBloodlust(player, stacks, tick) {
+  const state = getState(player);
+  state.bloodlustStacks = Math.max(0, Math.min(MAX_BLOODLUST, stacks));
+  state.bloodlustExpiresAt = state.bloodlustStacks ? tick + BLOODLUST_TIMEOUT : 0;
+}
+function refreshBloodlustFromHit(player, tick) {
+  const state = getState(player);
+  setBloodlust(player, state.bloodlustStacks + 1, tick);
+}
+function endRage(player, state) {
+  // Resistance II supplies the requested visual/status effect. It is removed at
+  // end rather than left to an unusually delayed server tick.
+  player.removeEffect("resistance");
+  debug(`Rage ended for ${player.name}`);
+}
+function endFrenzy(player, state) {
+  player.removeEffect("haste");
+  activeFrenzyPlayers.delete(player.id);
+  debug(`Frenzy ended for ${player.name}`);
+}
+function cleanupPlayer(player) {
+  const state = getState(player);
+  if (state.rage.active) endRage(player, state);
+  if (state.frenzy.active) endFrenzy(player, state);
+  reset(state.rage); reset(state.frenzy); resetBloodlust(player, state);
+  activeFrenzyPlayers.delete(player.id);
+  clearState(player);
+}
+function activateRage(player, tick) {
+  const state = getState(player);
+  if (!hasFullAresSet(player) || !isReady(state.rage, tick)) return;
+  activate(state.rage, tick, ARES.abilities.rage.durationTicks);
+  // Amplifier 1 is Resistance II. The damage event bridge below compensates for
+  // Resistance II'\''s 40% reduction so the pre-armour Rage result is 50% damage.
+  player.addEffect("resistance", ARES.abilities.rage.durationTicks, { amplifier: 1, showParticles: true });
+  updateActionBar(player, tick);
+  debug(`Rage activated for ${player.name}`);
+}
+function activateFrenzy(player, tick) {
+  const state = getState(player);
+  if (!hasFullAresSet(player) || !isReady(state.frenzy, tick)) return;
+  activate(state.frenzy, tick, ARES.abilities.frenzy.durationTicks);
+  player.addEffect("haste", ARES.abilities.frenzy.durationTicks, { amplifier: 1, showParticles: true });
+  // Frenzy is a direct stack assignment, then starts the normal five-second
+  // timer. This intentionally preserves Bloodlust'\''s normal refresh rule.
+  setBloodlust(player, 10, tick);
+  activeFrenzyPlayers.set(player.id, player);
+  updateActionBar(player, tick);
+  debug(`Frenzy activated for ${player.name}`);
+}
+
+export function registerAres() {
+  world.afterEvents.itemUse.subscribe((event) => {
+    const player = event.source;
+    if (player.typeId !== "minecraft:player") return;
+    const tick = system.currentTick;
+    if (event.itemStack.typeId === ARES.equipment.saber) activateRage(player, tick);
+    if (event.itemStack.typeId === ARES.equipment.axe) activateFrenzy(player, tick);
+  });
+
+  world.afterEvents.entityHitEntity.subscribe((event) => {
+    const player = event.damagingEntity;
+    if (player.typeId !== "minecraft:player" || !hasFullAresSet(player)) return;
+    if (!heldItemIs(player, ARES.equipment.saber) && !heldItemIs(player, ARES.equipment.axe)) return;
+    const state = getState(player);
+    const priorStacks = state.bloodlustStacks;
+    refreshBloodlustFromHit(player, system.currentTick);
+    // entityHitEntity fires only for a landed melee hit, never for an air swing.
+    // The native item damage lands first; this applies only the *linear* bonus.
+    const baseDamage = heldItemIs(player, ARES.equipment.saber) ? 8 : 10;
+    if (priorStacks > 0 && event.hitEntity.isValid) {
+      event.hitEntity.applyDamage(baseDamage * priorStacks * DAMAGE_PER_STACK, { cause: "entityAttack", damagingEntity: player });
+    }
+  });
+
+  // Bedrock does not expose a writable vanilla damage multiplier. Cancelling
+  // and reapplying in the next writable phase is the supported-event fallback.
+  // The guard prevents our replacement damage from recursively being replaced.
+  world.beforeEvents.entityHurt.subscribe((event) => {
+    const player = event.hurtEntity;
+    if (player.typeId !== "minecraft:player" || !getState(player).rage.active || rageReapplication.has(player.id)) return;
+    event.cancel = true;
+    const source = event.damageSource;
+    system.run(() => {
+      if (!player.isValid) return;
+      rageReapplication.add(player.id);
+      // Resistance II handles 40%; 5/6 × 60% = 50% before armour.
+      player.applyDamage(event.damage * (5 / 6), { cause: source.cause, damagingEntity: source.damagingEntity });
+      rageReapplication.delete(player.id);
+    });
+  });
+
+  world.afterEvents.entityDie.subscribe((event) => {
+    if (event.deadEntity.typeId === "minecraft:player") cleanupPlayer(event.deadEntity);
+  });
+  world.afterEvents.playerLeave.subscribe((event) => { activeFrenzyPlayers.delete(event.playerId); deleteState(event.playerId); });
+
+  // One 1 Hz maintenance cadence serves only online players. It advances finite
+  // states, expires Bloodlust, detects broken sets, and refreshes the action bar.
+  system.runInterval(() => {
+    const tick = system.currentTick;
+    for (const player of world.getAllPlayers()) {
+      const state = getState(player);
+      if (!hasFullAresSet(player)) resetBloodlust(player, state);
+      if (advance(state.rage, tick, ARES.abilities.rage.cooldownTicks)) endRage(player, state);
+      if (advance(state.frenzy, tick, ARES.abilities.frenzy.cooldownTicks)) endFrenzy(player, state);
+      if (state.bloodlustExpiresAt && tick >= state.bloodlustExpiresAt) resetBloodlust(player, state);
+      if (hasFullAresSet(player) || state.rage.active || state.frenzy.active || state.rage.cooldownEndsAt > tick || state.frenzy.cooldownEndsAt > tick) updateActionBar(player, tick);
+    }
+  }, 20);
+
+  // Cobweb traversal has no Bedrock "Weaving" effect or collision API. This
+  // narrowly scoped loop only touches players in their 30-second Frenzy window.
+  system.runInterval(() => {
+    for (const [id, player] of activeFrenzyPlayers) {
+      if (!player.isValid || !getState(player).frenzy.active) { activeFrenzyPlayers.delete(id); continue; }
+      for (const y of [0, 1]) {
+        const block = player.dimension.getBlock({ x: Math.floor(player.location.x), y: Math.floor(player.location.y) + y, z: Math.floor(player.location.z) });
+        if (block?.typeId === "minecraft:cobweb") block.setType("minecraft:air");
+      }
+    }
+  }, 1);
+}
diff --git a/behavior_pack/scripts/core/actionBar.js b/behavior_pack/scripts/core/actionBar.js
new file mode 100644
index 0000000000000000000000000000000000000000..c3995da175c8998739175167c507468c7a8add73
--- /dev/null
+++ b/behavior_pack/scripts/core/actionBar.js
@@ -0,0 +1,8 @@
+import { getState } from "./playerState.js";
+import { remaining } from "./cooldownManager.js";
+function display(ability, tick) { const left = Math.ceil(remaining(ability, tick) / 20); return ability.active ? `ACTIVE ${left}s` : left ? format(left) : "READY"; }
+function format(seconds) { return seconds >= 60 ? `${Math.floor(seconds / 60)}m ${seconds % 60}s` : `${seconds}s`; }
+export function updateActionBar(player, tick) {
+  const state = getState(player);
+  player.onScreenDisplay.setActionBar(`⚔ Rage: ${display(state.rage, tick)} | 🪓 Frenzy: ${display(state.frenzy, tick)} | 🔥 Bloodlust: ${state.bloodlustStacks}/20`);
+}
diff --git a/behavior_pack/scripts/core/cooldownManager.js b/behavior_pack/scripts/core/cooldownManager.js
new file mode 100644
index 0000000000000000000000000000000000000000..125e83c2268f75ac9744eaf178ce8cc3f9f22e57
--- /dev/null
+++ b/behavior_pack/scripts/core/cooldownManager.js
@@ -0,0 +1,11 @@
+// Tick timestamps make delayed cooldowns deterministic: activation starts a
+// duration, and only the duration transition starts the configured cooldown.
+export function isReady(ability, tick) { return !ability.active && tick >= ability.cooldownEndsAt; }
+export function activate(ability, tick, durationTicks) { ability.active = true; ability.endsAt = tick + durationTicks; }
+export function advance(ability, tick, cooldownTicks) {
+  if (!ability.active || tick < ability.endsAt) return false;
+  ability.active = false; ability.endsAt = 0; ability.cooldownEndsAt = tick + cooldownTicks;
+  return true;
+}
+export function reset(ability) { ability.active = false; ability.endsAt = 0; ability.cooldownEndsAt = 0; }
+export function remaining(ability, tick) { return ability.active ? ability.endsAt - tick : Math.max(0, ability.cooldownEndsAt - tick); }
diff --git a/behavior_pack/scripts/core/debug.js b/behavior_pack/scripts/core/debug.js
new file mode 100644
index 0000000000000000000000000000000000000000..dce2e8a667f108643a41de5073daa43e640f8077
--- /dev/null
+++ b/behavior_pack/scripts/core/debug.js
@@ -0,0 +1,2 @@
+export const DEBUG = false;
+export function debug(message) { if (DEBUG) console.warn(`[Pantheon] ${message}`); }
diff --git a/behavior_pack/scripts/core/equipment.js b/behavior_pack/scripts/core/equipment.js
new file mode 100644
index 0000000000000000000000000000000000000000..12353533b969920cc77ba92de9cac7ea15011821
--- /dev/null
+++ b/behavior_pack/scripts/core/equipment.js
@@ -0,0 +1,16 @@
+import { EquipmentSlot } from "@minecraft/server";
+import { ARES } from "./registry.js";
+
+// EquippableComponent is queried only on hits and once per second for players
+// already tracked by the UI; this avoids scanning all entities each game tick.
+export function hasFullAresSet(player) {
+  const equipment = player.getComponent("minecraft:equippable");
+  if (!equipment) return false;
+  return equipment.getEquipment(EquipmentSlot.Head)?.typeId === ARES.equipment.helmet &&
+    equipment.getEquipment(EquipmentSlot.Chest)?.typeId === ARES.equipment.chestplate &&
+    equipment.getEquipment(EquipmentSlot.Legs)?.typeId === ARES.equipment.leggings &&
+    equipment.getEquipment(EquipmentSlot.Feet)?.typeId === ARES.equipment.boots;
+}
+export function heldItemIs(player, itemId) {
+  return player.getComponent("minecraft:equippable")?.getEquipment(EquipmentSlot.Mainhand)?.typeId === itemId;
+}
diff --git a/behavior_pack/scripts/core/playerState.js b/behavior_pack/scripts/core/playerState.js
new file mode 100644
index 0000000000000000000000000000000000000000..36035848784f77aeabf6a2e345e2eebed5b91f8a
--- /dev/null
+++ b/behavior_pack/scripts/core/playerState.js
@@ -0,0 +1,16 @@
+// State is keyed by Player.id (a runtime-stable identity), never by a global
+// "current player". Records contain only primitives, so no disconnected entity is retained.
+const records = new Map();
+export function getState(player) {
+  let state = records.get(player.id);
+  if (!state) {
+    state = { bloodlustStacks: 0, bloodlustExpiresAt: 0,
+      rage: { active: false, endsAt: 0, cooldownEndsAt: 0 },
+      frenzy: { active: false, endsAt: 0, cooldownEndsAt: 0 } };
+    records.set(player.id, state);
+  }
+  return state;
+}
+export function deleteState(playerId) { records.delete(playerId); }
+export function clearState(player) { records.delete(player.id); }
+export function entries() { return records.entries(); }
diff --git a/behavior_pack/scripts/core/registry.js b/behavior_pack/scripts/core/registry.js
new file mode 100644
index 0000000000000000000000000000000000000000..f70b4da00c7f80dae037a0153064ede1fe154400
--- /dev/null
+++ b/behavior_pack/scripts/core/registry.js
@@ -0,0 +1,13 @@
+// The registry is the single source of truth for divine equipment. Future gods
+// are added here, allowing core systems to remain god-agnostic.
+export const ARES = {
+  id: "ares",
+  equipment: {
+    helmet: "pantheon:ares_helmet", chestplate: "pantheon:ares_chestplate",
+    leggings: "pantheon:ares_leggings", boots: "pantheon:ares_boots",
+    saber: "pantheon:ares_saber", axe: "pantheon:ares_axe",
+  },
+  abilities: { rage: { durationTicks: 400, cooldownTicks: 1200 }, frenzy: { durationTicks: 600, cooldownTicks: 3600 } },
+};
+export const DIVINE_EQUIPMENT = new Set(Object.values(ARES.equipment));
+export function isDivineEquipment(item) { return Boolean(item && DIVINE_EQUIPMENT.has(item.typeId)); }
diff --git a/behavior_pack/scripts/main.js b/behavior_pack/scripts/main.js
new file mode 100644
index 0000000000000000000000000000000000000000..f6ddb0cca4f91f344756ecffa2fc056de9b29cab
--- /dev/null
+++ b/behavior_pack/scripts/main.js
@@ -0,0 +1,6 @@
+import { system } from "@minecraft/server";
+import { registerAres } from "./ares/ares.js";
+
+// Deferring one tick ensures world event subscriptions are registered after the
+// behavior pack'\''s script module has initialized.
+system.run(registerAres);
diff --git a/dist/The-Pantheon-v0.1.0-BETA.mcaddon b/dist/The-Pantheon-v0.1.0-BETA.mcaddon
new file mode 100644
index 0000000000000000000000000000000000000000..c7319c446c1ad6939783f49efd00b0e8180354b3
GIT binary patch
literal 9219
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

literal 0
HcmV?d00001

diff --git a/resource_pack/attachables/ares_boots.json b/resource_pack/attachables/ares_boots.json
new file mode 100644
index 0000000000000000000000000000000000000000..9dc23b1a4121dee87d993fedef9438807bab7d78
--- /dev/null
+++ b/resource_pack/attachables/ares_boots.json
@@ -0,0 +1,20 @@
+{
+  "format_version": "1.10.0",
+  "minecraft:attachable": {
+    "description": {
+      "identifier": "pantheon:ares_boots",
+      "materials": {
+        "default": "armor"
+      },
+      "textures": {
+        "default": "textures/models/armor/netherite_1"
+      },
+      "geometry": {
+        "default": "geometry.humanoid.armor.boots"
+      },
+      "render_controllers": [
+        "controller.render.armor"
+      ]
+    }
+  }
+}
diff --git a/resource_pack/attachables/ares_chestplate.json b/resource_pack/attachables/ares_chestplate.json
new file mode 100644
index 0000000000000000000000000000000000000000..dd08f7ef2b849de22181774274b9b297336279a1
--- /dev/null
+++ b/resource_pack/attachables/ares_chestplate.json
@@ -0,0 +1,20 @@
+{
+  "format_version": "1.10.0",
+  "minecraft:attachable": {
+    "description": {
+      "identifier": "pantheon:ares_chestplate",
+      "materials": {
+        "default": "armor"
+      },
+      "textures": {
+        "default": "textures/models/armor/netherite_1"
+      },
+      "geometry": {
+        "default": "geometry.humanoid.armor.chestplate"
+      },
+      "render_controllers": [
+        "controller.render.armor"
+      ]
+    }
+  }
+}
diff --git a/resource_pack/attachables/ares_helmet.json b/resource_pack/attachables/ares_helmet.json
new file mode 100644
index 0000000000000000000000000000000000000000..81ff56d1f84803578a560fc6a5eb8922a5d4de96
--- /dev/null
+++ b/resource_pack/attachables/ares_helmet.json
@@ -0,0 +1,20 @@
+{
+  "format_version": "1.10.0",
+  "minecraft:attachable": {
+    "description": {
+      "identifier": "pantheon:ares_helmet",
+      "materials": {
+        "default": "armor"
+      },
+      "textures": {
+        "default": "textures/models/armor/netherite_1"
+      },
+      "geometry": {
+        "default": "geometry.humanoid.armor.helmet"
+      },
+      "render_controllers": [
+        "controller.render.armor"
+      ]
+    }
+  }
+}
diff --git a/resource_pack/attachables/ares_leggings.json b/resource_pack/attachables/ares_leggings.json
new file mode 100644
index 0000000000000000000000000000000000000000..7b0e578687020caa7e053795d8e47aa91fb67865
--- /dev/null
+++ b/resource_pack/attachables/ares_leggings.json
@@ -0,0 +1,20 @@
+{
+  "format_version": "1.10.0",
+  "minecraft:attachable": {
+    "description": {
+      "identifier": "pantheon:ares_leggings",
+      "materials": {
+        "default": "armor"
+      },
+      "textures": {
+        "default": "textures/models/armor/netherite_2"
+      },
+      "geometry": {
+        "default": "geometry.humanoid.armor.leggings"
+      },
+      "render_controllers": [
+        "controller.render.armor"
+      ]
+    }
+  }
+}
diff --git a/resource_pack/manifest.json b/resource_pack/manifest.json
new file mode 100644
index 0000000000000000000000000000000000000000..5269c43a8ec8587b0e9b9db956692e3f6fcad47a
--- /dev/null
+++ b/resource_pack/manifest.json
@@ -0,0 +1,6 @@
+{
+  "format_version":2,
+  "header":{"name":"The Pantheon: Echoes of the Divine RP","description":"Vanilla placeholder visuals for The Pantheon v0.1.0 BETA","uuid":"31fae8e0-e0f4-4177-a85a-7ea7bbd41115","version":[0,1,0],"min_engine_version":[1,21,110]},
+  "modules":[{"type":"resources","uuid":"b3fcc479-c543-47db-a62c-78e472c57a4e","version":[0,1,0]}],
+  "metadata":{"authors":["The Pantheon"],"product_type":"addon"}
+}
diff --git a/resource_pack/textures/item_texture.json b/resource_pack/textures/item_texture.json
new file mode 100644
index 0000000000000000000000000000000000000000..042483d660a69c5936830df8cd3cf0cbb7bbacce
--- /dev/null
+++ b/resource_pack/textures/item_texture.json
@@ -0,0 +1 @@
+{"resource_pack_name":"pantheon","texture_name":"atlas.items","texture_data":{"pantheon_ares_saber":{"textures":"textures/items/stick"},"pantheon_ares_axe":{"textures":"textures/items/netherite_axe"},"pantheon_ares_helmet":{"textures":"textures/items/netherite_helmet"},"pantheon_ares_chestplate":{"textures":"textures/items/netherite_chestplate"},"pantheon_ares_leggings":{"textures":"textures/items/netherite_leggings"},"pantheon_ares_boots":{"textures":"textures/items/netherite_boots"}}}
' | git apply --3way)
