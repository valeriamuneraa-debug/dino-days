# AI Direction Log
**Project:** Ritual — Daily Habit Tracker  
**Course:** AI 201 — Creative Computing with AI  
**Student:** Valeria Munera  

---

## What This Log Is

This document records my editorial relationship with Claude across the full build of Ritual. Each entry documents what I asked, what AI produced, what I changed, and why. This log is evidence of my role as creative director throughout the project — not a passive recipient of generated code.

---

## Entry 1 — Architectural Decision: Vanilla JS Over React

**What I asked:** Build a habit tracker with real user accounts, persistent data, and PWA install support for iPhone.  
**What AI produced:** An initial suggestion for a React + Vite setup with useState hooks and a component-based file structure.  
**What I changed:** I rejected React entirely. I directed Claude to build in vanilla JS inside a single `index.html` with a global `state` object, a `save()` function, and an `updateAll()` render loop. React + Vite requires a build step that breaks GitHub Pages static hosting unless you configure a full deployment pipeline. My app needed to be pushable with one `git push` and browsable instantly on an iPhone via Safari.  
**Why it matters:** This was the most important architectural decision of the project. The pattern I directed — one object owns everything, one function re-renders everything — is the same single-source-of-truth concept the assignment teaches. It's just more legible in vanilla JS. You can `console.log(state)` and see the entire truth of the app in one place.

---

## Entry 2 — Replacing "Task" With "Habit" Everywhere in the UI

**What I asked:** Replace every user-visible instance of "task" or "tasks" with "habit" or "habits" — this is not a to-do list.  
**What AI produced:** A find-and-replace that also renamed internal JS function names like `showAddTaskModal()`, CSS class names like `.task-item`, and HTML ids like `#tasks-container`.  
**What I changed:** I caught this immediately and directed Claude to only touch user-visible string literals — button labels, headings, toast messages, placeholder text — and leave all function names, class names, and variable names untouched. The internal code still uses `state.tasks` and `renderTasks()` because those are not user-facing.  
**Why it matters:** Renaming internal identifiers mid-project breaks all the other code that references them. This is a classic AI over-scoping error. The UI layer and the code layer are separate concerns.

---

## Entry 3 — The Pencil Icon vs. The Plus Icon — Separating Two Functions

**What I asked:** The pencil icon at the top of the Today screen should open a full habit management view where I can reorder habits, bulk-select, and delete. The plus icon at the bottom should open the "add a single new habit" modal. These are two separate actions.  
**What AI produced:** Both icons opening the same "Add Habit" modal.  
**What I changed:** I directed Claude to split this into two distinct flows — the pencil opens a Task Manager with drag-to-reorder, bulk selection, section management, and delete. The plus is a quick-add form only. I also later directed Claude to merge the reorder and bulk edit views into one screen because having them separate was unnecessary complexity.  
**Why it matters:** UX clarity. A user bulk-editing their habits is in a management mindset. A user tapping plus is in a quick-add mindset. They should never arrive at the same destination.

---

## Entry 4 — The Pivot: Removing the Dinosaur and Replacing With Challenges

**What I asked (originally):** Build a virtual pet dinosaur whose health reflects habit consistency. Users can name it, customize its color, and it dies if they miss too many days.  
**What AI produced:** A full pet system with hearts (3-heart life system), fainting states, a rewards lockout when the dino fainted, dino naming during onboarding, and an animated SVG dinosaur.  
**What I changed:** After iterating on the pet system for several sessions — including building hearts, light/dark mode color changes, and emotional revival mechanics — I made the executive decision to remove the dinosaur entirely. It was sitting there passively and not creating real motivation. I redirected Claude to replace the entire Dino tab with a Challenges system: solo challenges (21-day, 30-day, 90-day) and social challenges with friends in Race mode (competitive streaks) and Bond mode (shared streak). I also renamed the app from "Dino Days" to "Ritual."  
**Why it matters:** This is the biggest creative pivot of the project. I was the one who introduced the dino and I was the one who killed it. AI had no opinion — it built both faithfully. The decision came from me watching users (including myself) interact with the pet and realizing the emotional hook wasn't landing. A competitive challenge with a real friend is a stronger motivation loop than a passive mascot.

---

## Entry 5 — Onboarding Flow Design and Content Direction

**What I asked:** Build a 6-screen onboarding that teaches users what the app is while personalizing their experience. The flow: (1) choose a color theme, (2) enter display name, (3) pick habit categories, (4) select first habits from suggestions, (5) set up rewards, (6) "You're all set" closer screen followed by a spotlight tour.  
**What AI produced:** An onboarding that led with a welcome message, skipped the theme picker, and dumped all setup into a single long scrollable form.  
**What I changed:** I specified each screen individually, directed the exact heading and subtext copy for each, specified that screens should be discrete steps with a dot indicator, required back-arrow navigation on every screen, required a "skip" option below every continue button, and directed the "You're all set" closer to use the app name "Ritual" in Fraunces at 52px as the emotional beat instead of any feature explanation.  
**Why it matters:** Onboarding is the first impression. I didn't want users to feel like they were filling out a form. Each screen should feel like a small decision, not a step in a checkout flow.

