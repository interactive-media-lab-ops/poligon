# Poligon · Hackathon submission

2026-11-05 · Максим Поляков · Cultural Forces (ГО Культурний Десант)

## One-liner

**Poligon is a decision trainer for personnel-support reform: a board where you assemble the support system from blocks and immediately see where a decision from the top passes and where it stalls.**

The General Staff signs a directive; the company deputy carries the consequences. Poligon makes that distance visible. The user rearranges positions and functions across six command levels, flips system rules, and a deterministic engine of 57 rules recomputes time on core work, functions without an owner, trust barriers, risks and how many of seven typical decisions reach the soldier. Every number carries a provenance passport to its source. The tool does not score or rank models; it shows what follows from what. The conclusion is made by a person.

Single HTML file, no server, no install, works offline and on a projector. Pitch edition v0.9.1, build 2026-11-05.

## Problem

Reform decisions about personnel support are written at the General Staff and die between the brigade and the company, and nobody can see where. The norm ends at the battalion; reality ends at the company, where one deputy commander carries eight lines of work, four of them without any norm.

The numbers behind this, from the engine's baseline:

- The company deputy spends **50.3%** of time on core support work; the rest goes to service investigations (19.4%), social and living problems (9.8%), commander duties at the command post (7.2%) and reporting.
- **5 of 18** functions have no owner at the level where they are needed; culture and traditions drain down to the company.
- **8 of 8** trust barriers are active (weight 84 of 90 informants): help tied to punishment, fear of leaks to command, talks and visits used as control.
- **0 of 7** typical decisions from above reach the soldier without a block in the current configuration.
- **27** report forms go up from the company; about 4 hours a day in departments; one-way reports with no reply.

Every reform proposal on the table (four variants in the tool) is argued in words and slides. There is no shared instrument to test a variant against the same constraints, with the same numbers, before an order is signed. Poligon is that instrument.

## Solution

Poligon turns a reform proposal into a board state that can be moved, broken and compared, with consequences recalculated after every move.

**The board.** Rows are six command levels, from the General Staff to the company, with a platoon strip underneath for the soldier and the sergeant. Columns are eight verticals: command, support line, military police and legal, social services, medics and chaplains, personnel, military music, partners. Cells hold positions (cards) and functions (pills). A function sits at the lowest level it reaches; a red dashed pill marks a gap where it is needed but does not arrive.

**Moves.** Drag a block between cells; only allowed cells light up. Click a block for its menu: depth, vertical, replacement (company ensign instead of the company deputy), reinforcement (mobile group, clerk), removal, evidence. Sixteen system rules on the rail: thresholds, anonymous surveys, confidentiality boundary, single report form, mandatory reply, school and probation. Undo, redo, reset, and a link that encodes the whole board state.

**Consequences.** Six numbers on top: time on core work, functions with an owner, trust barriers, active risks, decisions passing, positions per brigade. The side panel shows a move card (what changed, which rules switched on and off) and indicators with a *rules: N* button that opens the fired rules and the passport of every number.

**Decisions from above.** Seven typical General Staff decisions, including one anti-example (named instead of anonymous surveys), run top-down through the levels. At each level the board answers: passes, friction or blocked. A marker runs down the rows and stops where the decision stalls. Move a block and run it again.

**Five variants.** Baseline (the current system) and four reform models, each a full board state. Any variant can be compared with the current board indicator by indicator.

**Second screen.** A display window for a projector: the board silhouette with the move highlighted, the six numbers, seven decisions with the level where each stalls, a radar of seven leadership principles and a one-line plain-language explanation. It also runs standalone from a link for a presentation without an operator.

## How it works

The engine is a deterministic rule model: the same board state always gives the same numbers, and every number can be traced to a rule and a source.

