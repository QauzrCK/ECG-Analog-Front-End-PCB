# ECG-Analog-Front-End-PCB

# Low-Noise ECG Biopotential Analog Front-End (AFE)

A single-supply ($3.3\text{ V}$) analog front-end designed to extract, filter, and amplify $1\text{ mV}_{\text{pp}}$ electrocardiography (ECG) signals for microcontroller ADC acquisition, verified in **LTspice** and laid out as a 2-layer PCB in **KiCad**.

## System Specifications & Simulated Performance
* **Supply Voltage:** $3.3\text{ V}$ single-supply ($V_{\text{ref}} = 1.65\text{ V}$ mid-rail bias)
* **Mid-Band Gain:** $530.9\text{ V/V}$ ($54.5\text{ dB}$)
* **Passband Bandwidth:** $3.39\text{ Hz} – 148.7\text{ Hz}$
* **Mains Interference Rejection:** $>35\text{ dB}$ attenuation at $50\text{ Hz}$
* **Input-Referred Noise:** $<10\ \mu\text{V}_{\text{rms}}$ ($0.5–150\text{ Hz}$)

## Signal Chain Architecture
1. **Instrumentation Amplifier (`AD8226`):** Differential gain set to $G_1 = 1 + \frac{49.4\text{ k}\Omega}{5.6\text{ k}\Omega} = 9.82\text{ V/V}$, level-shifted to $V_{\text{ref}} = 1.65\text{ V}$ to prevent clipping from $\pm 150\text{ mV}$ electrode DC offsets.
2. **1st-Order High-Pass Filter:** $C_1 = 10\ \mu\text{F}$ and $R_2 = 4.7\text{ k}\Omega$ ($f_c = 3.39\text{ Hz}$) referenced to $V_{\text{ref}}$ to eliminate respiratory baseline wander while maintaining low source impedance into the notch stage.
3. **Active Twin-T $50\text{ Hz}$ Notch Filter (`LT1490`):** Symmetrical R-R-C / C-C-R bridge ($R = 31.6\text{ k}\Omega$, $C = 100\text{ nF}$, $f_0 = 50.37\text{ Hz}$) with unity-gain output bootstrapping to narrow the notch bandwidth and preserve QRS complex sharpness.
4. **2nd-Order Butterworth Sallen-Key Low-Pass & Final Gain (`LT1490`):** Equal-component low-pass stage ($R = 10.7\text{ k}\Omega$, $C = 100\text{ nF}$, $f_c = 148.7\text{ Hz}$, $A_v = 1.59\text{ V/V}$, $Q = 0.709$) cascaded into a non-inverting gain stage ($R_{10} = 33\text{ k}\Omega$, $R_{11} = 1\text{ k}\Omega$, $G_4 = 34\text{ V/V}$).

## PCB Layout Highlights (KiCad)
* **2-Layer Stackup:** Signal and power routing on top layer (`F.Cu`) with a continuous solid `GND` copper pour on the bottom layer (`B.Cu`) stitched via ground vias.
* **Component Selection:** 0805 SMD passives and SOIC-8 IC packages for hand-assembly and probing.
