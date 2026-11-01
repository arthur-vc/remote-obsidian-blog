# Magneto-Optical Trap

## Introduction
A Magneto-Optical Trap (MOT) is a system designed to confine and cool neutral atoms through laser cooling and spatially varying magnetic fields to temperature as low as several microkelvins. The premise of a MOT operates through the intersection of a the zero of a weak quadrupolar magnetic field produced by an anti-helmholtz pair and several circularly polarised red-detuned optical molasses beams. The optical molasses beams provide the laser cooling configuration through pairs of counter-propagating orthogonally polarised laser beams intersecting in the region where the atoms are confined by the magnetic field. As the atoms travel away from the zero  field center, the spatially varying Zeeman Shift brings an atomic transition into resonanace with the laser beams. The beams progating in the oposite direction of this atomic movement are polarised in a way for which the atom is more inclined to absorb due to its Zeeman Shifted level. Each absorbed photon transfers some momentum $hk$ to the atom in the beams direction (towards the centre), and the re-emmited photons randomised directions average out to a net zero force. The result is a scattering force back to the centre, an repeated absorption and spontaneous emissions cycles eventually reduce atomic velocity to tens of centimeters per second - depending on atmoic species.
## Components

### Faraday Isolator
**Description.** A Faraday isolator is a one-way valve for light. It has three elements in a row: an input polariser, a rod of magneto-optic crystal (typically terbium gallium garnet, TGG) inside a ring magnet whose field points along the beam, and an output polariser tilted 45° from the first. It sits directly after the diode laser.

**Physics.** Light is a transverse wave, and its polarisation is the direction in which the electric field oscillates. A polariser transmits only one direction, and light polarised at angle θ to the axis is transmitted with intensity I = I₀cos²θ (Malus’s law) [5].

In the Faraday effect, a magnetic field B along the beam rotates the plane of polarisation by

θ = VBL

where L is the crystal length and V is the Verdet constant of the material (of order 100 rad T⁻¹ m⁻¹ for TGG near 780 nm). The cause is circular birefringence. Linear light is a sum of left- and right-circular components. The field splits the crystal’s electronic energy levels (Zeeman effect), so the two components see different refractive indices n_L ≠ n_R. They accumulate different phases, and on recombining the plane of polarisation has rotated by

θ = πL(n_L − n_R)/λ.

For θ = 45° with B ≈ 1 T, only L ≈ 8 mm of crystal is needed.

The essential property is **non-reciprocity**. The sense of rotation is fixed by the direction of **B**, not by the direction the light travels. In an ordinary optically active medium the rotation undoes itself on a return trip, but in a Faraday rotator it adds.

- _Forward:_ vertical light passes polariser 1, is rotated +45°, and passes polariser 2.
- _Backward:_ light passing back through polariser 2 is rotated a further +45°, ending horizontal. Polariser 1 (vertical) blocks it.

Practical isolators suppress returning light by roughly 30-40 dB while transmitting about 90% forwards.

_[Diagram 2: forward row (0° → 45° → transmitted) and backward row (45° → 90° → blocked).]_

**Role in the MOT.** Diode lasers are very sensitive to optical feedback. Even a tiny fraction of returned light (of order 10⁻⁵ of the output) can pull the laser’s frequency, cause mode hops, or broaden its linewidth [5]. A MOT contains many reflecting surfaces (windows, lenses, the AOM crystal, mirrors), so back-reflection is unavoidable. The isolator protects the laser so that its frequency stays steady and its linewidth stays well below the atomic linewidth Γ/2π ≈ 6 MHz [3]. Every later stage, the detuning in particular, assumes a stable laser frequency.

### Anti-Helmholtz Coil Pair
**Description**
An Anti-Helmholtz Pair involves two coaxial magnetic coils of radius $R$, symmetrically spaced by a distance of $h$ equal to the radius $R$ of each coil. When an equal electric current runs through the electromagnetic pair in opposite directions it generates a weak qaudrupolar magnetic field with regions of nearly uniform magnetic field gradient and a magnetic field cancellation of zero at the centre between both coils. The effect of setting $h=R$ defines a Helmholtz pair, which minimises the non-uniformity of the field at the centre of the coils. Its role within the MOT is predicted upon the Zeeman Effect, ensuring atoms have different energy levels depending on their position relative to the centre. 