The state **s** is the set of functions (vertical and depth), positions (vertical and level) and switches. Indicators are **F(s)**, computed in one pass over 57 rules. Each rule is a condition over predicates (reaches, inLane, structsAt, owned) and an effect: a time-article coefficient k(j) in [0, 1], a trust-barrier activity b(m) in [0, 1], a risk, a document, an owner status, forms or hours.

    T_l = 100 − Σ_j C(j,l) · k(j)

Time on core work at level *l* uses the timekeeping matrix C (7 articles × 4 levels). In the current system all k = 1 and the model reproduces the measured values exactly: 50.3 / 49.1 / 58 / 52.6.

    Trust = Σ_m w(m) · b(m)

Trust barriers are weighted by the number of informants naming each barrier (18, 13, 12, 11, 11, 9, 9, 7; total 90). Functions without an owner count the functions with status missing, falls short, drains or no executor. Decisions passing counts the decisions with no block on any of seven levels.

Coefficients are of two kinds: **measured** (timekeeping, informant counts) and **estimated** (residual time shares after a function is moved, partial barriers). All of them live in one registry with a passport and the role that must confirm each; rules read values only from there. Estimates are checked with a ±30% band. The principles layer (seven leadership principles on the display radar) computes nothing itself: it reads the indicators and maps them to 0–1 by threshold and target.

What the model does not do: it does not forecast over time, does not rank schemes, does not measure AWOL or staffing, does not balance workload between positions and does not model local adaptation. The board is the typical structure an order sets; how a given brigade adapts it the model does not know.

## What is built

The pitch edition is a working, tested single-file app (about 900 KB with embedded fonts), not a mock-up.

| Component | Content |
| --- | --- |
| Ontology | 6 levels, 8 verticals, 18 functions, 36 positions, 16 system rules |
| Engine | 57 deterministic rules, 8 trust barriers, 7 × 4 timekeeping matrix, coefficient registry with passports |
| Variants | Baseline + 4 reform models, each a full board state |
| Decisions from above | 7 decisions × 7 levels, including one anti-example |
| Screens | Board, Compare, Decisions, Links overlay, Lenses (view as the commander-in-chief, brigade commander, company deputy or soldier) |
| Second screen | Display window with animated moves, decisions track, principles radar; standalone mode from a link |
| State | Undo, redo, reset, link encoding the whole state, save and open as JSON |
| Crash-test journal | Key N: «this is not true» entries tied to screen and board state, export to Markdown and JSON |
| Interactive tour | 9 steps on the board itself |

Tested on 2026-11-05 in headless Chromium: all five variants, all seven decisions, every block menu, every system rule, drag and drop, undo and redo, link round-trip, standalone display. Zero page errors after fixes. Fixes in v0.9.1: keys 3–9 no longer throw on screens hidden in the pitch edition; Esc closes the journal from the text field; about 120 strings that still rendered in Ukrainian (move card, rule titles, block menus, link notes, toasts, tour, display window) now render in English.

## Evidence base and data policy

Every number in the engine has a source; in the pitch edition the sources are withheld and nothing in the tool can identify a person or a unit.

The model draws on three kinds of material: timekeeping of the current system across four levels (company, battalion, brigade, brigade stress group), 94 field interviews that give the trust-barrier weights and the scenarios, and the orders and doctrine that define where each function sits by norm. The tool states the status of each number: measured, draft report, estimate, or pending data (staffing breakdown, register of forms).

Pitch-edition rules, enforced in the data layer: no surnames, no unit names, nine sources shown as «Source 1–9 · withheld», reform variants labelled A–D instead of their authors, no evaluative labels on models. A stop-list of politicized terms is built in. The full edition with named sources exists and is shown only to the roles that provided the data.

This matters for a hackathon jury: the tool can be demonstrated, shared by link and left running on a screen without disclosing anything sensitive.

## Who it is for and impact

Poligon is for the people who decide the shape of personnel support and for the people who have to live inside that shape.

