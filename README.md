# betsy-phore

**Interactive architecture visualizer for the Betsy Architecture — a plano-hemispherical CVD diamond optical cavity as a spectrally multiplexed photonic reservoir computing substrate.**

> *Not related to the WS-BPEL Betsy test framework.*

> *Named after a dear friend who, like the concept itself, was full of light and not easily defined.*

-----

## What This Is

Betsy is a speculative volumetric photonic computing architecture based on a plano-hemispherical CVD diamond cavity with a honeycomb-patterned graphene modulator layer, deliberate dielectric scatterers for engineered mode diversity, and a gold flat base with high-reflectivity dielectric mirror coating. This repository contains an interactive browser-based visualizer illustrating the complete physical architecture — input laser, cavity physics, and skin readout — and the spectral multiplexing computation that constructs the stacked coupling matrix M_total from which effective rank r_eff is extracted.

**This is classical wave physics, not quantum computation.**

The architecture is a spectrally multiplexed photonic reservoir with engineered disorder. The benchmark task is nonlinear channel equalization. The P3 go/no-go condition is: r_eff(N_f=50; ε) > 50 from T2 simulation AND SER improvement >10% over a ring-resonator baseline.

-----

## The Physical Architecture

|Layer        |Material                                               |Function                                                 |
|-------------|-------------------------------------------------------|---------------------------------------------------------|
|Hemisphere   |CVD diamond, 25–30 mm diameter                         |Optical cavity body; n = 2.417                           |
|Inner surface|Graphene honeycomb (~400 cells, ~32% coverage)         |Electro-optic modulation via Al:HfO₂ ferroelectric gating|
|Interior     |Deliberate dielectric scatterers                       |Engineered disorder; Berry-Robnik mixed phase space      |
|Flat base    |Gold + TiO₂/SiO₂ dielectric mirror (R > 99.9%)         |Planar cavity end mirror; laser input port               |
|Outer surface|Electronic/photonic skin layer (~400 detector channels)|Conformal readout via evanescent leakage                 |

The cavity round-trip time is approximately 170 picoseconds. This is the cavity dynamics timescale, not system throughput.

### What Betsy Is Not

- **Not a whispering gallery mode (WGM) device.** Betsy exploits volumetric 3D mode structure throughout the full interior, not surface-bound evanescent modes at the rim.
- **Not quantum computing.** Classical wave computation, room temperature, Maxwell’s equations. No entanglement, no qubits.
- **Not a standard photonic reservoir.** 3D mode volume, ferroelectric material learning (Option C), and programmable disorder distinguish the architecture — whether these provide performance advantages over multimode fiber reservoirs is an open empirical question addressed by P3.

-----

## The Computation

The computational primitive is: **Resonate → Modulate → Collapse.**

1. **Resonate.** The cavity seeks stable mode configurations. Every supported mode is excited simultaneously from the first round trip.
1. **Modulate.** The graphene honeycomb imposes boundary conditions on which modes survive — applied continuously and simultaneously with resonance.
1. **Collapse.** The cavity evolves toward the lowest-loss mode configuration. The honeycomb defines an energy landscape; the light finds the minimum by existing within it.

### Spectral Multiplexing

Spatial rank alone is approximately 12 — insufficient for useful reservoir computation. Spectral multiplexing provides the dimensionality:

- **Sequential sweep:** N_f = 50 frequency steps across 50 GHz tuning range. Effective computation time ~5 μs.
- **Parallel comb injection:** Fiber frequency comb with tooth spacing ~Δf_corr injects all N_f channels simultaneously. Effective computation time ~100 ns — a ~50× improvement. Requires per-tooth demultiplexing validation.

The stacked coupling matrix M_total ∈ ℝ^((N_f × N_readout) × N_input) is constructed across the sweep. Effective rank r_eff(N_f; ε) is the count of singular values ≥ ε, where ε is pre-registered from the system noise floor before the simulation runs.

-----

## The Visualizer

A single-file interactive HTML/JS visualizer (`index.html`) with no build step and no dependencies. Illustrates the complete signal chain: laser injection → cavity physics → skin readout.

### What Is Shown

**Input — Laser source:**

- Stabilized Er-doped fiber laser, 1550 nm C-band, drawn below the gold base with fiber cable and cladding
- Input port cut into the gold mirror — the physical coupling point into the cavity
- In Comb Injection mode: label switches to FREQ COMB; multiple color-coded teeth beams (blue → red across 50 GHz) fan into the port simultaneously, per the Lesko group parallel injection architecture (FAU Erlangen-Nürnberg / LMU München)
- In Spectral Sweep mode: beam color tracks the current frequency step

**Cavity — Architecture layers:**

- CVD diamond hemisphere curved boundary with glow
- Graphene honeycomb (~400 hexagonal cells) activating on photon interaction, decaying between interactions
- Deliberate dielectric scatterers distributing photon trajectories toward Berry-Robnik mixed phase space
- Gold flat base with layered TiO₂/SiO₂ dielectric mirror coating

**Output — Electronic/photonic skin:**

- Conformal dashed detector arc on the outer hemisphere surface
- ~64 detector nodes lighting up in violet on evanescent leakage events, with signal lines back to the surface hit point
- Live skin readout canvas showing detector array signal by angular position across the arc with active channel count

**Computation — Sweep canvas:**

- Real-time construction of M_total columns as the spectral sweep advances
- r_eff estimate climbing as independent frequency steps accumulate
- Frequency axis labeled ω₁ → ω₅₀ (+50 GHz)

### Modes

