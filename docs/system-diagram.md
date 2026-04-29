# System Diagram — Ritual

**Project:** Ritual — Daily Habit Tracker  
**Course:** AI 201 — Creative Computing with AI  
**Architecture:** Browser → Detail View → Controller sharing a single `state` object

```mermaid
flowchart TD
    subgraph STATE["☁️ Single Source of Truth — state (Firestore)"]
        S1["tasks[ ]"]
        S2["sections[ ]"]
        S3["completedDays{ }"]
        S4["rewards[ ]"]
        S5["points / streak / bestStreak"]
        S6["history[ ]"]
        S7["challenges[ ]"]
        S8["weekOffset"]
    end

    subgraph BROWSER["📋 Browser — Today Screen"]
        B1["renderTasks()
maps over state.tasks
filters by day + section"]
        B2["toggleTask()
writes → state.completedDays
calls save() → updateAll()"]
        B3["filterSection()
writes → cf (active filter)
re-renders task list"]
    end

    subgraph DETAIL["📊 Detail View — Progress Screen"]
        D1["renderProgress()
reads state.streak
reads state.completedDays
reads state.tasks
NEVER writes"]
        D2["Weekly Calendar
reads state.completedDays[dateKey]
weekOffset controls which week shows"]
        D3["Habit Streaks List
reads per-habit streak
read only"]
    end

    subgraph CONTROLLER["⚙️ Controller — Task Manager + Rewards"]
        C1["editTask() / deleteTask()
modifies state.tasks
calls save() → updateAll()"]
        C2["addTask()
pushes to state.tasks
calls save() → updateAll()"]
        C3["buyReward()
decrements state.points
pushes to state.history
calls save() → updateAll()"]
        C4["evaluateChallenges()
modifies state.challenges
calls save() → updateAll()"]
    end

    subgraph SYNC["🔄 Update Loop"]
        U1["save()
writes full state
to Firestore"]
        U2["updateAll()
calls ALL render functions
simultaneously"]
    end

    STATE -->|"props down: data flows to all render functions"| BROWSER
    STATE -->|"props down: read only"| DETAIL
    STATE -->|"props down: current values"| CONTROLLER

    BROWSER -->|"events up: user action"| U1
    CONTROLLER -->|"events up: user action"| U1
    U1 --> U2
    U2 -->|"re-renders"| BROWSER
    U2 -->|"re-renders"| DETAIL
    U2 -->|"re-renders"| CONTROLLER

    subgraph SOCIAL["👥 Social Layer — Firestore listeners"]
        F1["onSnapshot(socialChallenges)
real-time friend challenge updates"]
        F2["profiles collection
public streak / points per user"]
    end

    SOCIAL -->|"updateAll() on change"| U2
```
