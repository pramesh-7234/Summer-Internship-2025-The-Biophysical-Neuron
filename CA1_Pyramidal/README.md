# CA1 Pyramidal Neuron: Morphology and A-Type Potassium Channel Exploration

## Research question

How can the anatomy and spatially varying membrane conductances of a hippocampal **CA1 pyramidal neuron** be represented computationally, as a step toward studying the role of dendritic **A-type potassium (K\_A) channels** in controlling action-potential backpropagation?

The scientific motivation comes from the study by **Hoffman, Magee, Colbert & Johnston (1997)**, *K+ channel regulation of signal propagation in dendrites of hippocampal pyramidal neurons*. This folder is an **exploratory model-building and morphology-analysis project**. It does **not** currently provide a complete numerical reproduction or validation of the paper's electrophysiological findings.

## Approach and methods actually present

1. **Morphology preparation.** A neuronal reconstruction (`052714C_37.asc`) was obtained from [NeuroMorpho.Org](https://neuromorpho.org/). The repository includes an SWC representation (`052714C_37.swc`) and a reduced GENESIS/MOOSE-compatible morphology (`052714C_37_condensed.p`). The original README records conversion via NeuroMorpho's `xyz2swc` tool and compartment reduction with [ShapeShifter](https://github.com/neurord/ShapeShifter). Those conversions are **not reimplemented** in this folder.
2. **Compartmental loading.** [`CA1.ipynb`](CA1.ipynb) calls `moose.loadModel` on the condensed `.p` morphology and accesses the resulting compartment objects. It also changes the somatic membrane capacitance in the loaded cell.
3. **Distance-from-soma analysis.** A recursive graph traversal follows axial compartment connections from a named soma (`1_1`) and accumulates path lengths. This spatial information is used to colour a connectivity network.
4. **Graph-based visualisation.** NetworkX builds a graph of the axial connections, Matplotlib renders a coloured network, and Plotly exports an interactive view to [`morphology_graph.html`](morphology_graph.html). The graph uses a *spring layout*, so drawn distance is **not** physical morphology distance; the colour encodes the calculated path length.
5. **Voltage-gated channel prototypes.** Later notebook cells construct prototypes for voltage-dependent **Na**, **delayed-rectifier K (KDR)**, and **A-type K (KA)** channels. Gate tables approximate activation/inactivation functions; one branch of the KA kinetics depends on distance from the soma. An event-driven synaptic channel prototype is also defined.
6. **Standalone geometry inspection.** [`neuron_plotter.py`](neuron_plotter.py) reads the SWC reconstruction and draws three-dimensional segments by morphology class using `vedo`.

## Files

| File | Role |
| --- | --- |
| [`CA1.ipynb`](CA1.ipynb) | MOOSE model loading, path-length analysis, graph plots, and active-channel prototype definitions. |
| [`neuron_plotter.py`](neuron_plotter.py) | Standalone SWC parsing and interactive 3D morphology visualisation using Vedo. |
| [`052714C_37.asc`](052714C_37.asc) | Original morphology reconstruction format. |
| [`052714C_37.swc`](052714C_37.swc) | SWC-format morphology coordinates and parent relationships. |
| [`052714C_37_condensed.p`](052714C_37_condensed.p) | Reduced morphology format loaded by MOOSE. |
| [`morphology_graph.html`](morphology_graph.html) | Previously exported interactive connectivity plot, viewable in a web browser. |

## Setup and execution notes

The notebook requires a working [MOOSE](https://mooseneuro.github.io/) installation plus `numpy`, `matplotlib`, `networkx`, `seaborn`, and `plotly`; the standalone viewer additionally needs `vedo`.

**Paths require attention:** the archived notebook's `moose.loadModel(...)` cell uses an absolute path under `/home/pramesh/ME/MOOSE_STUFF/...` rather than the relative file included here. Similarly, `neuron_plotter.py` hardcodes a different absolute path under `/home/pramesh/MOOSE_STUFF/...`. These were **left unchanged**; to execute them on another machine, the user must supply an appropriate local path, without modifying the morphology files themselves.

The generated HTML can be opened directly without running MOOSE. A browser may need an internet connection for external Plotly assets if the HTML references them.

## Current scope and limitations

The notebook constructs **candidate channel objects and gating tables** but does not show a completed set of stimulation experiments comparing dendritic backpropagation with and without A-type K conductances. Therefore, a physiological conclusion about suppression of backpropagating action potentials **cannot be inferred from these files alone**. The channel formulas, parameter scales, compartment connections, and treatment of voltage-dependent singularities also merit separate verification before quantitative biophysical claims are made.

This documentation describes the source as supplied; no Python code, notebooks, or morphology data have been altered.

## References

- Hoffman, D. A., Magee, J. C., Colbert, C. M., & Johnston, D. (1997). *K+ channel regulation of signal propagation in dendrites of hippocampal pyramidal neurons*. Nature **387**, 869–875.
- [NeuroMorpho.Org](https://neuromorpho.org/), [ShapeShifter](https://github.com/neurord/ShapeShifter), and [MOOSE](https://mooseneuro.github.io/).