---

## Entry 6 — Pointing Value Per Habit (Customizable)

**What I asked:** Every habit should have a customizable point value. The default is 5. Users should be able to change it when adding or editing a habit.  
**What AI produced:** A hardcoded 5 points per completion everywhere `toggleTask()` was called, with no per-habit customization.  
**What I changed:** I directed Claude to add a `points` property to each habit object in `state.tasks`, add a number input field to both the Add Habit and Edit Habit modals, and update `toggleTask()` to read `task.points` instead of the hardcoded value. I also specified that loading old habits from Firestore without a `points` property should default to 5 for backward compatibility.  
**Why it matters:** The state model needed to evolve to carry more information per item. This is a clean example of the data model driving UI — the `points` property in the JSON object is what makes the input field meaningful.

---

## Entry 7 — Social Features: Friends, Race Mode, Bond Mode, Custom Mode

**What I asked:** Add a friends system where users can add each other by unique user codes. Then add social challenges: Race mode (both friends build independent streaks toward the same goal, first to finish wins) and Bond mode (shared streak — if either person misses a day, both reset).  
**What AI produced:** The full social architecture using two Firestore collections (`profiles` for public data, `socialChallenges` for challenge documents), real-time listeners using `onSnapshot`, friend request flows with accept/decline, and competitive/cooperative logic.  
**What I changed:** I directed the specific data model shape for each challenge type, including `sharedStreak` for Bond mode vs. independent `currentStreak` per participant for Race mode. I also directed the addition of Custom mode (user-defined rules: allow misses, total days vs. streak-based victory conditions). I specified the celebration overlay on win, the bond medal system, and the winner determination logic.  
**Why it matters:** Social features are the core motivation loop I pivoted to after removing the dinosaur. Having two friends in a Bond challenge where one person's miss hurts both is dramatically more motivating than a solo virtual pet. This was a creative direction decision that shaped the entire second half of the app's development.

---

## Entry 8 — Rewards System Design

**What I asked:** Users earn points by completing habits and spend them on "pleasures" — personal rewards they define themselves. The rewards store should lock if they miss too many days.  
**What AI produced:** A rewards page with a hardcoded list of generic rewards and no lockout mechanic.  
**What I changed:** I directed the full rewards architecture: user-defined rewards stored in `state.rewards`, a purchase history stored in `state.purchaseHistory`, a paginated "load more" pattern for history (showing 5 at a time), an undo-redeem feature, and the lockout logic tied to the dino faint state (later: tied to streak breaks). I also directed the onboarding to include a rewards setup screen with popular suggestions as tappable chips.  
**Why it matters:** Rewards are the behavioral reinforcement loop. If users can set their own rewards, the system means something personal to them. Takeout, spa day, movie night — these are real things people want. The point value they assign is their own valuation.

---

## Entry 9 — Progress Tab: Weekly Navigation and Habit-Specific History

**What I asked:** The Progress tab should show which specific habits were completed on each day, not just a dot indicating "did something." Users should also be able to go back to previous weeks using arrows.  
**What AI produced:** A progress view showing only a completion dot per day with no drill-down, and no navigation beyond the current week.  
**What I changed:** I directed Claude to add previous-week navigation with a `weekOffset` variable (the current week is 0, previous week is -1, etc.), left/right arrow controls with disabled states at the bounds, and a detailed habit completion view when tapping a day dot. I also directed the fix for a bug where the week navigation wasn't resetting properly and was locking users on the current week.  
**Why it matters:** The Progress tab is the Detail View panel of the assignment architecture. It only reads from state — it never writes. But "reads" means showing real detail, not just aggregate dots. The specific habit list per day is what makes the progress view actually useful for reflection.

---

## Entry 10 — Per-Day Habit Scheduling

**What I asked:** Users should be able to set which days of the week a habit applies. Default is all 7 days. A habit set to "Monday, Wednesday, Friday" should only appear in the Today view on those days.  
**What AI produced:** All habits appearing every day regardless of their `days` array.  
**What I changed:** I directed Claude to add a days-of-week selector (7 pill buttons) to the Add and Edit Habit modals, store the selection as an array of day indices in `state.tasks[i].days`, and update `renderTasks()` to filter habits by comparing `task.days` against the current day of the week. I also directed the fix for a persistent bug where habits with specific day settings were still appearing on wrong days.  
**Why it matters:** Habit scheduling is core user need. Someone who goes to the gym only on weekdays shouldn't see their gym habit on Saturday. The `days` array is a first-class field in the data model, not an afterthought.
