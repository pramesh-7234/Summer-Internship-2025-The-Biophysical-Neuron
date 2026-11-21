# CHINTA Simulation Notebooks: P1 → P2 → P3

These notebooks implement three successive electrical models of a **ten-compartment cylindrical dendrite**, with the last compartment interpreted as the soma. They are best read in sequence. See the [project overview](../README.md) and [internship report](../Project%20Report.pdf) for biological context and interpretation.

## P1 — Passive cable and direct current injection

**Problem:** How do the position and order of current injections affect the somatic voltage in a dendrite without active ion channels?

**Method:** A reusable MOOSE `Compartment` prototype with `Rm`, `Cm`, `Ra`, geometry, and resting potential is copied ten times and connected axially. Each compartment has a `PulseGen` that supplies a brief current pulse; the pulse delays are varied to stimulate from one end of the dendrite to the other and then in reverse. A `Table` samples voltage at the somatic compartment.

**Analysis:** Plots juxtapose injection times with the somatic response to illustrate passive spread, attenuation, and overlap of signals. This is a discrete compartmental **cable-like model**, not a direct PDE solver.

[Open P1.ipynb](P1.ipynb)

## P2 — Passive dendrite with event-triggered synaptic inputs

**Problem:** What changes when input is delivered through a synaptic conductance rather than directly as injected current?

**Method:** The ten passive compartments are supplemented with `SynChan` and `SimpleSynHandler` objects. Each input `PulseGen` drives a `SpikeGen`; its `spikeOut` event activates the corresponding synapse. The model records voltage and input timing while reversing the sequence of activation.

**Analysis:** The notebook compares postsynaptic voltage responses at the soma and along the cable, including the consequences of closely timed inputs. The synapse is an idealised event-driven conductance, not a biochemical model of transmitter release.

[Open P2.ipynb](P2.ipynb)

## P3 — Active HH dendrite with synaptic input

**Problem:** Can voltage-gated Na and K channels sustain electrical excitation that would attenuate in a passive cable?

**Method:** To the same ten-compartment setup, the notebook adds `HHChannel`s for Na and K with `HHGate` variables (`m`, `h`, and `n`) defined from voltage-dependent rate expressions. Stimulus events travel from `PulseGen` through `SpikeGen` to synaptic conductances, while recording tables sample voltage, injected input, and Na/K/synaptic conductance.

**Analysis:** Multiple plots compare action-potential timing and voltage amplitude between compartments and between stimulation sequences. The associated report interprets the results as active propagation and dendritic signal integration.

[Open P3.ipynb](P3.ipynb)

## Running and limitations

Install/configure [MOOSE](https://mooseneuro.github.io/) in a Python/Jupyter environment with `numpy`, `matplotlib`, `pandas`, and `seaborn`; P3 also imports `plotly.express`. Open notebooks individually and run cells in order in **separate, fresh kernels** because the simulations create MOOSE objects at shared absolute paths.

The three notebooks are **archived experiments**, not a reusable API. Some cell comments describe intended timing rather than the exact current setting: refer to the executed `PulseGen` values in each notebook for the actual input schedule. No source code or notebook cells have been altered as part of the documentation update.
