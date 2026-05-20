# Ultra 50 Training Dashboard — Project Brief

## What this is
A 20-week training dashboard for a 50-mile ultramarathon. Tracks daily workouts, runs, nutrition, recovery metrics, weight, and personal cooking. Built as static HTML/JS that deploys to a CDN (Netlify) from this repo.

## Athlete profile
- Pescatarian, occasionally omnivore
- Has ADHD and dyslexia — design accommodations are core requirements, not nice-to-haves
- Strong cardio base, ~1 year detraining at program start
- Home gym (dumbbells + rack), trail running on mountain bike terrain
- Forages mushrooms on runs
- Has a garden (tomatoes, salad greens, herbs)
- Weekly grocery budget ~$125
- Cooks 5+ nights/week, sometimes for guests with same-day notice
- Cuisine preferences: Mediterranean, Japanese/Asian, Mexican/Tex-Mex, Italian, Indian, Thai
- Strong opinions on food: salmon only when exceptionally prepared, no salmon every day, mushrooms in everything that takes them

## Program structure
- **Start:** Wednesday April 15, 2026
- **Race:** ~September 1, 2026
- **Total:** 140 days, 20 weeks
- **Phases:** Cut + Base (1–5), Aerobic Build (6–11), Peak (12–18), Taper (19–20)

## Nutrition philosophy — near-keto pescatarian
- **No calorie counts anywhere** — track macros only: protein / fat / net carbs
- Under 50g net carbs on rest/easy days, up to 75g on hard training, up to 100g on long run days
- Intermittent fasting on a flexible 2–3 day rotation (16:8 default, 18:6 on aggressive fast days)
- Long run days: targeted carbs pre/during/post run only, return to keto after
- Eating window restrictions never imposed before hard sessions or long runs
- Fat as primary fuel — avocado, olive oil, nuts, full-fat dairy, eggs, fatty fish
- Protein anchors: eggs (primary), shrimp, white fish (cod, halibut, mahi, tilapia), occasional salmon (only miso-glazed or properly blackened), sardines, mackerel, full-fat cheese, full-fat Greek yogurt (small portions), protein powder in smoothies
- Smoothie base: unsweetened almond or coconut milk + avocado + berries (no banana except post-long-run) + nut butter + hemp hearts
- Legumes eliminated (too carb-heavy)
- Mushrooms featured in many meals

## Lifting program — single daily workout
**Design principle:** ADHD-friendly = zero decision overhead. One workout, memorized, executed daily. Same exercises every lift day across all phases. Volume scales by phase, exercises do not.

The workout is in `program.json` on every lift day. Includes a unique exercise called "Angled chest-pec curl" — gripping a fixed vertical support, leaning back to a 30–45° body angle, performing a one-arm dumbbell curl from that diagonal position to target pecs through the unusual line of pull.

Phase volume scaling:
- Phases 1–2: 4 sets compounds, 3 sets isolation
- Phase 3 (peak running): 2–3 sets, maintenance only
- Phase 4 (taper): 2 sets, light, neuromuscular only

## Daily activity timing framework
Time-of-day recommendations based on cortisol rhythm, fasting state, and adaptation goals:
- **Trail running:** 6–9am fasted (max fat oxidation)
- **Weightlifting:** 10am–1pm (post-cortisol-peak, neuromuscular drive high)
- **Cold plunge:** post-run or morning fasted — NOT immediately post-lift (blunts hypertrophy mTOR signaling, wait 4+ hours)
- **Hot tub / sauna:** evening, 1–3 hours before bed (GH potentiation, parasympathetic shift)
- **Red light therapy:** morning (circadian + mitochondrial) or post-workout (recovery)

## Files

### index.html
Main dashboard. Self-contained single file (~92KB). All JS inline. Two tabs: Today and Overview.

Today tab features:
- Day navigation
- Workout details with clickable exercise diagrams (SVG stick figures + YouTube search links to Jeff Nippard)
- Separate "Log lift" and "Log run" tracking buttons
- Daily vitals card (sleep hours, RHR — manual entry from Apple Watch)
- Run log: pace (min/mi), RPE 1–10, free-text notes
- Meal cards with checkboxes and free-text "what I actually ate" notes
- Nutrition shows eating window + macros + coaching note (no calories)

Overview tab features:
- Race countdown
- Training load arc (20-week bar chart with RPE overlay)
- Long run progression (with pace overlay)
- Effort split donut (Easy / Moderate / Hard distribution)
- Recovery chart (RHR + sleep with alerts when RHR spikes 5+ bpm above baseline)
- Cumulative miles bar
- Weight tracker with full graph and phase boundary lines

