# Signal-Down-A-telecom-technician-simulator-demo
created with the help of Claude Fable 5


SIGNAL DOWN: Metro Field Tech Simulator

Game Design Document — First-Person Wireless Network Technician Simulator


Gameplay Overview

Signal Down puts the player in the steel-toes of a wireless field technician working for a major carrier in a dense metropolitan city (modeled loosely on a Montreal/Toronto-scale grid: downtown core, industrial parks, residential boroughs, highway corridors, and a rural fringe with hilltop macro sites). The player starts each shift in a company truck loaded from a parts depot, receives tickets through a NOC dispatch system, and must keep the network healthy across ~60 cell sites — rooftop installs, monopoles, self-support towers, stealth sites, and small cells on light poles. Every site is built from real-world-inspired infrastructure: Ericsson Baseband 6630/6651 units, AIR 6449 massive MIMO antennas, Radio 4449/8843 RRUs, Cisco NCS edge routers, Juniper MX core switches at the MTSO, Ciena 6500 DWDM shelves, CWDM mux/demux at hub sites, fiber patch panels, -48VDC rectifier plants, battery strings, and generator backup.

The core fantasy is competence under pressure. The game never hands the player the answer — it hands them symptoms. Alarms, light level readings, VSWR sweeps, ping tests, and log outputs are the raw material; the player must reason from symptom to root cause before the game reveals whether they were right. Misdiagnosis costs time, parts, customer satisfaction, and sometimes a 2 AM callback. Good diagnosis builds reputation, unlocks higher-tier tickets (commissioning, integration, DWDM work), and eventually a promotion track from L1 tech to senior field engineer.


Gameplay Loop

The loop mirrors a real shift, end to end:


Shift Start / Ticket Reception — Player clocks in at the depot. The in-game laptop (NOC dashboard) shows the active ticket queue: automated alarms (e.g., "Cell Unavailable — eNB 4412, Sector Gamma"), customer-reported degradation tickets, scheduled maintenance windows (MWs), and new-build commissioning jobs. Each ticket has a severity (P1–P4), SLA clock, and site ID.
Triage & Prioritization — Player sorts the queue. A P1 site-down with 40K subscribers affected outranks a P3 single-sector RSSI imbalance — but the P3 site might be on the way, and weather is moving in. Prioritization choices are scored later.
Preparation — At the depot, the player pulls parts against their hypothesis: spare SFPs/QSFPs (LR, ER, CWDM-colored optics), patch cords (LC-LC, single-mode), a spare baseband, fuses, breakers, a rectifier module, jumper cables, weatherproofing kits. Tools are loadout-limited: OTDR, light meter (power meter + VFL), Anritsu Site Master for VSWR/distance-to-fault sweeps, console cable, PPE (harness, hard hat, RF monitor badge, arc-flash gloves). Truck cargo space is finite — overpacking slows you; underpacking forces a return trip.
Travel / Navigation — Fully drivable 3D city in the company truck. Traffic, construction detours, weather (rain reduces visibility and makes rooftop work a safety violation without proper gear; ice storms spawn mass outages). Route planning matters: GPS suggests fastest, but the player can chain tickets geographically.
Site Access & Safety — Arrive, badge/key into the compound or rooftop, complete a safety checklist (RF exposure check near antennas, lockout-tagout before touching the DC plant, harness inspection if climbing is in scope). Skipping checks saves minutes but risks a safety incident — an instant score hit or a shift-ending injury event.
Troubleshooting — The heart of the game. Player inspects equipment in first person: open the baseband cabinet, read LED states, plug the console cable in and read alarm logs, unseat optics and meter light levels, sweep antenna lines, check rectifier output and breaker positions, trace fibers through the patch panel. The game presents data, not answers.
Decision / Repair — Player commits to a fix: swap an optic, re-terminate a fiber, replace a baseband and re-load the configuration, correct a cross-connect at the patch panel, escalate to the transport team (Ciena/Juniper side), or book a tower crew for anything above the safety line (RRU swap at height, VSWR fault in the jumper at the antenna).
Verification — Confirm alarms clear on the laptop, run a speed/latency test from the test UE, verify all three sectors carrying traffic, confirm light levels within spec on both directions.
Closure & Documentation — Write closure notes (multiple-choice quality tiers: thorough notes boost the score and help future tickets at the same site; lazy notes cause "repeat offender" sites to be harder later). Return unused parts, log mileage, clock out — or take the overtime callback.


Persistent layer: parts inventory depletes and restocks weekly, the city remembers chronic sites, weather follows a forecast, and unresolved tickets roll into the next shift with angrier customers.


