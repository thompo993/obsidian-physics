---
tags:
  - note
  - super-musr
created: 2026-06-25
---

## Part 1: The Muon and Its Properties

### What is a Muon?

A muon is a fundamental particle similar to an electron but with a mass about 207 times larger. When high-energy cosmic rays strike the Earth's atmosphere, they create pions that decay into muons. These muons can penetrate deep into matter before decaying.

**Key properties relevant to μSR:**

- **Spin:** I = 1/2 (like an electron or proton)
- **Magnetic moment:** μμ ≈ 3.18 × 10⁻³ μB (where μB is the Bohr magneton)
- **Lifetime:** τμ ≈ 2.2 μs (in its rest frame)
- **Decay:** μ⁻ → e⁻ + νe + ν̄μ (the decay is spin-dependent)

The decay process is crucial: the neutrinos preferentially carry away angular momentum in a direction opposite to the muon's spin. This means the emitted positron is more likely to be emitted in the direction of the muon's spin—this is the **spin-asymmetry** that makes μSR possible.

### Why Does the Muon's Spin Polarization Matter?

When a polarized muon beam enters a sample, the muons' spins precess due to magnetic fields in the material. The **time-integrated muon spin polarization** tells us about the magnetic environment: if muon spins precess at different rates (due to spin relaxation or dephasing), the asymmetry in positron emission decreases over time. By measuring this decay, we learn about magnetic interactions.

---

## Part 2: Magnetic Resonance Basics

### Energy Levels of a Spin in a Magnetic Field

Consider a spin-1/2 particle (like a muon) in an external magnetic field **B₀** (conventionally aligned along the z-direction).

The spin Hamiltonian is: $$H = -\boldsymbol{\mu} \cdot \mathbf{B} = -\gamma \hbar \mathbf{I} \cdot \mathbf{B}$$

where:

- γ is the gyromagnetic ratio (γμ ≈ 1.35 × 10⁸ rad/(s·T) for muons)
- **I** is the spin operator
- ℏ is the reduced Planck constant

For a field **B₀** = B₀ **ẑ**, the z-component of the spin Hamiltonian gives: $$H_z = -\gamma \hbar B_0 I_z$$

The eigenstates are the spin "up" (|↑⟩) and "down" (|↓⟩) states along the z-axis: $$I_z |\uparrow\rangle = \frac{\hbar}{2}|\uparrow\rangle$$ $$I_z |\downarrow\rangle = -\frac{\hbar}{2}|\downarrow\rangle$$

The corresponding energies are: $$E_{\uparrow} = -\gamma \hbar B_0 \cdot \frac{\hbar}{2} = -\frac{\gamma \hbar^2 B_0}{2}$$ $$E_{\downarrow} = +\frac{\gamma \hbar^2 B_0}{2}$$

The energy difference between these states is: $$\Delta E = E_{\downarrow} - E_{\uparrow} = \gamma \hbar B_0$$

The corresponding **Larmor frequency** is: $$\nu_L = \frac{\Delta E}{h} = \frac{\gamma B_0}{2\pi}$$

or in angular frequency: $$\omega_L = 2\pi \nu_L = \gamma B_0$$

This is the fundamental frequency at which the muon's spin precesses around the magnetic field.

### Spin Precession in the Laboratory Frame

In the lab frame, a muon spin initially polarized along **B₀** will precess around the field direction at the Larmor frequency. The magnetization vector **M** evolves as: $$\frac{d\mathbf{M}}{dt} = \gamma \mathbf{M} \times \mathbf{B}$$

If **M** starts aligned with **B₀**, it maintains this direction. However, if it has a component perpendicular to **B₀**, that component rotates around **B₀** at frequency ωL.

---

## Part 3: The Rotating Frame and Resonance

### Introduction of an RF Field

Now we apply an additional **radiofrequency (RF) field** perpendicular to **B₀**. Conventionally:

- **B₀** = B₀ **ẑ** (static field, along z-axis)
- **B₁(t)** is a linearly polarized RF field oscillating at frequency νRF

A linearly polarized field can be decomposed into two circularly polarized components rotating in opposite directions. The component rotating in the same direction as the muon precession (the "on-resonance" component) is retained when we transform to the rotating frame.

### The Rotating Frame Picture

Transform to a reference frame rotating at the RF frequency νRF around the z-axis. In this frame, the lab-frame oscillations at νRF appear static.

The effective magnetic field in the rotating frame is: $$\mathbf{B}_{\text{eff}} = \mathbf{B}_1 + \frac{\omega_{RF} - \omega_L}{\gamma}\hat{\mathbf{z}}$$

where **B₁** is the magnitude of the RF field (along, say, the x-axis in the rotating frame).

**Key insight:** When νRF = νL (the RF is on resonance), the second term vanishes, and only **B₁** remains. The muon spin then experiences a much weaker effective field and can be easily tipped away from the z-axis.

When off-resonance (νRF ≠ νL), the effective field has two components:

- A residual field proportional to (νRF - νL)
- The RF field **B₁**

This creates an "off-resonance" precession around a tilted effective field, reducing the effectiveness of the RF at inducing transitions.

### Resonance Condition and Rabi Oscillations

At exact resonance (νRF = νL), the muon spin precesses around **B₁** at the **Rabi frequency**: $$\omega_R = \gamma B_1$$

Starting from a spin polarized along z (|↑⟩ state), the RF field can:

- Tip it into a superposition of spin states
- Eventually flip it to |↓⟩ (with a π-pulse: RF applied for time t such that ωRt = π)
- Oscillate it back and forth between states (Rabi oscillations)

These transitions between |↑⟩ and |↓⟩ are **resonant transitions**—they occur most efficiently when the RF frequency matches the Larmor frequency.

---

