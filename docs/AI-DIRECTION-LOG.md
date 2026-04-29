# AI Direction Log
**Project:** Ritual — Daily Habit Tracker  
**Course:** AI 201 — Creative Computing with AI  
**Student:** Valeria Munera  

---

## What This Log Is

This document records my editorial relationship with Claude across the build of Ritual. Each entry documents what I asked, what the AI produced, what I changed, and why. The log is evidence that I was the creative director — not a passive recipient of generated code.

---

## Entry 1 — Architectural Decision: Vanilla JS over React

**Session:** Project kickoff  
**What I asked:** Build a habit tracker with persistent state and Firebase sync that works as a PWA installable on iPhone.  
**What AI produced:** Initially suggested a React + Vite setup with component-based state management.  
**What I changed:** I rejected React and directed Claude to use vanilla JS in a single `index.html` file. React would have required a build pipeline incompatible with GitHub Pages' static hosting, and the PWA install flow works more reliably without a framework adding service worker complexity.  
**Why it matters:** This was the single most important architectural decision of the project. The single `state` object with `save()` → `updateAll()` loop I directed Claude to build is architecturally equivalent to React's lifted state + re-render cycle — but deployable directly to GitHub Pages with no build step.

---

## Entry 2 — State Model: One Object, One Truth

**Session:** Early development  
**What I asked:** All user data — habits, sections, rewards, points, streak, completion history, challenges — must live in a single `state` object. No panel should manage its own copy of any data.  
**What AI produced:** Initially created separate local variables in different rendering functions for things like points and streak display.  
**What I changed:** I directed Claude to consolidate everything into one `state` object and ensure that every render function reads exclusively from `state`, never from local variables. I also specified that `updateAll()` must be the only function allowed to trigger re-renders, so every user action flows through: action → `save()` → `updateAll()`.  
**Why it matters:** This enforces the single source of truth principle the assignment is built around. At any moment, `console.log(state)` tells you the complete truth about the app.

---

## Entry 3 — The Three-Panel Mapping

**Session:** Mid-development  
**What I asked:** Design the Today page (Browser), Progress page (Detail View), and Habit Manager (Controller) so that they behave as connected panels sharing one state.  
**What AI produced:** Built each page as an independent rendering function with its own data-fetching logic.  
**What I changed:** I directed Claude to wire all three through `updateAll()` — one call re-renders all panels simultaneously. I explicitly rejected any design where a panel "fetched" its own data. Every panel is a pure read from `state`.  
**Why it matters:** Cross-panel reactivity is the core grading criterion. Completing a habit on Today immediately updates the streak counter on Progress. Editing a habit in the Manager immediately changes what Today shows. This is the assignment's "click in one panel, others respond" requirement.

---

## Entry 4 — Firebase as Persistence Layer

**Session:** Mid-development  
**What I asked:** Sync `state` to Firestore so data persists across sessions and devices.  
**What AI produced:** A Firebase integration that re-fetched data from Firestore on every render.  
**What I changed:** I directed Claude to treat Firestore as a write-through cache, not the source of truth. State is loaded once from Firestore on auth. After that, all reads come from the local `state` object. Firestore writes happen on every `save()`. This keeps the UI fast (no async render blocking) while keeping data safe.  
**Why it matters:** Latency. If panels waited for Firestore reads before rendering, cross-panel updates would feel slow and broken. The in-memory `state` object is the fast single source of truth; Firestore is the durable backup.

---

## Entry 5 — Onboarding Architecture

**Session:** Late development  
**What I asked:** Build a multi-step onboarding flow that collects theme preference, display name, habit categories, initial habits, and rewards — then saves all of it to Firestore in one write.  
**What AI produced:** An onboarding flow that saved to Firestore after every individual step.  
**What I changed:** I directed Claude to accumulate selections in memory across all steps and batch-save everything at the end. This prevents partial saves if a user quits mid-onboarding and reduces Firestore write costs.  
**Why it matters:** Data integrity. A user should either complete onboarding or not — no half-initialized state in the database.
