# ROBOPREP — Gamified Robotics Interview Prep Web App

## Project Brief for Claude Code

---

## OVERVIEW

Build a single-page, static web app (deployable on GitHub Pages) that helps a robotics MS student (Siddarth Dayasagar, Northeastern, graduating Dec 2026) prepare for technical interviews covering **Controls, State Estimation, Reinforcement Learning, Perception, Navigation/Planning**, and **resume-specific questions**. The app must be gamified, mobile-responsive, and contain 90-100+ detailed technical Q&As with interview-grade answers.

---

## USER PROFILE (from resume — use for personalized questions)

- **Current:** MS Robotics @ Northeastern (GPA 3.63), graduating Dec 2026
- **Undergrad:** BTech Robotics & Automation, Jain University, Bangalore (GPA 3.8)
- **Skills:** Python, C++, MATLAB, ROS2, OpenCV, PCL, CasADi, Pinocchio, Gazebo, MuJoCo, Isaac Sim/Lab
- **Work Experience:**
  - **SpaceData Inc., Tokyo (Aug–Dec 2025):** PPO quadruped locomotion in Isaac Sim (70% improvement over baseline), ROS2 Nav2 + MPPI for GPS-denied indoor nav, outdoor nav with elevation grid mapping + MPC, Space Station OS (ROS2 autonomy framework with behaviour trees)
  - **Flomobility, Bangalore (Sep 2022–Apr 2023):** AprilTag-based localisation, Bang-Bang/PID control, robotics software stack ownership
- **Key Projects:**
  - **BHEEMA (Bipedal Humanoid):** Centroidal MPC (OSQP, 16 Hz) + whole-body controller (Pinocchio, 200 Hz) + impedance control for Unitree G1. Fixed Pinocchio↔MuJoCo frame mismatch. Achieved 20+ sec stable bipedal walking.
  - **PRANA:** Flow-matching vision-action policy, DINOv2 backbone, deployed on physical 7-DOF arm at 50 Hz
  - **PPO Pick-and-Place:** PPO for Franka Panda in MuJoCo, Cartesian action space, GAE, reward shaping
- **Weak areas (self-identified):** Navigation & planning (graphs, trees, search algorithms)
- **Strong areas:** Controls, state estimation, RL

---

## TECH STACK

- **Single `index.html` file** — no build tools, no frameworks, no dependencies
- HTML + CSS + vanilla JavaScript only
- All data embedded as JS objects (no external JSON files)
- Must work on GitHub Pages (static hosting)
- Must be fully responsive (mobile-first: works on phones, tablets, desktops)
- Use `localStorage` for persistence (progress, XP, streaks)

---

## APP STRUCTURE

### 1. Top Navigation Bar (fixed)
- Logo: "ROBOPREP" with accent color
- Stats display: XP points, current streak (days), questions completed
- Hamburger menu toggle on mobile

### 2. Sidebar (collapsible on mobile)
- **Main:** Dashboard, Quiz Mode
- **Topics:** Controls, State Estimation, RL, Perception, Navigation & Planning, Your Resume
- Each topic shows question count badge
- Active section highlighted

### 3. Main Content Area
Renders one of these views based on sidebar selection:

---

## VIEWS

### A. Dashboard (Home)
- Welcome banner with user name and streak badge
- 6 section cards in a grid, each showing:
  - Section icon and name
  - Question count
  - Progress bar (% of questions marked as reviewed)
  - Click navigates to that section
- Overall stats summary

### B. Section View (one per topic)
- Section header (tag, title, description)
- Progress bar for that section
- **Filter row:** difficulty chips (All / Core / Intermediate / Advanced) + text search input
- **Q&A cards** — accordion-style:
  - Question number, question text, difficulty badge, expand/collapse chevron
  - On click: expands to show the detailed answer
  - Answer supports rich formatting (see Answer Formatting below)
  - "Mark as Reviewed" button at bottom of each answer (toggles green checkmark, persists in localStorage)
  - Completed cards get a green left border

### C. Quiz Mode
- Section selector (dropdown or chips to pick which topic, or "All")
- Difficulty filter
- Shows one random question at a time from the selected pool
- The question is displayed in a prominent card
- Below it: a textarea for the user to type their answer (for active recall)
- Buttons: "Show Answer" → reveals the reference answer below
- After reveal: self-rating buttons ("Nailed it" +15 XP / "Partial" +8 XP / "Missed it" +3 XP)
- "Next Question" button to advance
- Quiz stats: questions attempted this session, score distribution

---

## ANSWER FORMATTING

Each answer in the data should support these inline HTML elements for rich display:

