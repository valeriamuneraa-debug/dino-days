# Records of Resistance
**Project:** Ritual — Daily Habit Tracker  
**Course:** AI 201 — Creative Computing with AI  
**Student:** Valeria Munera  

---

## What This Document Is

Three documented moments where I rejected or significantly revised what Claude produced. Each entry names what AI gave me, what I did instead, and why. These are not mistakes — they are the moments where my creative direction shaped the project into something mine.

---

## Resistance 1 — Rejecting Full-Screen Onboarding Overlay

**What AI produced:** When I asked Claude to fix the desktop onboarding experience, it interpreted "cover the app" to mean `position: fixed` on the overlay, which covered the entire browser viewport — the URL bar, tab bar, everything outside the app rectangle.  
**What I did instead:** I rejected this and specified `position: absolute` contained within `#app` which has `position: relative`. The overlay must cover only the rounded rectangle that is the app — not the browser chrome. I had to issue this correction across three separate sessions because Claude kept reverting to `position: fixed`.  
**Why:** The design intent of the app is that it lives inside a contained rounded card on desktop. Covering the full viewport broke that design language entirely and looked like a bug, not a feature. The art director decision here was: the container is part of the brand. It doesn't disappear during onboarding.

---

## Resistance 2 — Rejecting React in Favor of Vanilla JS

**What AI produced:** When I described the app I wanted to build, Claude's first architectural suggestion was a React + Vite project with useState hooks, component files, and a build pipeline.  
**What I did instead:** I rejected the entire stack and directed Claude to build in vanilla JS inside a single `index.html` file, with a hand-rolled state management pattern using a global `state` object, a `save()` function, and an `updateAll()` render loop.  
**Why:** React + Vite requires a build step. Build steps break GitHub Pages unless you configure deployment actions. My app needed to be deployable by pushing one file and browsable instantly on an iPhone via Safari. The vanilla JS approach also made the single-source-of-truth architecture more transparent — you can literally `console.log(state)` and see everything. No black box, no virtual DOM, no component hydration to debug. The professor's core concept — one object, every panel reads from it — is more legible in 30 lines of vanilla JS than in three React components with prop drilling.

---

## Resistance 3 — Rejecting AI's Tour Tooltip Positioning

**What AI produced:** When I asked Claude to fix the spotlight tour, it positioned tour tooltips using `position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%)` — always centered in the screen regardless of what was being highlighted.  
**What I did instead:** I rejected the centered approach and directed Claude to anchor tooltips to the RIGHT of the highlighted element using `getBoundingClientRect()`, with clamping logic to prevent going off-screen. The tooltip should feel like it's pointing at something, not floating in the middle of the screen.  
**Why:** The centered tooltip breaks the purpose of a spotlight tour. A spotlight tour works because your eye is drawn to the glow ring on the element, then the tooltip explains what that element does. If the tooltip is floating 400px away from the highlighted element in the center of the screen, the connection between "this glowing thing" and "this explanation" is severed. I made the call that a slightly imperfect anchored tooltip is always better than a perfectly centered disconnected one.