Core Scenarios / Challenges


Burnt Optic at the Edge Switch — Sector down. RX light level reads -30 dBm at the Cisco edge port (spec: -8 to -14 dBm for that LR optic). Is the optic dead, the fiber bent, or the far-end transmitter? Player must meter both directions and loop-test before swapping.
Bent / Macro-Bend Fiber — Intermittent CRC errors and flapping links. OTDR trace shows a sharp loss event 12 m from the panel — right where a contractor zip-tied a service loop too tight. Visual inspection along the tray confirms it.
Power Failure / Rectifier Fault — Site on batteries, 90 minutes of runtime left, generator failed to auto-start. Player decides: troubleshoot the ATS, manually start the genset, or hot-swap the failed rectifier module while the SLA clock burns.
Improper Configuration — Newly integrated sector won't carry traffic. Alarms clean, light good, but the VLAN tagging on the backhaul doesn't match the MTSO side. Console into the cell site router, compare against the CIQ (Configuration Information Questionnaire), find the mismatch.
Core Cross-Connect Error — The nastiest ticket type: two sites swapped at the MTSO patch panel after overnight transport work. Both sites show "up" but traffic lands on the wrong eNB. Requires correlating site IDs, fiber labels, and DWDM channel assignments at the Ciena shelf — and coordinating with the (NPC) transport engineer.
VSWR / Antenna Line Fault — Return loss alarm on Sector Beta. Site Master sweep shows distance-to-fault at 42 m — that's up the tower, beyond the player's climb authorization. Correct play: document the sweep, book a tower crew, weatherproof what's reachable. Wrong play: ignore it and let the RRU fold back power for weeks.
Signal Interference / PIM — Customers report poor uplink only during dry weather. RSSI floor elevated on one sector. Hunt for the external interferer (a rogue booster in a nearby apartment) or passive intermod from a corroded connector.
Scheduled Upgrade / Commissioning — Maintenance window job: install a new Baseband 6651, migrate fibers per the CIQ, load software, integrate with the OSS, and verify before the 6 AM window closes. Zero alarms at close or the MW is declared failed.
Weather Events — Ice storm spawns 8 simultaneous power outages; player must triage by battery runtime and subscriber count, dispatch portable generators, and accept that not every site survives.
Device Malfunction / Baseband Lockup — Site rebooting in a loop. Logs show a software crash signature. Patch fix (reseat + software reload) vs. full hardware swap — one is faster, one is permanent, and the game tracks which sites come back to bite you.



Chain-of-Thought Reasoning Example

The defining mechanic: before the game allows any repair action, the player must walk a reasoning sequence in the in-game "Field Notes" interface. The game evaluates the quality of reasoning, not just the outcome — a lucky guess scores worse than a sound diagnosis.

Ticket: P2 — eNB 3317 "Rosemont Rooftop", Sector Alpha degraded, link flapping toward the hub. Automated alarm: "Ethernet Link Fault — intermittent."

Player reasoning sequence (entered before any action unlocks):


