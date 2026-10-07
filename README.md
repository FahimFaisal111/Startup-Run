# Startup Run 3D: Entrepreneurship Simulator

A comic-styled 3D endless runner where you take on the role of a scrappy startup founder running through the city streets to keep your company alive before your bank balance burns to zero!

Built with **Three.js (WebGL)**, **HTML5**, and **Vanilla CSS** in a single file per the Master Game Spec.

---

## 🎮 How to Play

### 1. Launching the Game
Simply open [index.html](file:///d:/Downloads/Startup-Run-main/index.html) in any modern web browser, or serve it locally via:
```bash
python -m http.server 8000
```
Then navigate to `http://localhost:8000`.

### 2. Controls
- **Change Lanes**: `A` / `D` or `Left Arrow` / `Right Arrow` (or Swipe Left / Right on touchscreen)
- **Jump**: `W` / `Up Arrow` / `Spacebar` (or Swipe Up)
- **Slide / Fast-Fall**: `S` / `Down Arrow` (or Swipe Down)
- **Pause Game**: `P` or `Escape`

---

## 💡 Gameplay & Economy

- **Starting Capital**: $50,000 in your startup account.
- **Burn Rate**: Starts burning -$1,000/s passively, compounding +100% every minute survived.
- **Speed Acceleration**: Forward speed increases by +2% every 10 seconds.
- **Decision Gates**:
  - 🟢 **Green Neon Gates (Buffs)**:
    - *Angel Seed Investment*: +$25,000 Balance
    - *Product Goes Viral*: +$15,000 Balance & Reduces Burn Rate by 10%
    - *Popularity Surge*: +$20,000 Balance & Reduces Burn Rate by 15%
  - 🔴 **Red Hazard Gates (Debuffs)**:
    - *AWS Server Outage*: -$20,000 Balance
    - *Unexpected Tax Audit*: -$35,000 Balance
    - *Office Rent Overdue*: -$18,000 Balance
    - *Bank Debt Repayment*: -$22,000 Balance
  - 🟡 **Yellow Glowing Gates (Hire Staff / Trade-offs)**:
    - *Hire Top Executive*: -$10,000 Upfront / +$500/s Long-term Revenue
    - *Hire Core Tech Team*: -$8,000 Upfront / +$400/s Long-term Revenue
- **Obstacles & 2-Hit Fatal Collision Rule**:
  - 🚗 *Incoming Cars*: Drive fast toward player in oncoming traffic. Vault over with a timed jump (`W` / `Up` / `Space`) or dodge by switching lanes.
  - 🚧 *Roadblocks & Ditches*: Must jump over with `W` / `Up` / `Space`.
  - 🏗️ *Low Scaffolding*: Must slide underneath with `S` / `Down`.
  - 🚓 *Police Checkpoint*: Blocks 2 lanes. If caught, founder is held up for 4 seconds while survival runway burns!
  - ⚠️ **2 Strikes Game Over**: 1st obstacle crash warns and incurs balance/damage penalty; a 2nd collision is **FATAL GAME OVER**! Use **Legal Shields (🛡️)** to absorb hits without taking strikes.
- **Power-ups & Collectibles**:
  - 🪙 *Coins & Cash Stacks*: Collect to extend cash runway.
  - 🧲 *Cash Magnet*: Pulls all nearby cash to your lane for 10 seconds.
  - 🛡️ *Legal Shield*: Grants an energy forcefield that absorbs 1 obstacle collision penalty & strike-free.
  - ⚡ *Speed Boost Pads*: Accelerates forward with dynamic FOV kick and grants a 2x cash multiplier!
- **🌪️ Chaotic Macro Market Events (Every 45-60s, 15s duration)**:
  - 🚀 *"Tech Bubble / Crypto Craze"*: Golden glowing sky, forward speed spikes +30%, torrential cash and coins flood all 3 lanes!
  - 📉 *"Market Downturn / Recession"*: Dark stormy clouds and torrential rain roll in, passive burn rate DOUBLED (2x)!
  - 👾 *"Viral Server Crash / DDoS Attack"*: Screen glitch distortion with digital scanlines, decision gate metrics scrambled!
- **☁️ Dynamic Moving Clouds & Atmosphere**:
  - Volumetric 3D cumulus clouds drifting across the sky with natural wind and parallax.
  - Dynamic time-of-day progression: Day ☀️ -> Sunset 🌅 -> Night 🌙 -> Neon Cyberpunk ⚡, plus custom event atmospheres.
- **Game Over**: When your balance burns to $0 (Bankrupt) or upon suffering 2 fatal collisions! Survive the full 10:00 to achieve a **Unicorn IPO Exit**! Ranked on the **Top 10 LocalStorage Leaderboard**.