| User | What they do with it |
| --- | --- |
| Reform working group at the General Staff directorate | Test a variant against the same constraints before an order is drafted; see which decision stalls at which level |
| Commander or chief of staff at corps and brigade | Look at the board through the lens of their own level; see what reaches their deputies and what drains to the company |
| Authors of competing reform models | Put each model on the same board with the same numbers instead of arguing slides against slides |
| Researchers and NGO analysts | Attach new evidence and coefficients with passports; run the crash-test journal with practitioners |
| Training and leadership schools | Use the board as a case exercise: build a system from an empty board and watch which functions are left without an owner |

Expected impact: fewer decisions that are signed at the top and never reach the soldier; a shared, checkable language for a reform debate that today runs on opinion; and a reusable pattern (board + rule engine + provenance passports) for other structural reforms where the gap between the norm and the lowest level is the real problem.

Open question for the jury: which organisation hosts the full edition and runs the crash test with the seven reviewer roles (practitioner, defender of the current system, methodologist, report analyst, normative expert, commander, practising psychologist).

## Roadmap

The next step is not more features but the crash test: seven roles break the model before it is shown to decision-makers.

1. **Crash test (weeks 1–3).** Seven reviewer roles, 60–90 minutes each alone with the tool; every «this is not true» goes into the journal with role, screen and board state. A fix enters the model only with a passport.
2. **Data from the directorate (weeks 2–6).** Staffing breakdown per level and the register of report forms replace the estimates marked «estimate»; the ±30% band on estimated coefficients narrows.
3. **Signed weights (week 6).** The principles layer runs on derived weights today (1/n per principle, 1/m per factor). A signed table of weights from the methodologist goes into the WEIGHTS slot.
4. **Full edition for the working group.** Named sources, authors of the variants, the Scheme, Criteria, Evidence, Passport and Rules screens that the pitch edition hides.
5. **Second domain.** Apply the same board-and-rules pattern to another structural reform with the same symptom: an order that does not reach the lowest level.

Out of scope on purpose: forecasting, model ranking, a total score. The tool stays a trainer, not a judge.

## Team and organization

Poligon is built by Cultural Forces (ГО Культурний Десант), an NGO that works with military units on culture, morale and personnel support, which is where the field interviews and the timekeeping behind the model come from.

| Role | Who |
| --- | --- |
| Project lead, presenter | Максим Поляков · m.poliakov@culturalforces.org |
| Model and evidence | *fill in* |
| Engineering | *fill in* |
| Field research | *fill in* |

Fill in the remaining roles before submission; most hackathon forms ask for 2–5 named members with roles.

## Submission checklist and short answers

Most forms ask the same six things; the answers below are ready to paste, within typical limits.

- [ ] Project name: **Poligon** (decision trainer for personnel support reform)
- [ ] Category: civic tech / defence tech / decision support (pick what the hackathon offers)
- [ ] Demo link: the single HTML file hosted on any static host, or opened locally; add `#display&p=now` for the standalone display
- [ ] Repository or archive with `poligon-pitch.html` and this document
- [ ] Pitch deck
- [ ] Demo video, 2–3 minutes, recorded from the demo script
- [ ] Team members and roles filled in above
- [ ] Confirm the pitch edition is used in every public material (no names, no units, sources withheld)

**Tagline (under 100 characters).** Build the personnel-support system from blocks and see where a decision from the top stalls.

**Short description (under 300 characters).** Poligon is a deterministic decision trainer: a board of 6 command levels and 8 verticals, 57 rules, 7 decisions from above and 5 reform variants. Move a block and see time on core work, functions without an owner, trust barriers and which decisions reach the soldier.

**What makes it different (under 500 characters).** It does not score or rank reforms. It shows what follows from what, with a provenance passport on every number: timekeeping across four levels, 94 field interviews, orders and doctrine. Estimates are marked and bounded. A second screen for a projector and a crash-test journal make it a tool for a working group, not a slide.

**Technology.** One HTML file, vanilla JavaScript, no dependencies, no server, works offline. Engine and data layer are separate scripts; the pitch edition adds an English layer and anonymization on top without touching the engine.
