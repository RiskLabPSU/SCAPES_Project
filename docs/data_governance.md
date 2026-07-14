# SCAPES Consortium: Data Governance & Anonymization Protocol

## 1. Overview and Ethical Mandate    

The SCAPES project generates high-resolution datasets capturing human spatial behavior, peridomestic layout, and Lassa virus (LASV) seroconversion across rural Nigeria. Due to the sensitive nature of biological infection status and daily movement routines, open-access release of raw, unadjusted spatial coordinates poses re-identification risks. 

This document outlines the data governance architecture and anonymization protocols enforced across all SCAPES public data releases, ensuring strict adherence to National Health Research Ethics Committee (NHREC) guidelines and FAIR principles.

---

## 2. Spatial Anonymization: The Null-Space Affine Transformation   

To preserve the analytical utility of high-resolution human GPS tracking (i-GotU GT-600B) and drone-derived exposure surfaces without disclosing geographic locations, all public release spatial datasets undergo an **Affine Transformation into a local null-space**.

### Mathematical Specification    

For any given study village $V$, true geographic coordinates $(X_{\text{true}}, Y_{\text{true}})$ expressed in Universal Transverse Mercator (UTM) are projected into an anonymized Cartesian coordinate space $(X_{\text{anon}}, Y_{\text{anon}})$ via the linear transformation:

$$\begin{bmatrix} X_{\text{anon}} \\ Y_{\text{anon}} \end{bmatrix} = s \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix} \begin{bmatrix} X_{\text{true}} - X_{\text{origin}} \\ Y_{\text{true}} - Y_{\text{origin}} \end{bmatrix} + \begin{bmatrix} \Delta X \\ \Delta Y \end{bmatrix}$$

Where:    

* **Translation ($(X_{\text{origin}}, Y_{\text{origin}})$):** The true village centroid is shifted to an arbitrary local origin $(0,0)$.
* **Rotation ($\theta$):** The coordinate axes are rotated by a village-specific random angle $\theta \in [0, 2\pi)$. This prevents alignment with macro-geographic satellite features (e.g., rivers, roads).
* **Scaling ($s$):** In standard public releases, $s = 1.0$ is maintained to preserve true metric distances (e.g., meters moved, buffer radii, home-range areas). Where absolute distance obfuscation is ethically mandated, a uniform scalar $s \sim \mathcal{U}(0.9, 1.1)$ is applied.
* **Offset ($(\Delta X, \Delta Y)$):** An arbitrary false northing and false easting offset is added to project the coordinates into an unmapped ocean or null-space polygon.

### Analytical Preservation   

Because Euclidean distances and relative topological relationships are mathematically invariant under orthogonal transformation ($s=1$), downstream researchers can fully reproduce:    

* Autocorrelated Kernel Density Estimation (aKDE) home ranges.
* Integrated Step Selection Functions (iSSF) and step-length distributions.
* Spatial lag metrics and distance-to-ecotone covariates.

---

## 3. Epidemiological & Sero-Behavioral Aggregation   

While movement tracks exist in anonymized null-spaces, human biological data (ELISA IgG/IgM serostatus and RT-PCR results) are subjected to strict hierarchical aggregation before public release:

1. **Individual Serological Records:** Never released with household-level spatial identifiers. In public datasets, individuals are assigned a randomized alphanumeric ID tethered only to a coarse age-class (Child, Adolescent, Adult Male, Adult Female) and occupational motif.
2. **Spatial Risk Mapping:** For broad-scale modeling outputs (Aim 2), biological endpoints are rasterized and aggregated to a regular **$1 \text{ km}^2$ grid** across the hyperendemic zone.
3. **Administrative Reporting:** For national and international disease surveillance repositories, case occurrences and seroprevalence figures are aggregated strictly at the **Local Government Area (LGA)** or State level.

---

## 4. Accessing Restricted-Access Primary Data    

Researchers seeking access to pre-transformation spatial coordinates or linked human-household serological records for validation or collaborative synthesis should submit a formal data access request to the SCAPES Project Leadership team via `[Insert Contact Email / Issue Template]`. Access is contingent upon Institutional Review Board (IRB) concurrence and a signed Data Transfer Agreement (DTA) prohibiting re-identification attempts.