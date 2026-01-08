# Closed-Loop Harmonic Energy Schematic
THRL: A phase-locked resonant energy loop that maintains a high-Q oscillator with quadrature drive, harvests in-phase power as adjustable damping via synchronous extraction, recycles reactive energy locally, and buffers output through a storage bus under a hard safety envelope.
Closed-Loop Harmonic Energy Schematic

Name: Toroidal Harmonic Regenerative Loop (THRL)
Purpose: A closed-loop control schematic that does not claim free energy, but captures, shapes, stores, and reuses oscillatory energy (electrical + mechanical) by keeping a system at safe, high-Q resonance while harvesting only the controlled surplus.
Status: Conceptual architecture for open experimentation, education, and defensible disclosure.

1) Core idea in one sentence

Build an energy system that behaves like a musical instrument: you maintain resonance with minimal drive, extract energy only in-phase, and recycle reactive energy instead of dumping it as heat.

2) Closed-loop harmonic schematic (block + signal flow)
                 ┌─────────────────────────────────────────────────┐
                 │                 SAFETY ENVELOPE                 │
                 │  (limits, interlocks, temp, voltage, speed, EMI)│
                 └───────────────┬─────────────────────────────────┘
                                 │ enables/derates
                                 v
┌───────────┐   sense    ┌──────────────────┐   control   ┌──────────────────┐
│ Resonator │───────────▶│ Phase/Freq Lock  │────────────▶│ Drive Inverter   │
│ (electro- │  V,I,φ,a   │ (PLL + estimator)│  f*, φ*, A* │ (bidirectional)  │
│ mech)     │◀───────────│ (Q, modal state) │◀────────────│ (current-mode)   │
└─────┬─────┘  inject     └────────┬─────────┘  feedback   └───────┬──────────┘
      │                            │                                │
      │ harvest (in-phase)         │ setpoints                      │ injects
      v                            v                                v
┌──────────────────┐       ┌──────────────────┐             ┌──────────────────┐
│ Synchronous       │──────▶│ Storage Bus      │◀───────────▶│ Reactive Energy  │
│ Energy Extractor  │  Pdc  │ (supercap/batt) │  Qrecirc    │ Recycler (L-C)   │
│ (active rectifier │       │ + DC link)      │             │ (soft-switching) │
│ + load shaping)   │       └───────┬─────────┘             └──────────────────┘
└─────────┬────────┘               │
          │                         │ deliver
          v                         v
     ┌───────────┐           ┌───────────────┐
     │ Useful    │           │ External Load  │
     │ Output    │           │ / Grid / Micro │
     └───────────┘           └───────────────┘

What makes it “harmonic” and “closed-loop”

Harmonic: everything revolves around phase, frequency, Q, and modal energy—not just volts and watts.

Closed-loop: sensing → phase-lock → drive + extraction shaping → storage → drive again, with stability and safety limits.

3) The physical “resonator” options (choose one)
A) Electrical toroid resonator (cleanest to prototype)

Toroidal inductor + capacitor bank (L–C), optionally a coupled secondary.

Goal: high-Q oscillation at kHz–MHz range.

B) Electromechanical resonator (more intuitive)

Flywheel/rotor on magnetic bearing + motor/generator, operated as a mechanical oscillator (torsional resonance).

Goal: store energy as rotational momentum; harvest via controlled generator torque.

C) Acoustic / piezo lattice (low power, sensor-rich)

Piezo stack + acoustic cavity; resonance maintained, energy drawn into DC bus.

The architecture is the same: you treat the resonator like an “instrument” whose pitch (f) and phase (φ) must be held.

4) Key control laws (minimal math, maximum usefulness)
4.1 Phase-locked drive (keep it singing)

Maintain the drive so current (or force) is in quadrature with the resonator state to sustain oscillation efficiently, while the extractor takes the in-phase component.

Estimate resonator phase:

φ = angle(analytic_signal(V) or analytic_signal(position))

Set drive phase relative to resonator:

φ_drive = φ + 90° (sustain)

Amplitude control to hold target energy:

Ê = ½ C V² (electrical) or ½ J ω² (mechanical)

A* = clamp(A* + k(E_target − Ê), limits)

4.2 Synchronous extraction (take only what doesn’t collapse resonance)

Extraction is shaped to appear as damping that you can dial up/down:

Extractor current reference:

i_extract = k_damp · V_res (in-phase with V)

k_damp is adjusted so Q stays in a safe band:

k_damp = f(Q_target − Q̂)

4.3 Reactive energy recycling (stop wasting VARs)

Use bidirectional power stages and soft switching so reactive power circulates locally rather than heating devices.

5) Concrete “buildable” reference design (electrical prototype)
Components

L (toroidal inductor): low-loss core or air-core, sized for target f.

C (resonant capacitor bank): film caps, low ESR.

Bidirectional inverter: MOSFET/SiC half-bridge, current-mode control.

Active rectifier/extractor: synchronous full-bridge to DC bus.

Storage bus: supercap (fast) + battery (energy).

Sensors: V_res, I_res, DC link V, device temp, EMI pickup.

Operating cycle

Start-up chirp: sweep frequency to find peak response (resonance).

Lock: PLL locks onto resonant frequency; drive aligns phase.

Harvest: extractor increases damping slowly until bus target is met.

Regulate: bus controller allocates energy to load/storage.

Protect: if temperature, voltage, or instability rises → derate or open loop.

6) Why this is “closed-loop regenerative” (without magical claims)

In ordinary systems, reactive energy (sloshing between L and C) often causes losses because the electronics fight it.

Here, reactive energy is treated as a resource:

The drive maintains oscillation with minimal injection,

The extractor harvests as controlled damping,

The recycler reduces switching losses,

The bus stores and redeploys energy.

This is resonant energy management, not creation.

7) Stability & safety envelope (non-optional)

Failure modes in resonant systems are real: runaway voltage/current, thermal drift, parasitic oscillations, EMI.

Minimum safeguards:

Hard clamps: V_res max, I_res max, DC link max.

Thermal derating: MOSFET/inductor/cap temperature.

Mode detection: if multiple resonant modes appear, reduce gain or re-scan.

EMI containment: shielding + filtered measurement chain.

Safe shutdown: open drive, short or dump resonator through controlled path if needed.

8) “Harmonic schematic” in a single sentence (to publish)

THRL: A phase-locked resonant energy loop that maintains a high-Q oscillator with quadrature drive, harvests in-phase power as adjustable damping via synchronous extraction, recycles reactive energy locally, and buffers output through a storage bus under a hard safety envelope.

9) Optional upgrade: braided multi-resonator array (the “chorus”)

Instead of one resonator, use N coupled resonators (slightly detuned). Benefits:

wider operating bandwidth

reduced peak stress per cell

distributed thermal load

controllable modal patterns (“harmonic beamforming” for energy flow inside the array)

Control extension:

modal estimator → drive allocation vector u across cells

extractor damping applied per mode, not per cell