- `<b>bold text</b>` for key terms
- `<code>inline code</code>` for variable names, function names
- `<span class="formula">...</span>` — styled as a highlighted code block (for equations, formulas). Use monospace, purple accent, dark background, left border.
- `<span class="key-point">...</span>` — green left-bordered callout box for critical takeaways
- `<span class="interview-tip">...</span>` — orange left-bordered callout box for "how to say this in an interview" tips
- Standard line breaks for paragraph separation

---

## GAMIFICATION SYSTEM

All persisted in localStorage:

- **XP Points:** +10 for marking a question reviewed, +15/+8/+3 for quiz self-ratings (Nailed/Partial/Missed)
- **Streak:** Track consecutive calendar days the app was opened. Show 🔥 emoji with count.
- **Progress:** Per-section completion percentage (reviewed questions / total questions)
- **Total reviewed count** shown in nav bar
- **Level system (optional but nice):** Intern (0-100 XP), Junior (100-300), Mid (300-600), Senior (600-1000), Principal (1000+) — show current level somewhere

---

## QUESTION CONTENT REQUIREMENTS

Each section needs **15-20 questions** covering fundamentals to advanced topics. Every question needs:
1. The question text (as it would be asked in an interview)
2. Difficulty: "core", "intermediate", or "advanced"
3. A detailed answer written in **first-person interview style** — as if Siddarth is explaining to an interviewer. Include:
   - Clear conceptual explanation
   - Mathematical formulas where relevant (in formula spans)
   - Concrete robotics examples
   - Key insight callouts
   - Interview tips on how to phrase the answer
   - **References to Siddarth's resume projects where applicable** (especially in Controls, RL, and Resume sections)

### SECTION 1: Controls Engineering (20 questions)
Cover these topics:
- PID control: tuning, anti-windup, derivative kick, when to use
- MPC: formulation, receding horizon, linearization, QP solvers, when to prefer over PID
- LQR: Riccati equation, cost matrices Q/R tuning, relation to MPC
- Impedance control vs position control vs force control
- Operational-space vs joint-space control
- The Jacobian: velocity mapping, force mapping, singularities, null space
- Gravity compensation
- Feed-forward + feedback (computed torque control)
- State-space vs transfer function representations
- Controllability and observability
- Lyapunov stability analysis, CLF
- Gain scheduling
- Friction cone constraints for locomotion
- Regulation vs tracking
- Whole-body controller architecture (centroidal MPC → WBC → joint torques)
- Null-space control for redundant manipulators
- DH parameters vs Product of Exponentials
- Zero Moment Point (ZMP) and dynamic walking
- Centroidal dynamics / single rigid body dynamics model
- QP solvers (OSQP, qpOASES) and why they matter for MPC

### SECTION 2: State Estimation (12-15 questions)
Cover:
- Why state estimation is necessary for controllers
- Kalman Filter derivation and intuition (predict/update)
- EKF vs UKF vs Particle Filter — when to use each
- IMU + kinematics fusion for legged robots
- Sensor fusion principles and architectures
- Observability in estimation context
- Complementary filter for IMU
- SLAM overview (filter-based vs graph-based)
- EKF failure modes
- Dead reckoning vs estimation
- Covariance meaning and tracking
- Practical: how to estimate state for a quadruped in GPS-denied environment
- Bias estimation (gyro bias, accelerometer bias)

### SECTION 3: Reinforcement Learning (15 questions)
Cover:
- PPO: objective, clipping, why it's dominant in robotics
- GAE: bias-variance tradeoff, λ parameter
- Reward shaping: principles, pitfalls, potential-based shaping
- Domain randomization: what to randomize, why it works for sim-to-real
- Sim-to-real transfer: the reality gap, approaches (DR, system identification, fine-tuning)
- On-policy vs off-policy (PPO vs SAC vs TD3)
- Policy architectures for continuous control (MLP, Gaussian policies)
- Value function vs Q-function vs advantage
- Entropy regularization and exploration
- Curriculum learning for robotics
- Action space design: joint torques vs joint positions vs Cartesian increments
- Observation space design: what to include, normalization
- RL vs classical control: when to use RL
- Multi-task RL and transfer learning
- Safety in RL: constrained optimization, safe exploration

