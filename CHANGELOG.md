# Changelog

Everything since round 5 is **untested in the car**. The First drive card and the car test checklist walk through it.

## What the app does now (round 82)

Engine Swap plays one of 38 engines through the car speakers, driven live by the Golf's OBD data (Ancel BD500 with its learned profile, or any ELM327). Everything since round 5 is **untested in the car**; the BD500 and ELM327 conversation is byte-for-byte the round-5 one (checked against simulated adapters), and your round-5 settings carry over.

- **Drive:** big rev dial, the engine with ‹ ›, **🔧 Tuning** (one tap, shows the current mods) and **⇄ A/B**, throttle and boost bars, live car values, Start sound, volume, Pops, Inside/Outside, Record, My sounds. A **First drive** card (pair, test sound, connect, pick an engine, car test checklist) until the first drive is done. **HUD** and **🌙 Dim** in the header.
- **Sound:** Petrol / Diesel engines with ☆ favourites; **Tuning** per engine: exhaust, **turbo on every engine** (size on factory turbo engines, a *Turbo kit* or *Big turbo* on naturally aspirated ones), hot cam, anti-lag; shift light, blips and launch (folded); tone. Rev range and Match a real recording under **Advanced**.
- **Car:** Connect OBD, demo, motion sensor, car-radio controls; then folded: the car test checklist, *Times & drive log* (0–100 / 60–120, drive log, drive trace replay and export) and *Advanced: sound delay*.
- **More:** Help, **🛟 Safe mode** (every extra off in one tap, turbo kit included), **Copy report for Claude**, settings export / import, *Advanced: connection log & adapter settings* (BD500 capture).

**Test first in the car** (parked unless noted): the First drive card on the Drive screen → **▶ Test** sound, **Connect**, drive gently 5 minutes without touching the phone (anything odd: Safe mode), then the checklist and **Copy report**. Then try a **Turbo kit** on a V8 or the V10.

**Round 82: 12 new BMW engines (38 in all)**
- **Petrol:** M50 (E36 325i), M52 (E36 328i / E39 528i), M54 (E46 330i), N52 (E90 330i), N54 twin-turbo (E92 335i), N55 (F30 335i / M135i), B58 (G20 M340i / A90 Supra), B48 four (G20 330i), S55 twin-turbo (F80 M3 / F82 M4), S58 twin-turbo (G80 M3 / G82 M4). **Diesel:** N57 (F10 530d / 535d) and B57 (G30 530d / X5 40d), both with a sport exhaust as built (the M57 stays the straight-piped one). Every six fires 1-5-3-6-2-4, the B48 1-3-4-2; each card's info line gives firing order, idle, redline and induction.
- **Grouped:** on both tabs the BMW engines now sit together under a **BMW** header after the other makes (the S54, S65 and M57 moved into it): NA sixes, M NA engines, turbo sixes, M turbo sixes, the B48; diesels M57, N57, B57. ‹ › follows the same order. Favourites and saved settings are unchanged.
- **How they differ, in the direction the real engines do** (shares of the full-throttle pull; band balance through the output chain, relative to the whole):
  - NA sixes, rasp: M50 −23.0, M52 −23.5, M54 −21.5, N52 −20.5 dB (S54 −21.2). Low end (40-120 Hz): M52 −18.3, M54 −18.5, M50 −19.1, N52 −23.3 dB. Top (2-5 kHz): M52 −26.6, M50 −25.2, M54 −24.2, N52 −21.8 dB. So M52 the deepest and darkest, M50 with the most intake howl, M54 raspier on top, N52 the brightest and most metallic.
  - Turbo sixes, turbo share: N54 −11.5, S55 −11.0, S58 −12.5, N55 −13.5, B58 −14.0 dB (2JZ −13.0). Spool (10/50/90 % boost on a pull): N54 earliest (two small turbos), then B58, N55, S58, S55. The N54 also has a louder blow-off (×1.5) and keeps some flutter on a shift (−14.5 dB vs the pull); N55 and B58 have soft recirculating valves.
  - B58 vs N55: 4 dB more low end (−20.0 vs −24.2 dB), pops on lift-off ×2. S55 vs S58: rasp −18.5 vs −20.0 dB, 2-5 kHz −14.3 vs −16.5 dB, low end −24.7 vs −22.2 dB, pops 1.9 vs 1.7.
  - B48 vs the Golf R: less rasp (−21.0 vs −19.9 dB), 1.1 dB less at 2-5 kHz and 1.6 dB less at 1-2 kHz.
  - Diesels, clatter vs engine at idle: M57 −3.9, N57 −5.5, B57 −9.0 dB; turbo share M57 −9.7, N57 −12.0, B57 −13.0 dB; the B57 also burns more evenly (cycle-to-cycle variation 0.045 instead of 0.06).
- **Calibrated like the others:** pull loudness −13.4 dB (bench), output chain at the median of the older engines of the same fuel (petrol −11.2, diesel −10.6 dB rms, `CHAIN_TRIM`), Inside the same (`INSIDE_TRIM`, petrol −8.6, diesel −7.55 dB), all within 0.15 dB. True peak −1.37..−2.14 dBTP. Turbo kit on the four new NA engines: pull within 0.1 dB of as built.
- Two optional profile fields in the sound engine: `bov` (blow-off level, default 1) and `flutter` (flutter alongside the blow-off, default none). The 26 older engines render bit-identical: their story renders still hash `9dedd18b`; the 12 new ones hash `d5ea0c1b` (the suite checks both). 0 clicks stock, straight pipe, turbo kit / big turbo and anti-lag.
- Demos: `sound-demos/bmw-round82/` (story renders of the 12, plus the S54, 2JZ, Golf R and M57 to compare). Version 82.