### program.json
140-entry array. Each entry has:
- `week`, `day`, `date`, `phase`
- `run` (or null) — miles, effort, type, description
- `lift` (or null) — title, exercises array, note, scale
- `nutrition` — window, note, protein_g, fat_g, net_carbs_g, meals array

### kitchen.html
Standalone kitchen app (~70KB) linked from dashboard footer.

Uses OpenDyslexic font throughout. Four tabs:
- **Recipes:** 15 recipes filterable by category, cuisine, time, mushroom-content
- **Cook:** step-by-step cook-along with active step highlighting, servings scaler, US/metric toggle, browser speech synthesis read-aloud, timer button (floating timer with audio alert)
- **Pantry:** tap-to-toggle items, dynamic "Make now" and "Almost there" recipe sections
- **Shop:** running list organized by store section, tap to check off, spice kit one-time setup, custom add

Recipes use forage flags for mushroom upgrade suggestions where relevant.

## Storage
All progress data in browser localStorage. Keys:
- `u50-prog` — cached program.json
- `u50-done` — array of day keys marked done
- `u50-workouts` — split lift/run tracking object
- `u50-meals` — meal checkboxes and notes
- `u50-runlog` — pace, RPE, run notes
- `u50-daily` — sleep, RHR
- `u50-weight` — weight log entries
- `k50-pantry` — pantry state
- `k50-shopping` — shopping list

**This is the next problem to solve.** localStorage is device-specific and easily wiped. Plan: GitHub API persistence via Personal Access Token (write to a `logs.json` in repo).

## Deployment
- Repo: private
- Host: Netlify, free tier, connected to private repo, auto-deploy on push
- Default page: index.html (and kitchen.html linked from dashboard footer)

## What's built ✓
- Full 140-day program with workouts, runs, near-keto nutrition
- Single daily upper body workout across all lift days
- Daily tracking: lift, run, meals (with notes), pace, RPE, sleep, RHR, weight
- Overview analytics with logged data overlays
- Exercise diagrams + YouTube links
- Kitchen app with cook-along, pantry, shopping list
- Dashboard ↔ Kitchen linking

## Priority next builds
1. **GitHub API persistence** — replace localStorage with logs.json sync in repo. User generates PAT, dashboard reads/writes via API. Device-independent, never lost.
2. **Daily activity timing view** — surface the cortisol-aligned schedule (run / lift / cold plunge / sauna / red light) per day type
3. **Cann-Athletics module ("Labs" tab)** — see separate spec below
4. **More recipes in kitchen.html** — target 25–30 total. Gaps: Indian, Thai curries, more mushroom-forward, more 30-min impressive options, meal prep bowls
5. **Hosting mode in kitchen** — scale recipes for 2–4 people with reordered steps to prevent timing collisions
6. **Pantry-based meal planning** — select recipes for the week, auto-populate missing ingredients to shopping list
7. **Split workout tracking in overview stats** — currently day-level; should count lift and run separately

## Cann-Athletics module spec (new tab: "Labs")
The athlete uses cannabis as a performance protocol — not recreationally. Three primary use cases with distinct profiles:
- **Morning microdose inhaled** for focused work sessions (creative flow, ADHD focus)
- **Edible pre/during long trail efforts** for extended effort, perceived effort reduction, endocannabinoid potentiation at ultra distances
- **Backcountry sport contexts** (skiing, MTB) with safety-tiered dosing

The dashboard should support:

**Session Log** — every intake event records:
- Activity context, delivery method, product/strain, dose (mg THC, mg CBD, ratio, or inhaled duration), onset time, window duration, performance rating, notes

**Protocol Library** — named, reusable templates:
- Morning Flow, Long Run Protocol, Backcountry Protocol, Recovery Protocol
- One-tap log against a saved protocol

**Correlation Engine** — n=1 personal data plots:
- RPE × dose on training days
- Pace × dose on runs
- Sleep × evening use
- Recovery (next-morning RHR) × evening use
- CSV export

**Compliance:** This is personal use research data. Nothing customer-facing without legal review. Add "personal research only" footer to Labs tab.

## Design principles
- ADHD/dyslexia friendly: OpenDyslexic font in kitchen, one action per step, amounts inline in steps, never require cross-referencing
- Low friction logging: every metric saveable with one tap
- Mobile-first: max-width 640px, touch targets ≥44px
- Vanilla JS only — no React/Vue, no build step
- Self-contained files where reasonable
- Server timestamp: today is approximately the start of the program (mid-April 2026)