### SECTION 4: Perception — Transformers & VLA Models (15 questions)
Cover:
- Vision Transformers (ViT): architecture, patch embedding, positional encoding, self-attention
- Self-attention mechanism: Q/K/V, scaled dot-product, multi-head attention — with formulas
- CNNs vs Transformers for vision: inductive biases, when each wins
- DINOv2: self-supervised pre-training, why it's a strong backbone for robotics
- Vision-Language-Action (VLA) models: what they are, architecture, how they connect vision to actions
- Flow matching: how it differs from diffusion, velocity field formulation, why it's gaining traction
- Diffusion policies for robotics: iterative denoising of action trajectories
- Imitation learning vs RL: when to use each, behavioral cloning pitfalls
- Action chunking: predicting multi-step action sequences, amortizing inference cost
- Feature extraction: ViT-Tiny vs ConvNeXt vs DINOv2 — tradeoffs
- Attention mechanisms in robotics: cross-attention for language-conditioned policies
- Tokenization strategies for robotic observations
- Real-time deployment: inference optimization for 50 Hz control (quantization, action buffers)
- The BEHAVIOR Challenge and competition approaches
- End-to-end learning vs modular perception + planning + control

### SECTION 5: Navigation & Planning (15 questions)
*NOTE: User is weak here — be extra thorough with explanations, build intuition from scratch*
Cover:
- Graph representations for robotics: nodes, edges, adjacency, when to use graphs
- BFS vs DFS: intuition, time/space complexity, when each is useful
- Dijkstra's algorithm: step-by-step with example, why it works, complexity
- A* search: heuristic, admissibility, optimality guarantee, comparison with Dijkstra
- RRT and RRT*: sampling-based planning, why needed for high-dimensional spaces
- PRM (Probabilistic Roadmap): precompute phase vs query phase
- Configuration space vs workspace: why plan in C-space
- Potential fields: attractive/repulsive, local minima problem
- Costmaps in ROS2 Nav2: global costmap, local costmap, inflation layer
- Global vs local planning: how they interact in Nav2
- MPPI (Model Predictive Path Integral): sampling-based local planner, why you used it at SpaceData
- Behaviour trees: structure, tick mechanism, selector/sequence/action nodes, vs FSMs
- D* Lite: replanning for dynamic environments
- Lattice-based planning: motion primitives, state lattice
- Elevation mapping for outdoor navigation: 2.5D maps, terrain traversability

### SECTION 6: Resume-Based / Personalized Questions (15 questions)
These simulate "tell me about your project" style questions. Each answer should be structured as STAR-like narratives:
- Walk me through your centroidal MPC architecture for BHEEMA. What were the key design decisions?
- You improved PPO performance by 70% at SpaceData. What specifically did you change?
- Explain the Pinocchio↔MuJoCo frame mismatch bug. How did you identify and fix it?
- How did your whole-body controller work at 200 Hz? What computations were in the loop?
- Describe your flow-matching policy in PRANA. Why flow matching over diffusion?
- How did you deploy a learned policy on a physical 7-DOF arm at 50 Hz?
- What was Space Station OS? How did you architect it?
- How did you integrate Nav2 with MPPI for GPS-denied navigation?
- What was your outdoor navigation pipeline at SpaceData? How did elevation mapping work?
- At Flomobility, how did you use AprilTags for localisation? What problems did it solve?
- How did you handle sensor-to-actuator latency at Flomobility?
- Compare your PPO pick-and-place approach with classical IK-based methods. Tradeoffs?
- What reward functions did you design for the Franka Panda pick-and-place?
- How would you extend BHEEMA to handle uneven terrain?
- If you had to redo the PRANA project, what would you change?

---

## VISUAL DESIGN