|Mode            |What It Shows                                                                          |
|----------------|---------------------------------------------------------------------------------------|
|Free Propagation|Axial photons from laser port; graphene cells activate on diamond surface impact       |
|Scatterer Mixing|Off-axis injection; scatterers redirect trajectories toward Berry-Robnik disorder      |
|Spectral Sweep  |Steps through ω₁ → ω₅₀; each step adds a column to M_total; r_eff builds in real time  |
|Comb Injection  |All N_f channels simultaneously, color-coded by frequency; 50× throughput vs sequential|

Hover over any layer — laser, port, graphene cell, scatterer, cavity interior, skin layer — for architectural annotations drawn directly from the concept document.

### Running It

No build step. No dependencies. No internet connection required after initial font load.

```bash
git clone https://github.com/nhaaland/betsy-phore.git
cd betsy-phore
open index.html
```

Or visit the live version: **[nhaaland.github.io/betsy-phore](https://nhaaland.github.io/betsy-phore)**

-----

## Concept Document

The full architecture specification is documented in:

**Betsy Architecture v1.11** — *A Plano-Hemispherical Optical Cavity as a Volumetric Computing Substrate*
Nils Haaland | March 2026 | Concept Document

The document covers the formal physics framework (billiard classification, modal overlap parameter M, coupling matrix H_coupling), T2 simulation specification (2.5D axisymmetric MEEP), rank scaling hypothesis, null hypothesis, noise and drift analysis, fabrication readiness matrix, supply chain, and P3 cost/risk gate.

**Minimum Viable Falsification:**

- **KILL (physics):** r_eff(N_f=50; ε) ≤ 50 in T2 simulation → Betsy stops.
- **KILL (experiment):** P3 SER ≥ ring-resonator baseline → stop before H-track.
- **GO:** r_eff > 50 from T2 AND SER improvement >10% at P3 → proceed to H-track.

-----

## Fabrication Status

The materials are not speculative. The fabrication is.

|Step                                               |Status                                                |
|---------------------------------------------------|------------------------------------------------------|
|Optical grade CVD diamond hemisphere               |Specification gap only — Element Six capability exists|
|In-place graphitization on curved diamond (Route F)|Physically motivated; undemonstrated on curved surface|
|Honeycomb patterning on curved graphene            |No precedent                                          |
|Deliberate scatterer placement                     |Extrapolated from flat diamond biosensing precedent   |
|Optical quality rim bonding                        |**Go/no-go dependency.** No proposed solution.        |
|Gold base + dielectric mirror                      |Established on flat substrate                         |

The single highest-risk step is optical quality rim bonding of the curved diamond hemisphere to the flat gold mirror. It is a go/no-go dependency for the full Betsy track.

-----

## Validation Path

|Stage  |Description                                                                              |Status                                               |
|-------|-----------------------------------------------------------------------------------------|-----------------------------------------------------|
|P0     |Microwave scale model (250 mm cavity, polished aluminum/acrylic)                         |Not built                                            |
|T2     |2.5D axisymmetric MEEP simulation; pre-register ε; report r_eff, Δf_corr, memory capacity|Not run                                              |
|P1     |Flat graphene — commercial supply                                                        |Available today                                      |
|P2     |Flat ferroelectric + scatterers                                                          |Requires P1                                          |
|P3     |Planar reservoir benchmark vs. ring-resonator baseline                                   |Requires T2 + P2                                     |
|H-track|Full hemisphere                                                                          |Requires P3 + rim bonding + Element Six optical grade|

P0 costs a few hundred dollars and validates the core Sinai billiard mixing physics before any diamond or graphene investment. It is the cheapest possible physics check and should precede everything else.

-----

## Collaboration Targets

- **Element Six (De Beers, UK)** — optical grade CVD diamond hemisphere specification
- **Fraunhofer IAF (Germany)** — Route F graphene characterization on SCD(111); physics validation
- **Ghent University / Bogaerts group at imec (Belgium)** — P3 reservoir computing benchmark partnership
- **Daniel Lesko group, FAU Erlangen-Nürnberg / LMU München** — fiber frequency comb for parallel injection architecture

-----

## Strategic Context

Betsy is kept deliberately separate from the primary photonic research program (diamond-integrated 3D silicon photonics, non-volatile photonic memory, ferroelectric thin film pre-qualification, MISA programming framework). The material stack is shared. The fabrication paradigm, the computational architecture, and the speculative distance from demonstrated reality are not.

Betsy is the experimental wedge into the CoNexus framework — a single-device, falsifiable instantiation of one principle: that a plano-hemispherical optical cavity with engineered disorder can function as a high-dimensional photonic reservoir. Betsy can be published, validated, or killed without requiring the CoNexus thesis to be defended. That separation is deliberate and valuable.

-----

## Author

Nils Haaland | nhaaland@yahoo.com | March 2026
Solbakken Research Initiative — independent research

Review provenance: wave-chaos framework developed through dialogue with Claude (Anthropic) and GPT-4, with Perplexity providing independent review of v1.7, and DeepSeek and Gemini contributing to v1.10–1.11. Not reviewed by a wave-chaos physicist, photonic reservoir specialist, or CVD diamond fabrication expert. External domain expert review is the next validation step.

-----

## License

Code (visualizer): [MIT License](LICENSE)
Concept document: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — attribution required, derivatives permitted.

-----

## Topics

`photonic-reservoir-computing` `cvd-diamond` `optical-cavity` `photonics` `reservoir-computing` `spectral-multiplexing` `berry-robnik` `graphene` `volumetric-computing` `visualization`

-----

*The physics does not obviously say no. The T2 simulation will say something definitive.*
