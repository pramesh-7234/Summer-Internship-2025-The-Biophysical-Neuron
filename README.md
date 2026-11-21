# Biophysical Neuron Modeling with MOOSE

This repository collects computational neuroscience exercises and internship work on **how electrical signals are generated, transmitted, and integrated in neuronal compartments and dendrites**. The models range from a single Hodgkin–Huxley (HH) membrane and small connected-compartment systems to a ten-compartment dendrite and a morphology-based CA1 pyramidal-cell exploration.

The primary simulation environment is [MOOSE (Multiscale Object-Oriented Simulation Environment)](https://mooseneuro.github.io/). Most work is recorded in Jupyter notebooks; one project provides a PyQt5-based interactive visualisation. The emphasis is on constructing biophysical objects, connecting them, controlling stimulation, and analysing membrane-potential and channel-conductance traces.

## Projects and questions

| Location | Scientific/computational question | Principal methods |
| --- | --- | --- |
| [CHINTA_Projects/](CHINTA_Projects/README.md) | How does the location and order of dendritic inputs affect their attenuation, summation, and propagation towards the soma? | Ten interconnected cylindrical compartments; passive current injection; event-triggered synaptic conductances; HH sodium/potassium channels; time-series comparisons. |
| [CA1_Pyramidal/](CA1_Pyramidal/README.md) | How can realistic dendritic morphology and distance-dependent channel models be prepared to study the effect of A-type potassium channels on backpropagation? | Morphology conversion and reduction; MOOSE compartment loading; recursive soma-distance traversal; NetworkX/Plotly graph visualisation; voltage-dependent channel prototypes. |
| [HH Action Potential Visualization/](HH%20Action%20Potential%20Visualization/README.md) | How does injected current affect action potentials, membrane conductances, and HH gating variables? | MOOSE `HHChannel`/`HHGate`; voltage-dependent rate tables; `PulseGen`; recording `Table`s; interactive PyQt5/Matplotlib GUI. |
| [4 comp (chan and synchan).ipynb](4%20comp%20%28chan%20and%20synchan%29.ipynb) | Can a spike initiated in one active compartment trigger events in a chain of four synaptically coupled compartments? | Reusable HH-compartment prototype, `SpikeGen` → `SimpleSynHandler` → `SynChan` connections, current-pulse stimulation, voltage and synaptic-conductance recording. |
| [Y-shaped-branch.ipynb](Y-shaped-branch.ipynb) | How do signals travel through and interact at a simplified dendritic branch point? | Three ten-compartment HH branches joined in a Y topology; paired current injection at the outer ends; MOOSE voltage tables; time/position plots and an experimental PyQt5 viewer. |

## Main modelling ideas

- **Compartmental approximation:** spatially extended neuronal membranes are represented by coupled electrical compartments. Their capacitance, membrane conductance/resistance, and axial resistance shape the spread of voltage.
- **Active membrane dynamics:** voltage-gated sodium and potassium conductances are represented through Hodgkin–Huxley-style gating variables and voltage-dependent transition-rate tables.
- **Synaptic transmission:** a threshold-crossing `SpikeGen` sends events to a `SimpleSynHandler`, which activates a `SynChan` on a target compartment.
- **Numerical experiments:** injected-current timing, synapse activation, membrane voltage, and ionic/synaptic conductances are recorded and compared across compartments.
- **Morphology analysis:** tree connectivity, soma-to-compartment distance, and visualisation provide a basis for studying non-uniform channel distributions.

## Environment and dependencies

**MOOSE is essential** for the model simulations. Installation depends on the available Python/platform combination; follow the [official installation and documentation](https://mooseneuro.github.io/) and check that `import moose` works in the notebook kernel you select.

Other libraries are used in particular files rather than uniformly across the repository:

| Dependency | Used for |
| --- | --- |
| `numpy` | Array operations, sampled rate functions, time axes, numerical analysis. |
| `matplotlib` | Voltage, conductance, and morphology plots; embedded GUI figures. |
| `pandas`, `seaborn` | Tabular data and plotting in some dendritic notebooks. |
| `plotly`, `networkx` | Interactive morphology and connectivity graphs. |
| `PyQt5` | The HH visualiser and Y-branch notebook's experimental GUI. |
| `vedo` | The standalone three-dimensional SWC morphology viewer. |
| `scipy` | Imported by the HH visualisation script. |
| `jupyter` | Running and inspecting `.ipynb` notebooks. |

For example, after creating a suitable environment with a working MOOSE installation, install the applicable plotting/notebook dependencies with `python -m pip install numpy matplotlib pandas seaborn plotly networkx PyQt5 vedo scipy jupyter` as needed. This is a **dependency overview**, not a tested lockfile; compatible MOOSE and GUI builds may require a dedicated environment.

## How to explore

1. Start with [CHINTA_Projects/README.md](CHINTA_Projects/README.md) for the clearest sequence of related numerical experiments (passive current injection → passive synaptic inputs → active HH propagation). The accompanying PDF report describes the motivation, model setup, and interpretation.
2. Open the `.ipynb` files using Jupyter and run cells in their intended order in a MOOSE-enabled kernel. MOOSE simulations create named global model objects, so a **fresh kernel** is advisable between independent model runs.
3. For the interactive single-compartment model, run from the corresponding directory:

   ```bash
   cd 'HH Action Potential Visualization'
   python 'HH AP visualization.py'
   ```

4. For the morphology study, consult the [CA1-specific README](CA1_Pyramidal/README.md) before running anything: the archived notebook and plotting script contain **machine-specific absolute paths**.

## Scope and limitations

This is a **research/learning code collection**, not a validated neuronal simulator package. The archived notebooks reflect exploratory development and may depend on the author's original MOOSE setup, kernel state, or file paths. In particular, the CA1 notebook prepares morphology and channel definitions but does **not** establish a completed test of A-type potassium-channel suppression of backpropagation. The four-compartment and Y-branch notebooks also contain exploratory observations and plotting code, not formal parameter sweeps or validation studies.

The project-specific READMEs distinguish **implemented methods**, **reported observations**, and **work still to be verified**. Source code, notebooks, morphology assets, report, and existing figures are intentionally preserved.

## References and learning resources

- [MOOSE documentation](https://mooseneuro.github.io/)
- [MOOSE examples](https://github.com/subhacom/moose-examples) and [MOOSE notebooks](https://github.com/subhacom/moose-notebooks)
- [CHINTA summer-project report](CHINTA_Projects/Project%20Report.pdf)
- A-type potassium-channel motivation: Hoffman, Magee, Colbert & Johnston (1997), *K+ channel regulation of signal propagation in dendrites of hippocampal pyramidal neurons* (see the CA1 README).