**Round 81: Demo mode shifts like an automatic, and a blow-off valve vents without fluttering**
- **Demo mode at full throttle (your report: "like it does lag a bit").** Measured with a new test that drives the page's own demo, car-data and shift code into the sound engine (`tools/demo_test.mjs`, 20 s full-throttle pull, now in the suite):
  - With the rev range on *Diesel TDI (4800)*, the demo's gearbox still shifted at a fixed 5900 rpm, above that range's limiter. So it sat on the limiter before every upshift: **5.4 s of limiter bouncing in a 20 s pull**. Shift points now follow the rev range (up just under the limit, down at 30 % of it): **0.0 s** on the limiter in both ranges, four upshifts instead of three in the diesel range.
  - Every upshift went dead for too long: a 0.22 s torque cut, the simulated boost dropping and building back slowly, and the sound engine guessing the shift from the rpm drop and holding its own cut on top. The sound's engine load stayed under 90 % for **320 ms** per shift, and boost took **1040 ms** to come back on the turbo 4. Now the cut is 0.12 s (DSG-like), the boost barely drops through it, and the demo tells the sound engine about the shift itself (blow-off and clunk right on the shift; it no longer waits for the rpm-drop guess). Load back over 90 % after **180 ms** (60 ms after the cut ends), boost back at 90 % after 380 ms on the turbo 4 (the blow-off vents a little) and not dropping at all on the TDI. All shifts are now seen by the sound engine (before: 1 of 3 in the diesel range).
  - The real-car (OBD) path is unchanged: there the app still spots your own shifts from the rpm drop.
- On a gear change the turbo petrols played two things at once: the blow-off valve venting (immediate since round 77) and compressor-surge flutter ("stu-tu-tu"). Flutter is what happens when the boost has nowhere to go. A car with a blow-off valve vents it instead, so they don't surge together. Measured in isolation (`_test/hq/flt.mjs`: render with and without flutter), the flutter sat at −19 to −25 dB vs the pull on every blow-off engine.
- Now the flutter plays only where nothing vents: diesels, anti-lag tunes, or too little boost for the valve to open. The 2JZ, RB26, Evo, Golf R, RS3, Subaru and RX-7 now vent cleanly on a shift. Diesels keep their flutter, unchanged.
- The turbo spool estimate of round 80 now runs once per audio block instead of every sample. Same curve, cheaper. CPU (desktop bench, worst-case tune, the lowest of 3 runs per engine): mean 7.5 % vs round 68's 8.1 %, worst 14.7 % (round 68: 14.7 %).
- Pull loudness within 0.1 dB of round 68, turbo shares within 0.1 dB of round 80. 0 clicks. The reference hash is now `9dedd18b`. `trans_measure.mjs --shift` also prints a flutter (chop) figure.
- Demos: `sound-demos/hq-round81/` (demo-mode pulls before/after in both rev ranges, and the turbo shift with 150 ms OBD lag). Version 81.

**Round 80: The turbo comes on boost like a turbo**
- Measured the sound's own boost estimate (used when the car reports no boost, and for every turbo kit; new `tools/trans_measure.mjs --spool`). It rose in a straight line with rpm, so on a full-throttle pull every turbo engine already had 10 % boost at about 1100 rpm (practically at idle), and half its boost at half the spool rpm. And the spool lag was a fixed 0.8 s at any rpm: a tip-in at 4400 rpm took 1.8 s to reach 90 % boost, the same as at 2600.
- **Boost threshold.** Now there is almost no boost until the exhaust carries enough energy (about half the spool rpm). Then it comes in over an S-curve and is full at the spool rpm, only with the foot well down. 10 / 50 / 90 % on a pull: 2JZ 2450 / 3010 / 3480 rpm (was 1130 / 2460 / 4020), RB26 2970 / 3640 / 4190, Evo 2520 / 3090 / 3560, Golf R 2040 / 2510 / 2920, Golf GTD TDI 1480 / 1820 / 2080 (was 900 / 1510 / 2430).
- **Spool rate follows the exhaust flow.** The turbine spins up faster at higher rpm and faster once it's going (more boost, more exhaust energy), which gives the rush of a turbo coming on boost. Tip-in to 90 % boost on the turbo 4: 0.73 s at 2600 rpm, 0.34 s at 4400 (was 1.8 s at both). Big-turbo tunes spool later and slower through the same curve, because their spool rpm is higher.
- The boost display (no car boost) uses the same curve. With the car's own boost value nothing changes: the sound follows the car's turbo, as before.
- Because the turbo is now on full boost for more of a pull, each turbo engine's whistle/whoosh gain was trimmed (−1.2 to −1.6 dB), so its turbo share of the pull is where it was (all 13 within 0.1 dB of round 79). On boost, the turbo sits where it did; off boost it's quieter, as on the real thing.
- Pull loudness within 0.1 dB of round 68 (NORM untouched). 0 clicks. The reference hash is now `a23f53fc`.
- Demos: `sound-demos/hq-round80/` (full-throttle pull from idle plus two tip-ins per engine). Version 80.

