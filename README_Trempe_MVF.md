# 🔩 Simulation de Trempe d'un Cylindre en Acier AISI 1045
### Méthode des Volumes Finis (MVF) — Python

> **Projet de modélisation numérique** | M1 Mécanique Énergétique | USTHB  
> Auteur : **Khaled Merini** | Encadrant : Prof. Chikh | 2025–2026

---

## 📌 Description

Ce projet simule le **refroidissement par trempe** d'un cylindre d'acier AISI 1045 en utilisant la **Méthode des Volumes Finis (MVF)** en coordonnées cylindriques 1D (radial).

Le procédé consiste à chauffer la pièce à **850 °C** puis à la plonger dans un fluide à **40 °C**. Le code vérifie si les deux contraintes du cahier des charges industriel sont respectées.

---

## 🎯 Objectifs et contraintes du cahier des charges

| Contrainte | Critère | Importance |
|---|---|---|
| 🔩 **Métallurgique** | Vitesse de refroidissement au centre ≥ **30 °C/s** entre 800 °C et 500 °C | Durcissement de l'acier |
| ⚙️ **Mécanique** | Écart thermique maximal ΔT_max ≤ **200 °C** | Éviter les fissures |

---

## ⚙️ Modèle physique

### Équation gouvernante (1D cylindrique transitoire)

$$\rho c_p(T)\,\frac{\partial T}{\partial t} = \frac{1}{r}\frac{\partial}{\partial r}\!\left(r\,k(T)\,\frac{\partial T}{{\partial r}}\right)$$

### Conditions aux limites

- **Centre** $(r = 0)$ : symétrie axiale → flux nul
- **Surface** $(r = R)$ : convection → $-k\,\partial T/\partial r = h(T_{\text{surface}} - T_\infty)$

### Particularités du modèle

- Propriétés thermiques **dépendantes de la température** : $k(T)$ et $c_p(T)$
- Conductivités aux interfaces calculées par **moyenne harmonique**
- Schéma temporel **explicite** avec critère de stabilité de Fourier ($Fo \leq 0.4$)

---

## 📁 Structure du projet

```
trempe-mvf/
│
├── Merini_Khaled_IMPROVED.ipynb   # Notebook principal (version améliorée)
├── README.md                      # Ce fichier
│
├── figures/
│   ├── A_grid_independence.png    # Étude d'indépendance du maillage
│   ├── B_temperature_history.png  # Historiques thermiques centre/surface
│   ├── C_thermal_gradient.png     # Évolution de ΔT(t)
│   ├── D_parametric_study.png     # Analyse paramétrique en h
│   └── properties_AISI1045.png    # Propriétés thermiques tabulées
│
└── data/
    └── temperature_history.dat    # Export des données numériques
```

---

## 🚀 Lancer le notebook

### Prérequis

```bash
pip install numpy matplotlib jupyter
```

### Exécution

```bash
jupyter notebook Merini_Khaled_IMPROVED.ipynb
```

---

## 📊 Résultats principaux

### Partie A — Indépendance du maillage

Trois discrétisations testées : N = 10, 20, et 40 nœuds.  
L'écart relatif entre N = 20 et N = 40 est inférieur à **1 %** → maillage indépendant à N = 40.

### Partie D — Choix du fluide de trempe optimal

| Fluide / Régime | h [W/m²·K] | Vitesse [°C/s] | ΔT_max [°C] | Métallurgie | Mécanique |
|---|---|---|---|---|---|
| Huile | 500 | ~8.6 | ~133 | ❌ trop lent | ✅ |
| Eau stagnante | 1000 | ~7.5 | ~222 | ❌ trop lent | ❌ |
| **Eau agitée modérément** | **3000** | **~15.4** | **~198** | ✅ | ✅ |
| Eau agitée violemment | 5000 | ~20.5 | ~500 | ✅ | ❌ trop fort |

> ✅ **Condition optimale : h = 3000 W/m²·K** (agitation modérée)  
> Seule valeur qui satisfait simultanément les deux contraintes du cahier des charges.

---

## 🧠 Concepts clés abordés

- Méthode des Volumes Finis en coordonnées cylindriques
- Schéma explicite et critère de stabilité de Courant-Friedrichs-Lewy
- Propriétés thermiques dépendantes de la température
- Analyse d'indépendance du maillage
- Étude paramétrique et optimisation d'un procédé industriel

---

## 📚 Référence

> Patankar, S. V. *Numerical Heat Transfer and Fluid Flow*. McGraw-Hill, 1980.

---

## 👤 Auteur

**Khaled Merini**  
M1 Mécanique Énergétique — USTHB, Alger  
Faculté de Génie Mécanique et de Génie des Procédés