### Aesthetic: Dark, terminal-inspired, cyberpunk-lite
- **Background:** Very dark navy/charcoal (#0a0e17)
- **Cards:** Dark card backgrounds (#1a2234) with subtle borders (#263248)
- **Typography:**
  - Headings/code/labels: `JetBrains Mono` (monospace, techy feel)
  - Body text: A clean sans-serif like `IBM Plex Sans` or `DM Sans`
  - Load from Google Fonts CDN
- **Colors:**
  - Primary accent: Cyan (#00e5ff) — for Controls, active states, primary buttons
  - Secondary accent: Orange (#ff6b35) — for RL section, interview tips
  - Green (#00e676) — for State Estimation, success states, "completed" indicators
  - Purple (#b388ff) — for Perception, formulas
  - Yellow (#ffd740) — for Navigation, streaks
  - Pink (#ff4081) — for Resume section, advanced difficulty
- **Effects:** Subtle glow on hover (box-shadow), smooth accordion animations, minimal transitions
- **No images needed** — all visual interest from color, typography, spacing, and subtle gradients

### Mobile-first responsive:
- Sidebar collapses to hamburger menu on mobile
- Cards stack vertically
- Quiz textarea is full-width
- Touch-friendly tap targets (min 44px)

---

## FILE STRUCTURE

```
index.html          ← single file, everything inline (CSS in <style>, JS in <script>)
README.md           ← basic project description for GitHub
```

That's it. One HTML file. No build step. User drops it in a GitHub repo, enables Pages, done.

---

## IMPLEMENTATION NOTES

1. **All Q&A data** lives in a single JS object at the top of the `<script>` block. Structure:
```js
const DATA = {
  controls: {
    tag: "SECTION 01",
    title: "Controls Engineering",
    desc: "...",
    color: "var(--accent-cyan)",
    questions: [
      { q: "...", diff: "core", a: "..." },
      ...
    ]
  },
  estimation: { ... },
  rl: { ... },
  perception: { ... },
  navigation: { ... },
  resume: { ... }
};
```

2. **Rendering** is JS-driven. One `showSection(key)` function reads `DATA[key]` and renders all cards. One `showHome()` function renders the dashboard. One `showQuiz()` function renders quiz mode.

3. **localStorage keys:**
   - `roboprep_completed` → JSON object mapping `"sectionKey-questionIndex"` → `true`
   - `roboprep_xp` → integer
   - `roboprep_streak` → `{ count: N, lastDate: "YYYY-MM-DD" }`
   - `roboprep_quiz_stats` → `{ attempted: N, nailed: N, partial: N, missed: N }`

4. **Search/filter** — client-side string matching on question text. Difficulty filter shows/hides cards by checking the diff property.

5. **Accordion** — toggle `.open` class on card click. Only one answer open at a time (optional: could allow multiple).

6. **Quiz random selection** — pick from filtered pool, track "already seen this session" in a Set to avoid repeats.

---

## QUALITY CHECKLIST

- [ ] All 90-100 questions have detailed, technically correct answers
- [ ] Formulas are readable and correctly formatted
- [ ] Resume references are accurate to the attached resume
- [ ] Navigation section explains concepts from first principles (user is weak here)
- [ ] Perception section covers transformers and VLA models in depth
- [ ] Mobile layout works on 375px width (iPhone SE)
- [ ] localStorage saves/loads correctly across sessions
- [ ] Quiz mode randomly selects questions and tracks XP
- [ ] All sidebar links work and highlight correctly
- [ ] Progress bars update when questions are marked reviewed
- [ ] No external dependencies that could break (everything inline or from stable CDNs)
- [ ] The app loads fast (single file, no network requests except fonts)

---

## RESUME (for reference)

Siddarth Dayasagar
617-560-0540 | dayasagar.s@northeastern.edu

**Education:**
- Northeastern University, Boston — MS Robotics (Expected Dec 2026, GPA 3.63)
  - Coursework: Mobile Robotics, Robotics Mechanics & Control, Control Systems Engineering, Formal Methods, Legged Robotics, Pattern Recognition & CV
- Jain University, Bangalore — BTech Robotics & Automation (June 2023, GPA 3.8)

**Skills:** Python, C++, MATLAB, OpenCV, PCL, ROS2 (Humble, Jazzy), CasADi, Pinocchio, Gazebo, MuJoCo, Isaac Sim, Isaac Lab

**SpaceData Inc. (Aug–Dec 2025):**
- PPO quadruped locomotion in Isaac Sim, ~70% improvement via reward refinement, hyperparameter optimization, domain randomization
- ROS2 Nav2 + MPPI local planner for indoor GPS-denied disaster response
- Outdoor nav pipeline: elevation grid mapping + MPC local controller
- Space Station OS: modular ROS2 autonomy framework, behaviour trees, diagnostics, multi-node coordination

**Flomobility (Sep 2022–Apr 2023):**
- AprilTag-based localisation pipeline
- Bang-Bang and PID control for physical actuators
- Enterprise robotics software stack ownership
- Sensor-to-actuator latency optimization

**BHEEMA — Bipedal Humanoid (Jan–Apr 2026):**
- Centroidal MPC: SRBD, OSQP QP solver, friction cone constraints, 16 Hz
- WBC: Pinocchio analytical Jacobians, gravity compensation, joint-space PD, operational-space impedance, 200 Hz
- Fixed Pinocchio↔MuJoCo coordinate frame mismatch, achieved 20+ sec stable walking

**PRANA — Vision-Action Policy (Feb–Apr 2026):**
- Flow-matching policy: iterative denoising → 50-step action trajectories
- Ablation across ViT-Tiny, ConvNeXt, DINOv2 backbones
- Deployed on physical 7-DOF arm at 50 Hz with action buffering

**PPO Pick-and-Place (Nov–Dec 2025):**
- PPO for 7-DOF Franka Panda in MuJoCo
- Cartesian end-effector action increments, GAE, entropy regularization
- Custom reward shaping: reach, joint limits, smooth motion, collision avoidance
