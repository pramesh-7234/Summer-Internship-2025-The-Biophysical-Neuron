# Hodgkin–Huxley Action Potential Visualisation

## Question

How do an injected stimulus and voltage-dependent ion-channel gates shape the membrane potential and conductances of a Hodgkin–Huxley-type membrane?

This folder contains a standalone Python program that builds a **single-compartment membrane model** in MOOSE and provides an **interactive PyQt5 GUI** for exploring the simulated voltage and channel behaviour.

## Model and methods

- A MOOSE `Compartment` is created with membrane capacitance, resistance, and initial voltage settings.
- The potassium channel (`HHChannel`) uses an **n-type activation gate**, with a fourth-power activation dependence (`n^4`).
- The sodium channel uses **m-type activation** and **h-type inactivation** gates (`m^3 h`).
- Voltage-dependent gate rate coefficients are supplied through `HHGate.setupAlpha(...)` tables.
- A `PulseGen` delivers a user-controlled stimulus. After every graph-button selection, the program reinitialises MOOSE, simulates the response, and reads `Table` recordings.
- PyQt5 provides an injected-current slider and six graph buttons; Matplotlib draws the selected trace inside the GUI.

The program visualises **membrane voltage, sodium conductance, potassium conductance, and the n/m/h gate states**. These time traces illustrate the mechanistic link between depolarisation, channel activation/inactivation, and the resulting electrical response.

## Files

- [`HH AP visualization.py`](HH%20AP%20visualization.py): original implementation and GUI.
- [`Membrane Potential.png`](Membrane%20Potential.png): saved voltage plot.
- [`Sodium Conductance.png`](Sodium%20Conductance.png) and [`Potassium Conductance.png`](Potassium%20Conductance.png): saved ionic conductance plots.
- [`n gate.png`](n%20gate.png), [`m gate.png`](m%20gate.png), [`h gate.png`](h%20gate.png): saved gating-state plots.

![Example membrane potential trace](Membrane%20Potential.png)

## Running

The script depends on **MOOSE**, **NumPy**, **Matplotlib**, and **PyQt5**. It also imports pandas, seaborn, Plotly, and SciPy, so these packages must be available unless the import list is independently streamlined. Use a graphical desktop session with a functional Qt backend.

From this directory:

```bash
python 'HH AP visualization.py'
```

Move the slider to change the injected-current value, then select one of the six plotting buttons to run the simulation and display the corresponding trace.

## Limitations

This is an **educational single-compartment HH visualiser**, not an experimentally calibrated axon model or a validated biophysical parameter-estimation tool. The script uses its original numerical conventions for time, voltage, and conductance and does not explicitly perform standard unit conversions at every plotting step; labels should not be used to infer independent calibration. There is no automatic comparison with experimental voltage recordings or parameter-sensitivity analysis.

**Documentation-only update:** the script and all six saved plots have been retained unchanged.
