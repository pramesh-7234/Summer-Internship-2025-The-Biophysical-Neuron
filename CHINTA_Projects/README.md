# Dendritic Signal Propagation and Spatiotemporal Summation (CHINTA)

This folder documents a **May–July 2025 summer internship** using MOOSE to investigate how the location and sequence of inputs along a simplified dendrite affect the voltage measured at the soma. The supplied [project report](Project%20Report.pdf) explains the design and interpretation, while the [three notebooks](CHINTA_Projects/README.md) contain the simulation implementations. The [presentation images](CHINTA%20presentation/README.md) accompany the report.

## Problem

A dendrite receives electrical and synaptic signals at different positions and times. Even if the same stimulus is delivered, its effect at the soma may differ because of **electrotonic attenuation**, **temporal overlap**, **input location**, and the presence or absence of voltage-gated channels.

To isolate these effects, the project uses a **linear chain of ten cylindrical compartments**; the final compartment (`comp_9`) serves as the soma surrogate. Compartments are connected by their axial interfaces (`raxial` / `axial`). The same simplified geometry is used across three progressively more complex models.

## Experiments

| Notebook | Question | Model and method | Outputs examined |
| --- | --- | --- | --- |
| [P1.ipynb](CHINTA_Projects/P1.ipynb) | How are directly injected currents transmitted and summed in a passive dendrite? | No voltage-gated or synaptic channels; separate `PulseGen`s inject sequential pulses at each compartment, with input order reversed in a second run. | Timed current pulses and membrane voltage at the soma; attenuation and delayed responses. |
| [P2.ipynb](CHINTA_Projects/P2.ipynb) | How does event-triggered synaptic input differ from direct injection? | Passive membrane plus `SynChan`/`SimpleSynHandler`; `PulseGen` drives `SpikeGen` events that activate synapses; forward/reverse input sequences. | Presynaptic event times, somatic/compartment voltages, and evidence of overlapping postsynaptic potentials. |
| [P3.ipynb](CHINTA_Projects/P3.ipynb) | How do voltage-gated channels change propagation? | Same interconnected compartments with `HHChannel`s for sodium and potassium; tabulated HH gating rates; event-driven synaptic stimulation. | Compartment voltage traces and sodium, potassium, and synaptic conductance traces; action-potential timing across locations. |

### Techniques used

1. **Equivalent-circuit discretisation.** Each cylindrical compartment receives membrane capacitance (`Cm`), membrane resistance (`Rm`), and axial resistance (`Ra`); dimensions and specific passive parameters are used to calculate compartment properties.
2. **Prototype-and-copy construction.** MOOSE library objects are defined once, copied into the model, and coupled to neighbouring compartments.
3. **Controlled input sequencing.** Stimulation is delivered along the dendrite in one spatial order and then reversed to compare somatic responses. The notebooks specify pulse delays and simulation duration explicitly.
4. **Event-driven synaptic modelling.** In P2/P3, a `SpikeGen` triggers a synaptic event on a `SimpleSynHandler`, which activates a `SynChan` with specified reversal potential and temporal response.
5. **Active membrane dynamics.** P3 adds HH-type Na and K channels. Voltage-dependent gate kinetics are represented by tabulated rate functions and MOOSE integrates the resulting compartmental system.
6. **Measurement and comparison.** MOOSE `Table` objects record membrane potentials, stimulus signals, and/or channel conductances. Matplotlib plots compare input timing and voltage response across compartments.

## Interpretation

The [report](Project%20Report.pdf) discusses distance-dependent attenuation and timing in the passive experiments, spatiotemporal summation in the synaptic experiments, and sustained propagating spikes when voltage-gated channels are added. These are **observations reported for this simplified model**, not quantitative validation of a complete biological dendritic tree or a direct proof of Rall's cable-theory framework.

## Files

- [`CHINTA_Projects/`](CHINTA_Projects/README.md): `P1.ipynb`, `P2.ipynb`, and `P3.ipynb`; notebook-specific implementation notes.
- [`Project Report.pdf`](Project%20Report.pdf): the internship report, including the goals, model descriptions, diagrams, methods, results, discussion, and literature.
- [`CHINTA presentation/`](CHINTA%20presentation/README.md): twelve numbered PNG presentation pages.

## Running the notebooks

Use Jupyter with **MOOSE**, NumPy, Matplotlib, pandas, and seaborn available. P3 also imports Plotly. Run each notebook in a **fresh kernel**; objects are created at fixed MOOSE paths such as `/sim`, `/library`, and `/sim/model` and may collide when models are run in a shared kernel.

The notebooks were retained as originally authored. They have not been ported into a unified command-line program or exhaustively tested across MOOSE versions. See [notebook-level notes](CHINTA_Projects/README.md) before executing.
