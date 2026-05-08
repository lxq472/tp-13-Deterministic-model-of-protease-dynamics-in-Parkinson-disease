# Deterministic Model of Protease Dynamics in Parkinson’s Disease

This project implements and extends the deterministic model of α-synuclein aggregation and proteasome dynamics presented in:

**Sneppen et al., *Modeling proteasome dynamics in Parkinson’s disease*, Phys. Biol. 6 (2009)**

Developed as part of the MSc course:

**Physics of Molecular Diseases**  
Niels Bohr Institute — University of Copenhagen (Prof. Ala Trusina)


---

# 📌 Overview

The model describes the interaction between:

* **Fibrils (F)**
* **Proteasome (P)**
* **Proteasome–fibril complex (C)**

Key idea:

> Protein aggregates can "trap" proteasomes, reducing the cell’s ability to degrade them.

---

# 🔹 1a. Model Implementation & Reduction

## Full Model (7 parameters)

The system includes protofibrils and intermediate complexes:

```text
ds/dt   = m - gamma_s * s * P - omega * s
d(sP)/dt = gamma_s * s * P - nu_s * (sP)
dF/dt   = omega * s - gamma * F * P
dC/dt   = gamma * F * P - nu * C
dP/dt   = Lambda - P/tau - gamma * F * P + nu * C
          - gamma_s * s * P + nu_s * (sP)
```

---

## Reduced Model (4 parameters)

Using quasi-steady-state assumptions, the model simplifies to:

```text
dF/dt = m / (1 + P) - gamma * F * P
dC/dt = gamma * F * P - nu * C
dP/dt = sigma - P - gamma * F * P + nu * C
```

Parameters:

* `m`     : fibril production rate
* `gamma` : binding rate
* `nu`    : degradation rate
* `sigma` : proteasome production

---

## Key Result

* Low `m` → steady state (healthy)
* High `m` → oscillations (pathological)
* Low proteasome → high oligomer accumulation

---

# 🔹 1b. Induced Proteasome Production

Originally:

```text
sigma = constant
```

We extend it to:

```text
sigma = sigma0 + induction_term
```

---

## Cases

### i. Protofibrils (s)

```text
sigma = sigma0 + A * s / (K + s)
```

* Fast response
* Stabilizes system
* Suppresses oscillations

---

### ii. Oligomers (≈ 1/P)

```text
oligomers ≈ 1 / P
sigma = sigma0 + A / (K * P + 1)
```

* Activated when proteasome is low
* Reduces damage
* Does not change core dynamics

---

### iii. Fibrils (F)

```text
sigma = sigma0 + A * F / (K + F)
```

* Slow response
* Introduces delayed feedback
* Enhances oscillations

---

# 🔹 1c. Which Mechanism Has Maximal Impact?

Even though protofibril induction reduces damage the most, the **largest qualitative impact** comes from fibrils.

### Reason:

* Oscillations are caused by **delayed feedback**
* Fibrils are the **slow variable** in the system

**Conclusion:**

> Fibril-induced proteasome production has the strongest effect on system dynamics.

---

# 🔹 1d. Inclusion of Autophagy

## Model Extension

We add an additional degradation term for fibrils:

```text
dF/dt = m / (1 + P) - gamma * F * P - delta * F
```

where:

* `delta` = autophagy rate

---

## Interpretation

* Represents lysosomal degradation
* Acts in parallel with proteasome
* Targets larger aggregates

---

## Effect on Dynamics

### Without autophagy:

* Strong oscillations
* Low proteasome levels
* High oligomer accumulation

### With autophagy:

* Reduced fibrils
* Higher proteasome availability
* Suppressed oscillations
* Lower toxicity

---

## Key Insight

> Autophagy stabilizes the system by reducing fibril load and weakening the feedback loop.

---

# 📊 Simulations

Simulations compare:

* Constant vs induced proteasome production
* Different induction mechanisms (s, oligomers, F)
* With vs without autophagy

Metrics:

* Proteasome concentration `P(t)`
* Fibril concentration `F(t)`
* Oligomer accumulation ~ ∫(1/P) dt

---

# 📌 Conclusions

* Aggregation can destabilize cellular regulation
* Oscillations arise from delayed feedback
* Early sensing stabilizes
* Late sensing destabilizes
* Autophagy acts as protection

---

# 📚 References

Sneppen, K. et al. (2009), "Modeling proteasome dynamics in Parkinson’s disease", Physical Biology 6, 036005
Prof. Ala Trusina, NFYK14009U Physics of Molecular Diseases, Niels Bohr Institute, University of Copenhagen