## Part 4: Why Polarization is Lost at Resonance

### The Connection to μSR Detection

Recall that muon decay is asymmetric: positrons are preferentially emitted along the muon's spin direction. The **time-integrated muon spin polarization** is proportional to the z-component of the muon spin averaged over the sample.

**Before RF exposure:**

- Muons are polarized along the applied field **B₀** (along z)
- Average ⟨Iz⟩ is at its maximum
- Positrons are preferentially emitted along z
- We observe maximum asymmetry

**During RF resonance:**

- When νRF = νL, the RF field efficiently induces |↑⟩ ↔ |↓⟩ transitions
- Muon spins are tipped away from the z-axis into superpositions or flipped
- The z-component of the magnetization decreases: ⟨Iz⟩ → 0
- Positron emission becomes more isotropic
- We observe **reduced asymmetry**

This reduction in asymmetry (equivalently, reduction in time-integrated polarization) is the **signature of resonance**. By sweeping the RF frequency and measuring the asymmetry as a function of νRF, we find a dip at νRF = νL.

### Multiple Transitions in Hyperfine-Coupled Systems

In a muoniated radical (a molecule with a muon bonded to it), the muon's spin couples to electron and nuclear spins via hyperfine interactions. This creates **four energy levels** (for a radical with one nearby nucleus of spin I = 1/2):

The hyperfine coupling shifts the energy levels. Instead of a single transition at νL, there are **four allowed transitions** (governed by selection rules: ΔμI = ±1, where μI is the muon spin quantum number).

The RF resonance condition is met for each allowed transition at slightly different fields or frequencies. A broadened resonance line (or multiple overlapping lines) is typically observed.

---

## Part 5: RF-μSR in Dilute Solutions

### Why RF-μSR is Necessary

**TF-μSR (Transverse Field)** works well for solid samples where muons occupy specific lattice sites. The muons experience well-defined magnetic fields, and the time-resolved oscillations directly reveal the local field magnitude.

**ALC-μSR (Avoided Level Crossing)** exploits level crossings and anticrossings in the hyperfine structure—useful when specific field conditions create interesting dynamics.

However, in **dilute solutions**, muons diffuse and encounter many different molecular environments. The magnetic field experienced by each muon fluctuates with time, and the resonance lineshape is broadened. TF-μSR and ALC-μSR struggle because the well-defined resonance conditions they rely on are smeared out.

**RF-μSR is ideal here:** By sweeping the RF frequency at a fixed magnetic field (or sweeping the field at a fixed frequency), we can map out the entire resonance line, including its breadth. This directly reveals the **hyperfine coupling constant Aμ** and spin relaxation effects.

### The Resonance Condition in the High-Field Limit

In the high-field limit (external field B₀ >> hyperfine field), the transition frequencies are determined by: $$\nu_{RF} = \nu_L + \Delta\nu_{\text{hfs}}$$

where Δνhfs represents the hyperfine shifts for the allowed transitions.

Rearranging to find the resonance **magnetic field** for a given RF frequency: $$B_{\text{res}} = \frac{h\nu_{RF}}{2\pi\gamma} - \frac{h\Delta\nu_{\text{hfs}}}{2\pi\gamma}$$

Or more simply: $$B_{\text{res}} = \frac{\nu_{RF}}{\gamma/2\pi} - B_{\text{hfs}}$$

where Bhfs is the hyperfine field contribution (related to Aμ).

**This is what "Equation 12.8" likely refers to** in your original text—the high-field limit result that the resonance field is proportional to the RF frequency, with hyperfine shifts as corrections.

---

## Part 6: Putting It Together: RF-μSR Experiment

### Experimental Setup

1. **Polarized muon beam:** Muons are created with their spins aligned by the production mechanism
2. **Sample & B₀ field:** The sample sits in a static magnetic field B₀
3. **RF irradiation:** An RF coil delivers oscillating field at frequency νRF
4. **Positron detector:** Detects decay positrons and their asymmetry

### The Measurement

1. At a fixed B₀, sweep νRF across a range
2. At each frequency, measure the muon spin asymmetry parameter A (which reflects ⟨Iz⟩)
3. Plot A vs. νRF

**Result:** A dip (or multiple dips for hyperfine-split transitions) appears at resonance frequencies where νRF matches the Larmor frequency plus hyperfine shifts.

### Why No Polarization Loss from Mu → Muoniated Radical

When a muon forms a chemical bond (Mu → muoniated radical), there is **no discontinuous loss of spin polarization** because:

- The muon's spin state remains an eigenstate of the system
- The initial z-axis (along **B₀**) is still a special direction defined by the external field
- Only after RF excitation does the muon's spin begin to precess away from the z-axis in the rotating frame

The key point: the muon polarization is "carried forward" into the muoniated radical. The external field **B₀** continues to define the quantization axis, and the muon's spin remains quantized along this axis until disturbed by the RF field or other perturbations.

---

## Summary

**RF-μSR fundamentals:**

1. A polarized muon spin precesses at the Larmor frequency ωL = γB₀ in an external field
2. An RF field perpendicular to B₀, when tuned to resonance (νRF = νL), efficiently induces spin transitions
3. These transitions tip the muon spins away from the quantization axis, reducing the z-component of magnetization
4. This reduction appears as a dip in the detected muon spin asymmetry
5. In hyperfine-coupled systems (like muoniated radicals), multiple transitions occur at slightly different frequencies, revealing the hyperfine coupling constant Aμ
6. In dilute solutions, RF-μSR maps the full resonance lineshape, providing detailed information about the magnetic environment

The technique is "time-consuming" because it requires frequency or field sweeping and does not provide time-resolved data like TF-μSR, but it is essential for measuring hyperfine couplings in systems where TF-μSR and ALC-μSR cannot be applied.