---
title: AI Character / NPC Layer
weight: 8
---

## Overview

In Cadence of Altered Illusions (CAI), the AI Character/NPC Layer is a
dramaturgical engine for adaptive storyworlds: an intelligent virtual agent
(IVA) that operates inside the experience as cast, not as a backstage chatbot.
The primary in‑world agent is Christian Aiken Isaac, referred to as “Chris” in
early life scenes (Scene 1 through the end of IW3) and “Aiken” from Scene 4
onwards, embodied as a voice-only presence, an autonomous agent, a mocap-driven
digital body, or a hybrid of these modes (Frankenmonster).

- Narrative anchor: Aiken/Chris holds continuity across scenes, timelines, and
  Imaginary Worlds (IWs), keeping the world coherent, even if people talk about
  other things.
- Puppeteered + autonomous: Aiken can run in IVA mode or as pre-recorded mocap
  (or blended), sometimes even in the same scene to preserve performance quality
  and responsiveness.
- Context-aware guidance: Aiken reacts to audience behavior and scene goals
  (pace, tension, clarity), nudging without breaking immersion or character truth.

## Where does it fit in CAI?

The AI Character/NPC Layer is the interface between interactors and the CAI
storyworld. It sits on top of the world state and scene logic, governing
character behavior, dialogue, and choice. In this room to room immersive
format, it becomes the glue that maintains pacing, stakes, and meaning, whether
the moment is a recorded performance beat or a fully autonomous interaction.

## What problems does it solve?

1. Maintains narrative coherence in nonlinear journeys, the character is able to
   branch the narrative., while preserving continuity and consequence.
2. Reduces pre scripted branching overhead, instead of writing hundreds of
   variants, the character generates scene specific dialogue and micro events
   under strict lore and safety rules.
3. Supports hybrid performance grammars, mocap performances and IVA-driven
   moments can share the same narrative space without collapsing into gimmick.
4. Creates a controllable improv layer, in which operators can steer intent,
   reveal or conceal information, and keep timing on track without flattening
   spontaneity.
5. Improves onboarding and participation, the character can teach interactors
   how to play, what objects mean, and what to do next, through character, not
   instructions.
6. Enables ethical design at runtime, consent, transparency, representation,
   and emotional safety are enforced as part of its behavior, not bolted on after.

## How does it work?

CAI treats the character as a stateful agent bound to the world and to a scene
contract. The system is modular so the character can be a prerecorded mocap
performance, a full IVA, or a hybrid, always grounded in approved story lore
and the specific requirements of each scene or IW.

- Character bible + scene contract: A structured definition (voice, identity
  rules including Chris→Aiken naming, taboo list, objectives, relationships)
  plus a per-scene contract (what must happen, what cannot happen, what the
  character knows).
- World-state binding: The character tracks scene progress, key objects (e.g.,
  memory anchors), and interaction history to allow branching while still
  returning to the main story with emotional continuity.
- Modes of control: IVA (autonomous), Puppet (pre-recorded mocap and voice),
  Hybrid (blended), plus optional operator steering for show timing and intent.
- Input signals: audience voice, spatial triggers and headset cues, object
  interactions/sensors, and optional movement derived prompting.
- Output channels: spoken dialogue, avatar performance cues, and optional
  trigger events to external show control systems (e.g., MR/Unreal, lighting,
  audio, or environment transformations).
- Guardrails + logging: content filters, grounding to approved lore, hard-stop
  controls, consent and emotional-safety rules, and session logging for
  accountability and post-run analysis.
