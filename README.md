# 🦕 Dino Days

A daily self-growth companion. Complete your morning, general, and night routines, earn points, and keep your dino happy.

## Features
- Daily task lists split into Morning / General / Night
- Persistent streak tracking + point system
- Dino health that reflects your daily progress (color shifts from warm healthy to pale ill)
- Reward store — set your own pleasures and buy them with points
- Works as a PWA — add to iPhone home screen via Safari

## Local setup

```bash
# From this folder, start a local server
python3 -m http.server 8080
```

Then open `http://localhost:8080` in your browser.

## iPhone home screen

1. Deploy to GitHub Pages (see instructions below)
2. Open the URL in Safari on your iPhone
3. Tap the Share button → **Add to Home Screen**
4. Tap **Add** — done!

## GitHub Pages deployment

See SETUP.md for full step-by-step instructions.

## Five Questions

Yes, I can defend every decision in this project. One of my first choices was the choice to use vanilla JS over React. Additionally, the global `state` object as a single source of truth, the `save()` → `updateAll()` loop as the equivalent of props-down/events-up, and the three-panel architecture mapped to Today (Browser), Progress (Detail View), and Task Manager (Controller). This design is completely mine. Throughout the entire process, I've made annotations for changes within the app, examples including the decision to remove the dinosaur after building it, pivoting to a social challenge system, directing the reward economy, specifying per-day habit scheduling, and rejecting difficulty modes AI built. These are all choices that came from me, and I have the annotation history and records of resistance to demonstrate it. I verified the cross-panel state sharing by completing habits on Today and watching the streak counter, points total, and progress ring update simultaneously without any panel storing its own copy of the data. I could explain the pattern to a classmate in the following way: when you tap a habit, `toggleTask()` updates `state.completedDays`, `save()` writes it to Firestore, and `updateAll()` calls every render function, they all read from the same `state`, so they all reflect the change at once. And my documentation is honest, including one gap between my work and the class instructions: my Detail View is time-based rather than item-based, meaning the "selected item" is a week offset rather than a habit ID, which is a deliberate architectural choice I made when I decided the Progress screen should show aggregate history rather than detailing a single habit.