**Round 79: The idle breathes**
- Measured the crank speed the sound engine actually plays at a warm idle (new `tools/trans_measure.mjs --idle`). It was held to **0.2 rpm** on every engine: the idle-wander noise was averaged sample by sample until almost nothing was left. A real idle is never that still, and the cam lope on the V8s only made the lazy cylinders quieter, never slower.
- **Idle controller.** The rpm now hunts slowly around the target, about 0.5-1 Hz and a few rpm, the way an idle valve or electronic throttle does. It's steadier on diesels, looser with a hot cam or a cold engine, and fades out within 400 rpm above idle.
- **Uneven combustion moves the crank.** Each firing that burns a little weaker or stronger (the existing cycle-to-cycle variation, and the cam lope's lazy cylinders) now nudges the crank speed. The flywheel and idle controller pull it back within about 80 ms. A cammed V8 now lopes in rhythm: the beat sags and recovers. The effect scales with how many cylinders share the torque and is gone 1200 rpm above idle, so cruising and pulls are untouched.
- Measured crank speed now: petrol 5-7 rpm (std), the cammed V8 about 10, the V-twin 11 plus 16 rpm from firing to firing, and the diesels 2.6-2.7 rpm. In the car, the Golf's own OBD rpm wanders on top of this, as before.
- Pull loudness unchanged (within 0.1 dB of round 68, NORM untouched). 0 clicks in the 26 story renders. The reference hash is now `a0fec745`.
- Demos: `sound-demos/hq-round79/`. Version 79.

**Round 78: Inside view as loud as before, per engine**
- Through the full chain (`chain_test.py --inside --only all` against round 69), the Inside view had drifted by up to 0.9 dB per engine since round 70's body-transmission model: Golf GTD (tdi20) −0.9 dB, LFA and 458 −0.6, V10 −0.5, the rest −0.4…+0.3.
- Each engine now has a small extra trim in front of the chain while Inside is selected (`INSIDE_TRIM`, −0.5…+1.8 dB; the chain's compressors absorb part of it, so the trim is larger than the gap). It glides in 0.15 s when you switch view, together with the Inside filtering. All 26 engines are now within 0.2 dB of round 69 Inside, and true peak still passes.
- The engine sound and Outside are unchanged (renders still hash `5e43f6df`). `chain_test.py --inside` now applies the Inside trim the way the app does.
- Demos: `sound-demos/hq-round78/`. Version 78.

**Round 77: Blow-off right at the gear change**
- On the turbo petrols (without anti-lag), the blow-off valve waited for the engine's smoothed load to drop under 15 %. With the car's OBD load lagging 150 ms, it vented **312 ms** after the app spotted the shift. On an instant lift it often never vented at all, because the boost had already bled under the threshold.
- Now it vents the moment the shift is spotted (within the next 0.35 s, if there is still boost): **3 ms** in both cases, on all seven turbo petrols with a blow-off (i4t, i6t, evo, rb26, i5, ej25, rot). A normal lift without a gear change is unchanged. Diesels and anti-lag engines have no blow-off, as before.
- Loudness unchanged (the pull isn't affected); 0 clicks; the reference hash is now `5e43f6df`. Demos: `sound-demos/hq-round77/`. Version 77.

**Round 76: Less fizz on the M3 and the S2000**
- Since round 18 the suite has flagged two engines whose straight-pipe gas rasp sat well over their character target: the BMW S54 (E46 M3) at a rasp share of -19.2 dB (target -22) and the Honda F20C (S2000) at -18.0 dB (target -20). On Bluetooth that extra 2-5 kHz rasp comes out as fizz on top of the howl.
- Rasp brought to 1 dB over target (S54 −21.0, F20C −19.0 dB), deliberately not all the way, so they keep their edge: S54 gain 0.728 → 0.593, F20C 0.597 → 0.534. Through the output chain, 2-5 kHz −0.6 / −0.3 dB and > 8 kHz −1.0 / −0.3 dB. All character targets are now within 1.5 dB.
- Loudness unchanged: pull within 0.1 dB of round 68 (NORM untouched) and the chain level identical (S54 −11.2, F20C −12.2 dB). The other 24 engines are untouched. 0 clicks; the reference hash is now `4ab2d578`.
- Demos: `sound-demos/hq-round76/`. Version 76.

**Round 75: Gear changes: an instant lift and a driveline clunk**
- Measured a gear change with the car's real OBD lag (`trans_measure.mjs --shift --lag 150`: the load still reads full for 150 ms after the app spots the shift). The sound kept pulling into the shift and only fell 6 dB after 245-290 ms, and the clutch biting again made no sound of its own.
- **Instant lift.** The app spots a shift only once you have lifted, so the torque is now cut the moment it does, and held until the car's load value shows the lift too (at most 0.35 s). Measured: -6 dB after 75-170 ms on most engines (v8x 245 → 170 ms, m57 290 → 75 ms). The turbo 4 takes 220 ms because of its whoosh.
- **Driveline clunk.** When the load comes back after the shift, the driveline lash is taken up and the engine rocks on its mounts: a short, low thud (44-58 Hz, ~45 ms, with a brief knock on its front edge). It sits at the same level on every engine, -13 to -16 dB below the pull, and is 1.6× stronger in the Inside view (structure-borne). It plays only after a lift the car has confirmed, so lagging load data cannot set it off early.
- Nothing outside a gear change changes: pull loudness and the output chain are as before. 0 clicks in the 26 story renders and both turbo-kit sets, no NaN. The reference hash is now `bd674f5d`.
- CPU (desktop bench, worst case, nothing else running): mean 9.6 → 10.0 % and worst 15.1 → 15.7 % of real time vs round 68 (+4 %).
- Demos: `sound-demos/hq-round75/`. Version 75.

**Round 74: Overrun burble with late combustion in the pipe (Pops & bangs on)**
- Measured the lift-off with Pops on (`trans_measure.mjs --overrun --pops`, 3 seeds). Each pop was a louder copy of a normal exhaust pulse, fired at an even random rate: about 40 bangs per second on the V8s, the turbo 4 and the RX-7, spread evenly (clustering factor about 2, where 1 is perfectly even). A machine-gun rattle.
- Now about a quarter of those pops become **late combustion**. The lean overrun charge that didn't burn in the cylinder lights 1-8 ms later in the pipe. That gives a round, low "bup" (a pressure wave 2.5-5 times slower than a firing pulse, with a smooth onset), and 60 % of the time a short **crackle** tail: sparse ticks for about 40 ms, rounded to about 4 kHz so they crackle without being digital clicks.
- Pops and late combustion come in **clusters**: the mixture wanders rich and lean every ~0.2 s, so there are bursts of burble and quieter stretches. Clustering factor about 2.0 → 3.4 on average, similar or fewer bangs per second, the same overall level.
- Late combustion needs a hot exhaust. After a hard pull the lift-off burbles; after gentle cruising it stays cleaner, as on a real car. Petrol only.
- Click detections with Pops on: 5 in 18 renders (round 73: 7), i.e. no new clicks. Pops off: nothing changed; renders still hash `279fef5e`, and loudness is unchanged.
- Demos: `sound-demos/hq-round74/`. Version 74.

**Round 73: The rev limiter bounces like a real one**
- Measured the limiter with a new tool (`tools/trans_measure.mjs --limiter`, 5 ms loudness envelope held on the limiter). Every petrol engine dropped 75 % of its firings at random, at a flat rpm. That is a rough, hollow buzz with no rhythm: no bounce rate stood out (periodicity 2.3-4, where 1 is pure noise), and it was 3-5 dB quieter than the pull.
- Now it works like an ECU fuel cut. Fuel goes off at the limit and comes back about 2 % lower (the gap varies ±20 % per cycle). With no torque the rpm falls, with full torque it climbs back, so the engine bounces in that band and you hear the pitch go "bap-bap-bap". Measured: a clear bounce at 9.5-9.8 Hz on the V8s and the turbo 4, 10.3-11.5 Hz on the high-revving engines (periodicity 7-18), and only 0-2 dB quieter than the pull. A few cylinders still fire during the cut (10 %), as on a real hard cut.
- Pops & bangs and anti-lag still fire on the cut firings; launch control uses the same cut band at its launch rpm. Diesels keep their fuel governor (no cut), unchanged.
- The rest of the sound is unchanged: renders still hash `279fef5e` (the limiter isn't in the story render), and loudness is unchanged.
- Throttle stab, measured too (`--attack`): petrol engines reach 90 % of the full-load level in 20-40 ms with a short bark (1.5-4.6 dB overshoot). That is already where a real engine is, so it was left as it is.
- Demos: `sound-demos/hq-round73/` (before / after, with and without pops). Version 73.

**Round 72: The low end no longer cancels on a mono speaker**
- Measured how each engine folds down to mono (left + right summed, which is what one speaker, a summed woofer, or the cabin itself at low frequencies plays). Inline and single-exhaust engines were fine, but the two-bank engines were not: the cross-plane V8s lost **5.5 to 7.7 dB below 120 Hz** (LS3 −5.5, Coyote −6.0, C63 −7.3, S65 −7.3, RS4 −7.7) and the V10 / V12 / LFA **3.4 to 6.9 dB at 120–500 Hz**. Their banks were partly in anti-phase between the left and right speaker, so in stereo the doors played anti-phase bass, and summed the rumble disappeared.
- Fix: below about 800 Hz both speakers now get the same low end (the nearer tailpipe's), above that the stereo image is as before. Mono fold-down now: V8s −0.1 to −0.9 dB below 120 Hz, V10 / V12 / LFA −1.8 to −2.4 dB at 120–500 Hz (no worse than two unrelated sounds), all others unchanged. Each speaker plays the same level as before.
- Only the two-bank engines (V8, V10, V12, flat-6, split-manifold sixes) get this; single-exhaust engines with twin tailpipes never cancelled and are unchanged.
- The turbo-kit bench's peak check, taken *before* the output chain, now allows 1.35 instead of 1.2 (shared low end lines up the peaks a little more: LS3 big kit 1.21, RS4 kit 1.31). What reaches the speakers is held by round 70's oversampled ceiling, and the suite's chain test checks the true peak there (≤ −1.36 dBTP).
- Loudness recalibrated (NORM and CHAIN_TRIM): every engine within 0.1 dB of round 71 through the chain. 0 clicks. The reference hash is now `279fef5e`.
- Demos: `sound-demos/hq-round72/` with stereo and MONO files. Tools: `node tools/hq_measure.mjs --mono`. Version 72.

**Round 71: Every engine as loud as before through the new output chain**
- Round 70's chain (bass compressor, gentler glue, limiter + ceiling) changed the loudness per engine: high-revving engines with little low end came out quieter (Aventador V12 −2.3 dB, LFA −1.9, V10 −1.8, 458 −1.7, RS4 −1.3, RX-7 −0.8), the Subaru boxer 1 dB louder. Each engine now has a small fixed trim in front of the chain (`CHAIN_TRIM`, −1.5 to +3.5 dB), measured through the real chain: all 26 engines within 0.1 dB of their round-69 level (pull), true peak still −1.36 dBTP or lower. Inside: within −0.9..+0.3 dB of round 69 per engine.
- The engine sound itself is unchanged (renders still hash `bd45d173`). Swapping engines while running, the trim glides in 30 ms during the swap fade.
- `tools/chain_test.py`: the sound engine's randomness (pops, bangs) is now seeded like the bench, so runs are repeatable; `--only all` runs all 26 engines.
- Demos: `sound-demos/hq-round71/`. Version 71.

**Round 70: Output chain for Bluetooth and car speakers, a more real Inside view**
- **True-peak headroom.** The old limiter (−2 dB threshold, no look-ahead ceiling) let the output reach −0.1 to −0.5 dB *true peak* (between samples) on every engine. Through the phone's lossy Bluetooth codec that is where crunchy clipping on bangs comes from. Now a gentler limiter (−1.5 dB, 5 ms attack, 250 ms release) feeds a 4× oversampled soft ceiling at −1 dBFS. True peak measured in headless Chrome: −1.4 to −1.9 dBTP on all engines, Outside and Inside. No measurable distortion added on pops and bangs (> 8 kHz energy added by the chain below −85 dB).
- **Bass management.** A 4th-order speaker-protection high-pass at 32 Hz (was 2nd order at 28 Hz: no excursion wasted below what a car woofer plays), then the 40–120 Hz band (Linkwitz-Riley split at 120 Hz) gets its own slow compressor. Big low ends sit steadier and don't overload the woofer (V-twin 40–120 Hz share −3.1 → −5.3 dB, Cummins −3.8 → −6.0 dB), small engines get a solid low end (2JZ −26.6 → −23.5 dB, S2000 −19.4 → −16.8, V10 −32.1 → −28.4). Full-load pumping unchanged (gain wobble 0.4–3.5 dB, as before); V-twin Inside 7.0 → 4.0 dB. Overall loudness during a pull within 0.5 dB of round 69 for most engines; the high-revving ones with little low end come out a bit quieter (RX-7 −0.8 dB, V10 −1.8 dB). The bass slider works as before.
- **Inside view.** Instead of a plain 1.5 kHz low-pass over everything, the sound engine now models the body: the firewall, floor and doors losing about 6 dB per octave above ~2 kHz (mass law), the glass and seals leaking a little at 2–3 kHz, and the Golf cabin's booms (42/78/118/165 Hz) on top as before. Measured: the boxy 1–2 kHz honk 4–5 dB lower, 2–5 kHz 2–4 dB more presence. Safe mode keeps the round-5 Inside filter.
- The Outside sound of the engines is unchanged (renders still hash `bd45d173`).
- Demos: `sound-demos/hq-round70/` (before/after, Outside and Inside, through the full chain).
- Tools: `tools/chain_test.py` renders engines through the app's real output chain in headless, muted Chrome (offline, nothing plays) and measures true peak, pumping, distortion on pops, the 40–120 Hz band; `--old` for the round-69 chain, `--inside`, `--wav`. In the suite (true peak must stay below −0.5 dBTP).
- **Unchanged:** the BD500 / ELM327 code, the controls, saved settings. Version 70.

**Round 69: Cleaner, more real engine sound (all 26 engines)**
- **Band-limited exhaust pulses.** Every exhaust pulse starts with a sharp corner, and at high rpm the shortest pulses (rotary, VTEC, V10s, flat-plane V8) folded part of their top end back down as inharmonic "fizz" (aliasing). Each pulse onset is now corrected sub-sample (polyBLAMP). Aliasing at high rpm, measured on a deterministic render: RX-7 −54 → −68 dB, S2000 at 8190 rpm −57 → −68 dB, V10 −56 → −69 dB, Ferrari 458 −59 → −69 dB, 2JZ −67 → −75 dB. The saturator was checked too: it was not the source (first-order anti-aliased already).
- **Combustion noise is no longer white hiss.** The gas noise of each firing was independent white noise per cylinder, switched on at full level at the instant the valve opens. Now each exhaust bank has one band-limited noise stream whose colour follows the pulse (a slow, soft pulse = darker noise, 1.8–8 kHz), with a soft ~0.3-pulse-width attack. The pipes and mufflers sound like air moving, not static. Combustion noise above 5 kHz −3 to −18 dB (M57 straight pipe −12 dB in 5–8 kHz and −18 dB above 8 kHz), the low and mid texture unchanged. With *Pops & bangs* on, the start of a pop is no longer a click: 1–2 click detections per 26 renders instead of 19–25.
- **Cycle-to-cycle variation.** Every combustion burns a little faster or slower (±6 % pulse width, twice that near idle), like a real engine: the idle and the burble breathe instead of repeating identically.
- **Diesel clatter is a knock, not a hiss.** The steep pressure rise of a diesel combustion is now a short rounded tick that rings the block and head modes, with a lower body mode for weight, instead of a noise burst through two narrow filters. Clatter level per diesel kept the same as round 68 (within 0.6 dB). The fizz above 3 kHz at idle is 7–8 dB lower (M57 −9.0 → −17.2 dB, Golf TDI −10.2 → −17.5 dB).
- **Turbo whistle lives.** The compressor tone has a tiny slow pitch wobble (±0.5 %, a real shaft never spins perfectly evenly) and its 2nd harmonic fades out above 7 kHz (less shrill through Bluetooth).
- **Outside has a space.** A faint ground bounce and a few nearby walls (2–41 ms, highs and lows rolled off so the bass stays tight), like hearing the car in a street instead of an anechoic room; much thinner in the Inside view (your real cabin adds its own). Off in Safe mode.
- Loudness per engine recalibrated (NORM): every engine's full-throttle pull within 0.1 dB of round 68, idle/cruise levels as before. 0 clicks on the 26 test renders and both turbo-kit sets, no NaN. CPU about +10 % in the worst case (psd73 straight pipe + big turbo: 14.1–14.7 % → 16.1–17.1 % of real time on the desktop bench).
- Before/after demo WAVs: `sound-demos/hq-round69/` (local, not in git: M57, 2JZ, S2000, LS3, 458, RX-7, Coyote with the turbo kit) with a README of what to listen for.
- Tools: `tools/hq_measure.mjs` (aliasing, band balance, crest, stereo correlation, idle noise floor); `PAGE=old.html node tools/synth_bench.mjs …` runs the bench on another copy. The suite's reference hash is now `bd45d173`.
- **Unchanged:** the BD500 / ELM327 code (bd500_compare identical to round 5), the controls, the saved settings. Version 69.

**Round 68: Rotary straight pipe without the idle click**
- With the straight pipe, the Mazda 13B rotary sometimes clicked at idle just after starting (in 5 of 8 test renders, for example at 3.13 s). Cause: every firing starts a short burst of exhaust noise, the rotary's pulses are the sharpest of all engines (0.46 ms) with a lot of that noise, and the straight pipe opens its muffler to 4960 Hz, so the start of the burst came through as a click when the idle is quiet. As built (sport exhaust) it never clicked.
- Fix, for the rotary's straight pipe only: muffler opening 4300 Hz instead of 4960, pulse noise 0.18 instead of 0.36. 0 clicks in 20 renders, still the loudest and brightest of its exhaust options (+1.8 dB over sport). No other engine or exhaust option changed: all 26 engines as built still render bit-identical (hash 6483c6c5), and nothing in the sound engine itself changed.
- The test suite renders the turbo kit with straight pipe and anti-lag again (round 67 had left straight pipe out because of this click): 0 clicks on all 26 engines. Version 68.

**Round 67: A turbo on every engine, and a tidier app**
- **Turbo for every engine.** Sound → Tuning always shows a *Turbo* row now. Factory turbo engines keep *Small · quick / Stock / Big · laggy*. The 13 naturally aspirated engines get *None (as built) / Turbo kit / Big turbo*: a turbo layer (whistle, whoosh, blow-off, gear-change flutter) whose full-boost rpm comes from the engine's size and redline (LS3 kit ≈ 2500 rpm, F20C ≈ 4250, big turbo about 35 % later) and whose level sits over the exhaust like on the factory turbo petrols (turbo share −12 to −14 dB). Saved per engine like the other tuning; *None* is the default, so nothing changes until you pick it. Anti-lag becomes available with a kit. A line under the control says what it does and at what rpm it reaches full boost.
- **Boost follows the swapped engine:** the Drive screen's Boost bar and value, the HUD and the drive log's peak boost show the swapped engine's boost: the car's own boost when it reports it and the engine has a turbo (factory or kit), else the same estimate the sound uses; *–* for a naturally aspirated engine without a kit. The sound itself gets the car's boost exactly as before.
- **Drive screen:** a **🔧 Tuning** button next to A/B shows the current mods (*turbo kit*, *straight pipe, hot cam*…) and opens the Tuning card in one tap.
- **Merged:** *Get started*, the *🧪 Car test* card and the checklist's *First car test* box are one **First drive** card (steps 1–4, then the checklist as step 5). After the first connect it shows only while the checklist is unfinished; *Hide* hides it for good, and a Car test card you hid before stays hidden.
- **Removed:** the *What's new* card (this changelog and the update card cover it; Help links here), the *In the car* card (in Help → Quick start), the separate *Safety* card (one line at the top of Help), unused CSS.
- **Moved / folded:** Sound → *Advanced* holds Rev range (with the manual RPM scale) and Match a real recording. Car → *Times & drive log* (timer, drive log, replay, exports) and *Advanced: sound delay* are folded. More → *Advanced: connection log & adapter settings*.
- Help → *What each feature does* rewritten for this layout. Version 67.
- **Unchanged:** the BD500 / ELM327 code (bd500_compare identical to round 5), Safe mode, Copy report, the engine sounds as built (renders still hash 6483c6c5). Old saved keys that are no longer used (`seenVersion`) are simply ignored.
- Tests: `synth_bench.mjs --kit` (each naturally aspirated engine with the kit and the big turbo: pull loudness within 1.5 dB of as built and inside the stock range, turbo share, peak, NaN); the suite renders the kit (with straight pipe and anti-lag) through `audio_qa` (0 clicks). The smoke test checks the turbo row on all 26 engines, the kit, the boost readouts, the Drive button and Safe mode; the upgrade test checks the First drive card with round-5 settings. Every browser test now runs with `--mute-audio`.

## Rounds 56–66 (overnight, also untested in the car)

**Round 66: The ELM327 conversation is unchanged too; the "stalls" were a test artefact** (tests only)
- `tools/bd500_compare.py` now also compares round 5 and today on a plain ELM327: the same setup commands, the same requests in the same order, the same 5 requests while running, the same rate, the same values, and the same after a reconnect. On an ELM327 whose car answers one value per request, today's app differs on purpose: since round 38 it also reads speed, throttle and coolant there (round 5 read only rpm). That case is shown, not judged.
- The 150–220 ms pauses `perf_test.py` reported since round 61 are not garbage collection while driving. They happen once, about a third of a second in, when the screen is first drawn on the slowed CPU, even with an empty drive trace. After a 2-second warm-up, 9 runs of 30 s had no pause over 50 ms except one 59 ms trace save (every 20 s). So the hot paths were left alone, and the long-task limit is back to 150 ms. The test also prints when each pause happened and takes `--trace-rows`.
- No app change.

**Round 65: One command for every check** (tools only)
- `bash tools/run_suite.sh` runs the whole test suite in the order used before every push since round 57 and prints `RESULT fail=0` when everything passes. The README says so.
- No app change.

**Round 64: Teaching the BD500 again gives the same result** (test only)
- `tools/bd500_compare.py` also feeds the same synthetic Bluetooth capture to the round-5 and today's capture analyser: both learn the identical adapter profile (the name, which holds the date, aside). So if you ever re-teach the BD500 with today's app, you get what round 5 would have learned.
- No app change.

**Round 63: Proof the BD500 conversation is unchanged since round 5** (test only)
- New `tools/bd500_compare.py` runs the round-5 build (8f2f2ec, the one tested in the car) and today's build against the same simulated BD500, with the same learned profile. It records every frame each one writes to the adapter. Both send the same init frame, the same 30 first frames in the same order (the extra-value probes included), the same 6 frames while running, at the same rate (24 frames/s against the simulated adapter), read the same rpm and speed, and do all of it again identically after the adapter drops and the app reconnects by itself. Stable over 3 runs.
- A deliberately changed copy (one probe asking for a different value) fails it, so the check does catch a change.
- No app change.

**Round 62: Data gaps in the report's first line**
- When the car's data paused for more than a second during the session (but less than 2.5 s, so not ignition off or a reconnect), the report's *At a glance* line now says so, for example *Car connected (learned adapter profile, 9 updates/s) · 2 car data gaps over 1 s*. With no such gaps the line is unchanged.

**Round 61: Connecting with the log open no longer stalls the phone**
- Every log line made the browser lay out the whole log again (up to 40,000 characters) just to scroll it to the bottom. That only costs something while the log is visible, which is when you'd watch it: More tab, *Connection log* open, during a connect, when the adapter's raw replies are logged (about 40 lines). On a CPU 4× slower that blocked the main thread for **1.6 s**, right while the adapter's first answers arrive. Now the text is still written at once, but the log scrolls once per screen refresh: **20 ms**.
- Opening *Connection log* now jumps to the latest lines.
- `tools/perf_test.py` checks this (limit 300 ms), shows saveTrace timings during the run, and takes `--page` to run an older copy. Its long-task limit is now 300 ms, because an occasional 150–220 ms pause (round 60 shows it too) is most likely garbage collection, not the app's own code.
- Nothing you hear changed. Renders still hash 6483c6c5, the connection code is untouched (only the shared log helper changed).

**Round 60: Help for jumpy revs** (text only)
- Help → Troubleshooting → *Revs jump or the sound drops to idle*: how to read the report's *Car data gaps* and *Phone stalls*, and what to try for each.

**Round 59: The report says if the adapter or the phone was slow**
- *Copy report* → Car & phone has a new line: the longest gap between two rpm updates from the car, how many gaps were over 0.3 s and over 1 s, how many were over 2.5 s (the app then lets the engine sound fall to engine-off until data comes back; switching the ignition off counts too), and how many times the phone's processor was stuck for more than 50 ms. If the sound lags or cuts out on the first drive, this shows which side caused it.
- Counting only, nothing changes how the app behaves. Safe mode doesn't count. The connection code is unchanged: the counter sits in the car-data handler that every source already uses.
- `obd_test.py` checks the counter on the simulated ELM327 (92 updates, longest gap 17 ms), the smoke test checks the report line.

**Round 58: Lighter HUD and Car tab**
- The HUD only rewrites the numbers and shift-bar lights that changed (it rewrote all of them 20 times a second): 19 % → 16 % main thread on the 4× slower CPU.
- The Car tab's drive-trace line copied the whole 20-minute trace (24,000 rows) four times a second to count it. It now counts in one pass without copying, and the drive-log numbers are only rewritten when they change: 10 % → 8 %. (That line is a screen-reader live region, so it is also better not to rewrite it when nothing changed.)
- `tools/perf_test.py` now checks the Drive screen, the HUD and the Car tab in one run.
- Nothing you see or hear changed. Renders still hash 6483c6c5, the connection code is untouched.

**Round 57: Lighter Drive screen, connect advice clears**
- New `tools/perf_test.py` runs the app on a CPU slowed 4× (roughly a mid-range Android phone) with the sound on and car data arriving 20 times a second in real time. The Drive screen kept the main thread (which also receives the adapter's Bluetooth replies) **53 % busy**. Most of it was three CSS transitions: the dial's arc animated `stroke-dashoffset` at 60 fps (−23 points), the glow animated a `drop-shadow` filter (−6) and the bars animated their width. The display already updates 20 times a second, so these 60–100 ms transitions smoothed nothing you could see. Removed: **16 % busy**, no long tasks. HUD 19 %, Car tab 10 %.
- The view checks in the display timers use the tab's class instead of `offsetParent` (same result, no forced layout).
- **Fixed:** after a failed first connect (for example the adapter list closed), the advice under *Connect OBD* stayed there even after the car connected. It now goes back to the normal text once connected. `obd_test.py` has a scenario for it (failed on round 56).
- No sound change (renders still hash 6483c6c5), the BD500 / ELM connection code is untouched, Safe mode unchanged.

**Round 56: Docs match the app** (text only)
- The note under *Copy report* (More tab) mentions the summary and the errors. The Safe mode description lists replay speed. The README gets a Safe mode row. This Morning summary was added.

## Rounds 41+ (also untested in the car)

**Round 55: Help catches up** (text only)
- Help → Troubleshooting → *Won't connect* explains an empty adapter list: Location off, or Chrome missing the *Nearby devices* permission, and where to allow it.
- Help → *Timer & drive log* mentions replay's ½× / 2× and the position bar.

**Round 54: Replay speed and seeking, clearer Bluetooth problems**
- **▶ Replay drive** (Car tab): while it plays, pick **½×, 1× or 2×**, and drag the bar to jump to any part of the drive. Safe mode keeps replay at 1× and hides these controls.
- When Bluetooth itself is the problem, the Car tab now says what to do, right under Connect (not only in the log):
  - phone Bluetooth off: switch it on;
  - Chrome not allowed: Android Settings → Apps → Chrome → Permissions → allow Nearby devices and Location;
  - the adapter list closed or stayed empty: switch on Bluetooth and Location, allow Nearby devices, ignition on;
  - the adapter refused the connection: close the Ancel app, ignition off and on.
  If the phone reports Bluetooth is off when the app opens, the Car tab says so, and the hint goes back once it's on. The Samsung Internet and Firefox message now names Chrome.
- **Fixed:** tapping *Test sound* twice (the Get started and checklist buttons) could start two tests at once. Only one runs now.
- Reconnecting after a drop and the BD500 path are unchanged. The new messages only appear when a first connect fails.

**Round 53: Odd saved values can't break the app**
- **Fixed:** a settings file with an unexpected value in it (edited by hand, or from another app) could stop *My sounds*, the times list or the checklist from showing, and could break *Copy report*. Saved sounds, times, tuning, checklist results and drive totals now fall back to empty when a value has the wrong shape. Good values are never touched.
- The smoke test now stores seven such values, reloads, and checks that everything still shows (the old code failed it). Upgrades from rounds 5, 10, 20, 30 and 40 still pass.

**Round 52: Plainer words** (text only, the sound is unchanged)
- Help → *What each feature does* drops the sound-engineer terms: Bass, Aggression, Turbo loudness, Louder with speed, Inside and Match a real recording now say what you will hear.
- Checklist: *Connect* no longer counts fewer than 5 updates/s as a failure (a slower adapter still works). *Delay* asks you to rev in neutral and check that the sound keeps up.
- Copy report: the summary no longer says "not connected" twice, and each problem you ticked shows your note next to it.

**Round 51: Easier report, and the first car-test step works on your phone**
- **Fixed:** the checklist's *First car test* step 1 said "Get started → ▶ Test". On your phone the Get started card is hidden, because you connected to the car with the round-5 app. The step now has its own **▶ Test sound** button. Tapping Test sound while the car is connected (or a replay is playing) now says why nothing happens, instead of doing nothing.
- **Copy report** starts with an *At a glance* line: is the car connected (and how many updates per second), is the sound playing, Safe mode, how many errors, and how far the car test got. Problems you ticked are listed by name right under it. The rest is grouped into Sound, Car & phone, Drive, Car test, Errors and the log, with a blank line between groups. Nothing was removed.
- Help → *Lost connection* now says how reconnecting really works: every 4 s at first, every 15 s after 2 minutes, and it gives up after 30 minutes without the car.

**Round 50: Long-drive soak test** (no app changes)
- New `tools/soak_test.py` simulates a long drive (60 minutes by default, `--minutes 120` tried) through the app's real car-data path, with the sound running. It covers pulls through the gears, cruising and stops.
- Results: memory grows 1.4 MB and then stays flat (that's the 20-minute drive trace filling up). Page size, event listeners and the log stay constant, nothing throws, storage uses 1.3 MB, all shifts are detected, and the sound still plays at the end.

**Round 49: Errors go into the report**
- Anything that goes wrong is now kept with the time, the message and where it happened, and *Copy report* lists it under **Errors**. That covers program errors, failed background tasks, and the sound processor crashing (which means silence). It keeps up to 20 different ones and also shows a ⚠ line in the log. A failure during the car test can now be diagnosed from the pasted report.

**Round 48: Updates arrive cleanly**
- **Fixed:** after an update, opening the app could still load the old version for up to 10 minutes (the browser's own cache). The new offline copy was then filled with that old page, so offline it stayed old until the next online start. The offline worker now always checks with the server (a tiny "unchanged" reply when nothing changed) and installs fresh copies.
- If a new version arrives while the app is open, a **New version ready · Reload** card appears on the Drive screen. The app never reloads by itself, because that would cut the sound mid-drive. It also checks for updates when you come back to it, at most once an hour.
- New `tools/update_test.py` covers this, with GitHub Pages' caching. The upgrade test now also starts from v10, v20, v30 and v40, with tuning, favourites, checklist results, timed runs, drive totals, a drive trace and a learned BD500 profile stored. Everything carries over.

**Round 47: First-run and upgrade checks**
- The upgrade test now starts from the real round-5 build (8f2f2ec). Earlier notes called 8a07b80 round 5, but that is round 6; its car-connection and storage code is the same. Both upgrade paths pass.
- On a factory-fresh phone the first screen is *Get started* (pair, test, connect, pick an engine, or the demo), with no What's new card. Tapping Connect starts a normal Bluetooth connect, and no errors show.

**Round 46: Upgrade safety and a shorter What's new**
- New `tools/upgrade_test.py` sets up the round-5 app (the one tested in the car) the way you had it, then opens today's app on the same phone storage. It checks that the engine, fuel, Inside, tone sliders, car-radio mode, the learned BD500 profile (byte for byte), the remembered adapter and My sounds all carry over. It also checks that no saved value is rewritten and the new extras start off.
- **Fixed** (the test found it): a saved sound for an engine the app doesn't have (from an old or edited file) left the engine blank, and then *Start sound* failed. Tapping one now just says so.
- What's new only lists what's new since the version you last opened. A first install skips it, so *Get started* is the first thing a new phone shows.

**Round 45: Drive log export** (Car tab → *⬇ Drive log & times (CSV)*)
- One file with this drive's totals (top speed, peak rpm, km, minutes, shifts, peak boost) and every saved 0–100 / 60–120 time, with date, engine and whether it was the car or the demo. It opens in any spreadsheet.

**Round 44: Fixes from an audit**
- During the delay test the engine is turned down so the clicks are clear. A speed change (demo, or rolling) could turn it back up. It now stays down until the test ends.
- Replay used to copy the whole recorded drive 50 times a second to find the next sample (up to 24,000 rows each time). It now just steps forward, which is much lighter on the battery during a long replay. The button's clock only redraws when it changes.
- Replay no longer works out an rpm slope across a skipped gap (it could make *Predict ahead* jump once after a gap).

**Round 43: Accessibility**
- Screen readers (TalkBack) hear which tab, fuel, Inside/Outside, N/D and tuning button is selected, and status changes (connection, delay result, drive trace, recording match) are read out.
- A clear orange outline shows where the focus is when you use a keyboard or switch access.
- If the phone is set to *Remove animations*, the blinking Record button and arrow animations stop.

**Round 42: Better shared clips** (⏺ Record)
- The file is named after the engine, plus *inside* and any mods, e.g. `engine-swap-2JZ-inside-straight-pipe-big-turbo-….webm`.
- Sharing adds a line that says what it is: the engine, inside or outside, mods, length, top rpm, whether it was your Golf or a replayed drive, and the app link.
- The clip fades in and out (20 ms in, about 0.1 s out), so it no longer starts or ends with a click.

**Round 41: Sound delay** (Car tab)
- **⏱ Measure delay**: parked, with the phone playing through the car, tap the big button on each click you hear (14 clicks, the first 3 are ignored). It shows the delay from the phone to the car speakers and what the phone itself reports.
- **Predict ahead** (0–150 ms): plays the rpm a little ahead along its trend, so revving up sounds less late. Measuring sets it to about half the delay. It is **0 (off) until you measure**, and Safe mode always turns it off.
- On a recorded drive heard 250 ms late, 150 ms ahead halves the typical rpm error (48 → 25 rpm). Gear changes don't get better, since a shift can't be predicted.
- The delay and the predict-ahead value go into the report. The checklist's *Delay* test now uses it.

## Overnight, rounds 22–40: getting ready for the car test

No engine sounds changed in these rounds.

**New things to try**
- **🛟 Safe mode** (More tab): if anything misbehaves in the car, one tap turns off every extra added since round 5. That's tuning, blips, launch control, shift beep and flash, cabin booms, the motion sensor and car-radio mode. Your settings are kept.
- Car tab: **The car sends** line shows which values arrive from the car (rpm, load, pedal, speed, boost, coolant) and what the sound does without the missing ones. It's also in the report.
- The checklist starts with a **first car test** card: 5 things to do in order.
- **Help** (More tab): quick start, how to teach the app the Ancel BD500, what every option does, troubleshooting.
- **Get started** now has a *Test the sound* step: a 3-second rev so you can check it comes out of the car speakers and set the volume.
- **Car test checklist** (Car tab): 17 short tests (parked, driving, after the drive). Tap ✓ Works, ✗ Problem (with a note) or – Skip.
- **Copy report for Claude** (More tab): one tap copies the app version, settings, adapter details, audio delay, checklist results and the recent log. Paste it in the chat.
- **Drive trace** (Car tab): the app records the car's rpm, load, pedal, speed and boost while you drive (last 20 minutes).
  - **Replay drive** plays it back through the sound while parked, so you can hear other engines on your own drive.
  - **Export (CSV)** saves it so the sound can be tuned against your real driving.
- **☆ Favourite engines** (Sound tab): with two or more, ‹ › and the steering-wheel next/previous buttons switch between favourites only.
- **Settings export / import** (More tab): one file with all your engines, tuning, My sounds, adapter profile and times.

**Fixed**
- After *Stop sound → Start sound*, Record and the Car-radio controls stopped working.
- Tapping Disconnect during a slow connect could still end up connected; a pending reconnect could stop the demo.
- A Bluetooth connect that hangs now gives up after 15 s and tries again.
- Reconnecting slows down after 2 minutes (every 15 s) and stops after 30 minutes without the car, to save battery.
- After a long phone call the sound comes back by itself (it used to give up after 30 s).
- Android Back closes the HUD or the dimmed screen instead of leaving the app.
- Trying the demo after a drive no longer erases the recorded drive.
- If the BD500 hasn't been taught yet, a failed connect opens the Help section that explains how.
- Easier to read at arm's length: brighter grey text and slightly bigger labels on the Drive screen.
- Cars whose engine computer answers one value at a time never got speed, throttle or coolant (so no timer, launch or speed volume). Fixed.
- Changing the engine after *Stop sound* caused an error. Fixed.
- Lighter on the phone's battery: the screen only redraws what changed and nothing that is hidden under the HUD or the dimmed screen.

**What to send back after the drive**
1. Work through the checklist (Car tab).
2. Car tab → *Export (CSV)* to save the drive trace.
3. More tab → *Copy report for Claude*, then paste the report and attach the CSV in the chat.

## Earlier (rounds 5–21)

These were never tried in the car either:
- 8 new engines (26 in total) and per-engine tuning.
- Shift light, rev-match blips and launch control.
- 0–100 / 60–120 timer and drive log.
- HUD with mirror mode, and A/B compare.
- Inside (cabin) sound with booms.
- All engines re-voiced to their character targets.