**Physics**
3D Quadrapole field - Zeeman Effect


When the pair of two electromagnets of a Helmholtz coil carry an equal electric current in the opposite direction, it is known as **anti-Helmholtz coil**, which creates a region of nearly uniform magnetic field [gradient](https://en.wikipedia.org/wiki/Gradient "Gradient"), and is used for creating [magnetic traps](https://en.wikipedia.org/wiki/Magnetic_trap_\(atoms\) "Magnetic trap (atoms)") for [atomic physics](https://en.wikipedia.org/wiki/Atomic_physics "Atomic physics") experiments.
- **Physics:** Biot-Savart, field cancellation at the midpoint, B_z = A·z with A = 3μ₀NIR²a/(R²+a²)^{5/2}, ∇·B = 0 (radial gradient is half the axial), Zeeman shift ΔE = g_F m_F μ_B B (~1.4 MHz/G).
- **Role:** zero field marks the trap centre, and the position-dependent Zeeman shift (10 G/cm gives 14 MHz at 1 cm) creates the restoring force F = −κz when combined with circularly polarised beams.
- **Diagram:** coils with field lines, plus Zeeman levels vs z with a red-detuned laser line.



### Acousto-Optic Modulator (AOM)
**Description.** An AOM is a crystal (e.g. TeO₂) with a piezoelectric transducer bonded to one face and an acoustic absorber on the opposite face. A radio-frequency (RF) signal of about 80 MHz drives the transducer. The laser beam enters at a small angle and emerges as an undeflected zeroth-order beam plus a diffracted first-order beam, which is the one used. It sits in the main beam before the light is split into six.

**Physics.** The transducer launches a sound wave of wavelength

Λ = v_s/f

where v_s is the speed of sound in the crystal (about 4.2 km/s in TeO₂, so Λ ≈ 52 µm at 80 MHz). Compressed regions are denser and have a slightly higher refractive index (the photoelastic effect), so the sound wave acts as a **moving diffraction grating** of index layers, with no moving mechanical parts.

The layers are many wavelengths thick, so reflections from successive layers add up only at the Bragg angle:

2Λ sinθ_B = λ, i.e. sinθ_B = λ/(2Λ).

For λ = 780 nm, θ_B ≈ 0.43°, and the first-order beam leaves 2θ_B ≈ 0.85° from the zeroth order.

The diffracted light is also frequency-shifted. The layers move at v_s, so reflection from them is Doppler-shifted by Δf = 2v_s sinθ_B/λ = v_s/Λ = f. Equivalently, a photon absorbs a phonon of energy hf, so f_out = f_in + f. The shift equals the RF drive frequency exactly.

The diffracted fraction rises with RF power and reaches about 80-90%. The rise time is roughly the time for sound to cross the beam, about (beam diameter)/v_s, which is about 240 ns for a 1 mm beam. A mechanical shutter takes milliseconds.

_[Diagram 3: crystal, transducer, wavefront lines, incident beam at θ_B, zeroth and first orders separated by 2θ_B.]_

**Role in the MOT.** The cooling force depends on the detuning Δ through a factor of the form 1/[1 + s + 4(Δ/Γ)²] [2], so the detuning must be set to within a fraction of 6 MHz. Doppler cooling works best at roughly Δ ≈ −Γ to −2Γ, and if the light drifted to the blue side it would heat atoms rather than cool them.

In one common design the laser is locked to a Rb spectroscopy feature that sits 133.5 MHz below the cooling transition, and the AOM then adds about +120 MHz (60 MHz in a double-pass arrangement), giving a detuning of about −13.5 MHz ≈ −2.2Γ. The AOM therefore:

- **sets the detuning** electronically (changing f_RF changes Δ),
- **switches the beams off** in under a microsecond, which allows the cloud’s temperature to be measured from its expansion in the dark,
- **controls intensity** through the RF power, balancing the six beams.

In double-pass, the beam is sent through the crystal a second time so that the angular walk-off from changing f (about λδf/v_s) cancels while the shift doubles.

### Quater Wave Plate



## Conclusion
## Scaffold

### Introduction (~½ page)

- **Aim:** cool and trap neutral Rb-87 atoms (~100 µK, ~1 mm cloud) using light and magnetic fields.
- **Two ideas:** red-detuned light gives a velocity-dependent friction force (Doppler cooling); a magnetic field gradient plus circular polarisation gives a position-dependent restoring force.
- **Overview diagram:** six beams, two coils, atom cloud, and the laser chain.
- **Roles table:** one line per component (where it sits, what it does, which force it serves).
- **Name your four components** and state that the laser, lock and polarisation optics are supporting parts, not marked ones.

### Per-component template (1 / 6 / 3)

|Part|Content|
|---|---|
|Description (1)|What it is, where it sits in the MOT, 2-3 sentences + labelled diagram|
|Physics (6)|Equations with every symbol defined, named phenomena, a worked number|
|Role (3)|Which MOT force or requirement it serves, with typical numbers|

### 1. Faraday isolator

- **Description:** polariser, TGG rod in a magnet, polariser at 45°, placed right after the laser.
- **Physics:** polarisation, Malus’s law (briefly), Faraday rotation θ = VBL, circular birefringence (n_L ≠ n_R), non-reciprocity.
- **Role:** diode lasers are hypersensitive to feedback. The isolator keeps frequency and linewidth stable so the detuning is well defined.
- **Diagram:** forward path (0° → 45° → passes) and backward path (45° → 90° → blocked).

### 2. AOM

- **Description:** TeO₂ crystal, piezo transducer, absorber, RF drive.
- **Physics:** Λ = v_s/f, photoelastic effect, Bragg condition sinθ_B = λ/2Λ, frequency shift by Doppler or photon-phonon argument, efficiency vs RF power, rise time ≈ d/v_s.
- **Role:** shifts the beam by ~+120 MHz to sit at about −2Γ, switches in under a microsecond, controls intensity, double-pass to avoid beam walk.
- **Diagram:** crystal, wavefronts, beam at θ_B, zeroth and first orders.

### 3. Fabry-Pérot interferometer

- **Description:** two mirrors (R ≈ 0.99), one on a piezo, photodiode behind. Samples a small fraction of the beam, off the main path.
- **Physics:** multiple-beam interference, Airy function T = 1/[1 + F sin²(δ/2)] with F = 4R/(1−R)², resonance 2nL = mλ, FSR = c/2nL, finesse, resolution, scanning.
- **Role:** confirms the laser is single-mode and stable. State honestly that it can’t resolve sub-MHz linewidth.
- **Diagram:** mirrors with beam zig-zag, then Airy peaks with FSR and FWHM marked.
- **Flag:** this is the one component whose “key to the MOT” status is arguable, so ask on Ed.

### 4. Anti-Helmholtz coils

- **Description:** two coaxial coils, opposite currents, around the chamber.
- **Physics:** Biot-Savart, field cancellation at the midpoint, B_z = A·z with A = 3μ₀NIR²a/(R²+a²)^{5/2}, ∇·B = 0 (radial gradient is half the axial), Zeeman shift ΔE = g_F m_F μ_B B (~1.4 MHz/G).
- **Role:** zero field marks the trap centre, and the position-dependent Zeeman shift (10 G/cm gives 14 MHz at 1 cm) creates the restoring force F = −κz when combined with circularly polarised beams.
- **Diagram:** coils with field lines, plus Zeeman levels vs z with a red-detuned laser line.

### Conclusion (optional, 2-3 lines)

How the four work together: isolator and FP keep the light clean, the AOM sets the detuning (cooling), the coils set the position dependence (trapping).

### References (IEEE, numbered by first citation)

Steck (Rb-87 D line data), Metcalf & van der Straten, Foot, Raab et al. 1987, Saleh & Teich.