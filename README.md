# Quantum Zeno Effect: Freezing Quantum Evolution via Continuous Measurement

A quantum simulation project built with **Qiskit 1.x** demonstrating the **Quantum Zeno Effect**—a phenomenon where an evolving quantum system is frozen in its initial state through frequent, repetitive measurement.

---

## Overview

In classical mechanics, observing a system does not alter its trajectory ("a watched pot never boils" is purely metaphorical). In quantum mechanics, however, observation induces **wavefunction collapse**. 

This repository models a single qubit initially in state $|0\rangle$ subjected to a continuous driving field (represented by an $R_y(\pi)$ rotation gate) that would normally flip it completely to $|1\rangle$. By interrupting this evolution with $N$ intermediate measurement gates, we observe how increasing the measurement frequency suppresses the transition probability and locks the qubit in $|0\rangle$.

---

## Mathematical Formulation

To rotate a qubit from $|0\rangle$ to $|1\rangle$, a total rotation of $\theta_{\text{total}} = \pi$ radians is required. Dividing this evolution into $N$ equal micro-steps yields an incremental rotation angle per step of:

$$\delta\theta = \frac{\pi}{N}$$

After a single micro-step $R_y(\delta\theta)$, the quantum state becomes:

$$|\psi_1\rangle = \cos\left(\frac{\pi}{2N}\right)|0\rangle + \sin\left(\frac{\pi}{2N}\right)|1\rangle$$

The probability of measuring the qubit in state $|0\rangle$ after one step is:

$$P_{\text{survival}}^{(1)} = \cos^2\left(\frac{\pi}{2N}\right)$$

For $N$ consecutive micro-rotation and measurement steps, the total survival probability is given by:

$$P(|0\rangle) = \left[\cos\left(\frac{\pi}{2N}\right)\right]^{2N}$$

Using Taylor expansion for small angles ($N \gg 1$):

$$\cos^2\left(\frac{\pi}{2N}\right) \approx 1 - \frac{\pi^2}{4N^2}$$

$$P(|0\rangle) \approx \left(1 - \frac{\pi^2}{4N^2}\right)^N$$

Taking the limit as measurement frequency approaches infinity:

$$\lim_{N \to \infty} P(|0\rangle) = 1$$

---

## Project Structure

* **Circuit Construction:** Iteratively builds parameterized single-qubit circuits containing alternating $R_y(\delta\theta)$ operations and standard computational $Z$-basis measurements (`qc.measure(0, 0)`).
* **Simulation Engine:** Executes circuits using Qiskit's `AerSimulator` and the modern `SamplerV2` primitive across $3,000$ shots per value of $N$.
* **Data Evaluation:** Compares empirical simulation results against theoretical trigonometric limits.
* **Visualization:** Generates high-resolution publication-ready plots using Matplotlib.

---

## Key Results

| Measurement Steps ($N$) | Rotation per Step ($\delta\theta$) | Empirical $P(|0\rangle)$ | Theoretical $P(|0\rangle)$ | State Evolution Status |
| :--- | :--- | :--- | :--- | :--- |
| **1** | $180.0^\circ$ ($\pi$) | `0.0000` | `0.0000` | Full transition to $\|1\rangle$ |
| **2** | $90.0^\circ$ ($\pi/2$) | `0.2500` | `0.2500` | Partial transition |
| **5** | $36.0^\circ$ ($\pi/5$) | `0.6054` | `0.6050` | Majority preserved |
| **20** | $9.0^\circ$ ($\pi/20$) | `0.8838` | `0.8841` | Strongly frozen |
| **100** | $1.8^\circ$ ($\pi/100$) | `0.9752` | `0.9755` | Near-complete Zeno freezing |

---

## Significance & Applications

1. **Quantum State Preservation:** Demonstrates foundational concepts behind preventing unwanted state drift in physical hardware.
2. **Quantum Error Mitigation:** Serves as an intuitive precursor to **syndrome measurements** and **ancilla qubit monitoring** in modern fault-tolerant architecture.
3. **Fundamental Quantum Mechanics:** Empirical validation of measurement-induced wavefunction collapse and non-unitary evolution.

---

## Author

* **Farhan** — *Qiskit Fall Fest Submission*
