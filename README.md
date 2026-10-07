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
- **Obstacles**:
  - 🚗 *Incoming Cars*: Can be vaulted over with a timed jump (`W` / `Up` / `Space`) or dodged by switching lanes.
  - 🚧 *Roadblocks & Ditches*: Must jump over with `W` / `Up` / `Space`.
  - 🏗️ *Low Scaffolding*: Must slide underneath with `S` / `Down`.
  - 🚓 *Police Checkpoint*: Blocks 2 lanes. If caught, you are held for 4 seconds while your balance burns!
- **Game Over**: When your balance hits $0 or below, you go Bankrupt! Your company's survival time is ranked on the **Top 10 LocalStorage Leaderboard**.
