# Records of Resistance
**Project:** Ritual — Daily Habit Tracker  
**Course:** AI 201 — Creative Computing with AI  
**Student:** Valeria Munera  

---

## What This Document Is

Documented moments where I rejected or significantly revised what Claude produced. Each entry names what AI gave me, what I did instead, and why. These are the moments where creative direction actually happened.

---

## Resistance 1 — Rejecting the Dinosaur Entirely After Building It

**What AI produced:** A full virtual pet system — animated SVG dinosaur, 3-heart life mechanic, fainting states that locked the rewards store, light/dark mode color interpolation for the dino's health state, a revival mechanic requiring 100% habit completion to bring the pet back, a naming screen in onboarding, and customizable dino colors. Claude built all of this faithfully over multiple sessions because I asked for it.  
**What I did instead:** I scrapped the entire pet system. I directed Claude to remove the dinosaur SVG, remove all heart logic, remove the faint-based rewards lockout, remove the dino naming screen from onboarding, and replace the entire Dino tab with a Challenges system. I also renamed the app from "Dino Days" to "Ritual."  
**Why:** I watched myself use the app and realized I felt nothing toward the dinosaur. It was sitting there, bouncing slightly, doing nothing. A virtual pet works in Tamagotchi because you carry it everywhere and it makes noise when it needs you. In a web app you visit once a day to check off habits, a passive mascot doesn't create urgency. The pivot to social challenges — where a real friend's streak depends on whether I show up — is a fundamentally stronger motivation loop. I made this call after the pet system was fully built, which means I also had to absorb the cost of redirecting Claude across multiple sessions to undo and rebuild. That was the right call.

---

## Resistance 2 — Rejecting `position: fixed` for the Desktop Onboarding Overlay

**What AI produced:** When I asked Claude to make the desktop onboarding cover the full app rectangle, it applied `position: fixed` to `#onboarding-overlay`, which caused the overlay to cover the entire browser viewport — URL bar, tab bar, everything outside the app. The rounded rectangle that is the app's visual identity completely disappeared.  
**What I did instead:** I rejected this across three separate sessions where Claude kept reverting to `position: fixed`. I specified that `#app` must have `position: relative` and the overlay must use `position: absolute` with `inset: 0`, so it is contained within and inherits the border-radius of its parent. The overlay should cover the rounded rectangle — not the browser.  
**Why:** The contained rounded card on desktop is a deliberate brand decision. The app looks like an object sitting on your desktop, not a website. When onboarding breaks out of that container and takes over the full browser, it destroys the design language. I don't care that `position: fixed` is simpler — it's wrong for this app. The art director call here was: the container is part of the identity. It doesn't disappear.

---

## Resistance 3 — Rejecting AI's Three-Difficulty-Mode Proposal

**What AI produced:** When I asked whether a difficulty system was a good idea (Easy/Medium/Hard modes with different point and life rules), Claude said yes and provided a fully fleshed-out implementation: three selectable modes in settings, different point accumulation rules per mode, different heart-loss thresholds, and a mode-locked state that prevented switching mid-streak.  
**What I did instead:** I rejected the implementation entirely and did not ship difficulty modes. I kept a single consistent rule set for all users.  
**Why:** The professor asked "or is it too overwhelming for users?" in my annotation and I concluded yes. Adding difficulty modes creates three separate mental models users have to understand before they even start using the app. The onboarding was already complex enough. More importantly, the habit of showing up consistently is the same regardless of difficulty — the app should reinforce that, not turn it into a game difficulty setting. This was a case where AI was too agreeable. It said "yes" to my question and immediately started building. I had to be the one to say "actually no."

---

## Resistance 4 — Rejecting Hardcoded Section Names

**What AI produced:** Throughout early development, the Add Habit and Edit Habit modals had hardcoded dropdown options for "Morning," "General," and "Night" — regardless of what sections the user had actually created during onboarding or in the Task Manager.  
**What I did instead:** I identified this as a state duplication bug — the section data existed in `state.sections` but the modal wasn't reading from it. I directed Claude to replace every hardcoded section dropdown with a dynamic loop over `state.sections`, generating options from the user's actual data. I also directed this fix in the bulk-edit "Move selected to..." dropdown in the Task Manager.  
**Why:** This is exactly the kind of bug the assignment exists to teach you to avoid. The sections are defined in one place (`state.sections`). Any UI that shows sections must read from that one place. Having hardcoded options in modals while `state.sections` held different data meant the same information existed in two places and could disagree. That's the definition of a state duplication bug.

---

## Resistance 5 — Rejecting a Centered-Screen Tour Tooltip

**What AI produced:** When I asked Claude to fix the spotlight tour tooltip positioning, it proposed anchoring the tooltip to `position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%)` — always centered in the viewport regardless of which element was highlighted.  
**What I did instead:** I rejected the centered approach and directed anchoring the tooltip to the right of the highlighted element using `getBoundingClientRect()`, with clamping logic to prevent overflow off-screen edges.  
**Why:** A spotlight tour's entire purpose is to connect your eye to an element and then explain what it does. If the tooltip is floating in the center of the screen while the glow ring is on a sidebar item in the upper left, the user has to look in two places and mentally connect them. That breaks the UX of the feature. Claude's suggestion was technically simpler and avoided edge cases, but it produced a fundamentally worse user experience. Simple is only good when it serves the goal.

---

## Resistance 6 — Rejecting Emoji Icons and Directing to Custom SVGs

**What AI produced:** Throughout the app, navigation icons in the sidebar and action icons in headers were rendered using phone emojis (📊 for Progress, 🎯 for Challenges, 🏆 for Rewards).  
**What I did instead:** I directed Claude to replace emoji icons with custom inline SVGs across the UI — line-art icons consistent with the minimal aesthetic of the app.  
**Why:** Emoji icons render differently across operating systems. On macOS they look one way, on Windows another, on Android another. For a design-forward app with a carefully chosen type palette (Fraunces + DM Sans) and a warm linen color system, having bright colorful cartoon emojis as navigation icons was visually inconsistent. Custom SVGs render identically everywhere and can be styled with CSS variables to match the theme. This was a visual identity decision, not just a preference.

---

## Resistance 7 — Rejecting Rewards-Store Lockout Tied to the Dino

**What AI produced:** When the dinosaur fainted (0 hearts), the entire rewards store was locked with a dimmed overlay and a message telling the user to revive their dino before spending points. This was tied to `state.dinoFainted`.  
**What I did instead:** After removing the dinosaur, I directed Claude to remove the rewards lockout entirely. The rewards store is always accessible.  
**Why:** A lockout that prevents users from spending points they legitimately earned is a punitive mechanic that creates frustration, not motivation. The habit system already has enough consequence built in — missed days don't earn points, challenges reset, friends' streaks get affected. Adding a second layer of punishment where you can't access your own rewards is bad UX. It was designed around the pet logic which no longer existed. Without the emotional stakes of the pet, the lockout was just annoying.