Symptom restatement: Link is flapping, not hard-down. Sector still partially carries traffic. So whatever is wrong is marginal, not catastrophic — that points away from a clean break and toward a degraded path: dirty connector, macro-bend, failing optic, or marginal light budget.
Topology recall: This site backhauls over dark fiber to the hub 3 km away, CWDM mux at both ends, 1550 nm channel, into a Cisco edge at the hub side. Two fiber strands, two connectors per end, one optic per end — that's the suspect list.
Hypothesis ranking: (a) Failing optic on either end — common, cheap, likely. (b) Dirty/damaged connector at a patch panel — very common after any recent site work. (c) Macro-bend introduced by recent contractor activity. (d) CWDM mux port degradation — rarer. Ticket history shows an HVAC contractor was on the roof Tuesday. That bumps (b) and (c) up the list.
Diagnostic plan: Meter RX light at the site end first (I'm already driving there). If light is low → problem is upstream (fiber/connectors/far-end optic), and I clean-and-inspect connectors before anything else. If light is in spec → the local optic or the baseband port is suspect, swap the optic first since I carry spares. Bring the OTDR in case cleaning doesn't recover the budget.
Parts/tools pulled: One-click cleaner, inspection scope, light meter, two spare CWDM 1550 optics, two LC-LC patch cords, OTDR.


Only after this reasoning is submitted does the game unlock the truck. On site, the player meters RX: -21 dBm against a -16 dBm sensitivity floor — marginal, explaining the flapping. Inspection scope shows contamination on the panel-side connector. Clean, re-mate, re-meter: -11 dBm. Link stable for 15 in-game minutes. Alarms clear.

Outcome scoring: Correct first hypothesis chain, no unnecessary part swaps, root cause documented ("contractor disturbed panel, connector contaminated") → full Accuracy and Efficiency marks, plus a bonus for flagging the contractor activity in the closure notes (which triggers a site-access policy NPC event later).


Sample Session Example

[Scenario placeholder: P1 — Site [SITE-ID] "[Site Name]" completely down. NOC alarm: "eNB Unavailable — all sectors." Subscriber impact: [N]. Weather: [condition]. SLA: [time] remaining.]

— PLAYER REASONING PHASE (Field Notes interface) —


Step 1 — Scope the failure: All sectors down simultaneously. Three sectors don't fail at once from radio problems — this is a common-cause failure: power, baseband, or backhaul transport.
Step 2 — Remote checks before rolling: From the laptop, can the site router be pinged? [Placeholder: ping result]. If pingable → power and transport are alive, suspect the baseband. If unreachable → suspect power or fiber path.
Step 3 — Correlate external data: Check the utility outage map for the neighborhood. Check whether other sites on the same fiber ring alarmed at the same timestamp — if [N other sites] dropped together, this is a fiber cut or DWDM/hub failure, and rolling a truck to the cell site is the wrong move; roll to the hub instead.
Step 4 — Commit to a hypothesis: [Placeholder: chosen hypothesis, e.g., "Single site, unreachable, no utility outage → on-site power failure, likely rectifier or breaker."]
Step 5 — Load-out decision: [Placeholder: parts/tools, e.g., "rectifier module, fuses, DMM, headlamp, generator cables"] — and a contingency item in case the hypothesis is wrong.
Step 6 — Route + safety plan: [Placeholder: route choice, weather gear, whether site access requires escort or after-hours key].


— IN-GAME OUTCOME PHASE —


Player arrives at [Site Name]. First-person inspection: [Placeholder finding, e.g., "DC plant shows two of four rectifier modules dark; battery voltage sagging at 46.1 V; main AC breaker tripped."]
The game now checks the finding against the submitted reasoning. If the hypothesis matched: the repair flow proceeds smoothly — lockout-tagout the plant, [placeholder repair action], verify rectifier output at -54.2 V, watch the baseband boot, confirm all sectors return, run the test UE. If the hypothesis missed: the player must return to the Field Notes interface, update the reasoning with the new evidence, and re-plan — burning SLA time and possibly requiring a parts run.
Closure: Player writes documentation [placeholder: notes quality tier selected], returns unused parts, and the NOC confirms alarm clearance.
Session debrief screen: time-to-restore vs. SLA, diagnostic accuracy (hypothesis vs. actual root cause), safety checklist compliance, parts efficiency, customer-satisfaction delta, and a one-line supervisor comment that reflects the cumulative reputation track.



Summary of Evaluation / Scoring

Performance is tracked per-ticket and rolled into a persistent Technician Reputation profile across five axes:


Efficiency (25%) — Time-to-restore vs. SLA, route optimization, minimizing return trips to the depot. Chained tickets and smart pre-staging of parts earn multipliers.
Accuracy (30%) — The heaviest weight, and it scores the reasoning, not just the result. First-hypothesis correctness, logical diagnostic ordering (cheap/likely checks before expensive/rare ones), and avoiding shotgun part-swapping. A solved ticket with sloppy reasoning scores lower than a tougher ticket diagnosed cleanly.
Safety Compliance (20%) — RF exposure checks, lockout-tagout, PPE, weather-condition judgment, respecting climb authorization limits (booking tower crews instead of free-climbing). Safety violations are the only category that can produce a negative score event and end a shift early.
Customer & Stakeholder Satisfaction (15%) — Subscriber-minutes of outage, proactive communication choices (updating the NOC mid-job, calling the enterprise customer back), and honest ETAs vs. overpromising.
Documentation & Professionalism (10%) — Closure note quality, parts reconciliation, escalation etiquette. Good documentation literally makes future tickets at the same site easier (the game surfaces your own past notes); bad documentation makes chronic sites worse.


Progression: Reputation gates the ticket tiers — new players get optic swaps and battery checks; high-rep players unlock DWDM channel turn-ups at the hub, new-site commissioning during maintenance windows, mentoring an NPC junior tech (whose mistakes you inherit), and eventually a choice between the Senior Field Engineer path (harder technical content) and the Team Lead path (managing the ticket queue and dispatching NPC techs — a meta-game of the same prioritization skills).

Failure states are soft: missed SLAs, chronic sites, and angry supervisors degrade the experience but the shift always continues — exactly like the real job. The only hard fail is a serious safety violation.
