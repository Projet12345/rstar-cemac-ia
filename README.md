# Estimation du taux d'intérêt naturel dans la CEMAC par intelligence artificielle

**Auteurs :**
Anderson Nguetoum Likeufack¹ et Mathurin Soh²

¹ Chercheur indépendant, Cameroun
² Professeur, Université de Dschang, Faculté des Sciences,
  Département d'Informatique et de Mathématiques, Cameroun

**Statut :** Recherche en cours — working paper en préparation
**Dernière mise à jour :** Avril 2026

---

## Présentation du projet

Ce dépôt contient le code source, les notebooks et les données
associés à un article de recherche proposant une méthodologie
d'intelligence artificielle pour estimer le taux d'intérêt
naturel (r*) dans la zone de la Communauté Économique et
Monétaire de l'Afrique Centrale (CEMAC).

Le taux d'intérêt naturel est le taux d'intérêt réel compatible
avec un output à son niveau potentiel et une inflation stable.
C'est la variable de référence qui permet à une banque centrale
d'évaluer si sa politique monétaire est restrictive,
expansionniste ou neutre. Étant inobservable, il doit être
estimé à partir des données macroéconomiques disponibles.

Ce projet propose une alternative aux approches semi-structurelles
classiques en exploitant les méthodes d'IA modernes pour capturer
les non-linéarités, réduire les intervalles d'incertitude et
identifier explicitement les déterminants de r* dans la CEMAC.

---

## Contexte et motivation

La BEAC (Banque des États de l'Afrique Centrale) utilise le
TIAO (Taux d'Intérêt des Appels d'Offres) comme instrument
principal de politique monétaire depuis la réforme de 2018.
Évaluer l'orientation de cette politique suppose de disposer
d'une mesure fiable de r*.

Le working paper BEAC BWP N°01/25 (Mounkala, 2025) constitue
la première estimation de r* pour la CEMAC. Ce projet en
propose une version améliorée sur trois points :

- Capture des non-linéarités par des architectures de deep learning
- Intervalles de confiance plus étroits grâce à la quantification
  bayésienne de l'incertitude
- Identification explicite des déterminants de la composante
  résiduelle z (cours du pétrole, taux mondiaux, taux de change)

---

## Structure du dépôt

```
rstar-cemac-ia/
│
├── notebooks/
│   ├── NB01_Données_CEMAC.ipynb        # Construction du dataset
│   └── NB02_Benchmark_BEAC.ipynb       # Réplication modèle BEAC
│
├── data/
│   └── cemac_dataset_principal.csv     # Dataset principal (1991-2023)
│
├── figures/
│   ├── fig1_series_observees_cemac.png # Figure 1 de l'article
│   └── fig2_rstar_benchmark.png        # Figure 2 de l'article
│
├── article/
│   ├── article_main.tex                # Code LaTeX de l'article
│   └── references.bib                  # Bibliographie BibLaTeX
│
└── README.md
```

---

## Données

Le dataset couvre la zone CEMAC sur la période **1991Q1–2023Q4**
(132 observations trimestrielles) et contient quatre variables :

| Variable | Description | Source |
|---|---|---|
| `log_PIB` | Logarithme du PIB réel agrégé CEMAC | Banque Mondiale (NY.GDP.MKTP.KD) |
| `inflation` | Taux d'inflation annualisé pondéré (%) | FMI World Economic Outlook |
| `TIAO` | Taux directeur BEAC annualisé (%) | BEAC / Archives CPM |
| `taux_reel` | Taux d'intérêt réel = TIAO − inflation (%) | Calculé |

**Pays couverts :** Cameroun, Gabon, Congo (Rép.), Tchad,
République Centrafricaine, Guinée Équatoriale.

**Méthodes de construction :**
- PIB agrégé trimestrialisé par la méthode de Denton-Cholette
- Inflation pondérée par le PIB 2010, lissée par moyenne mobile
- TIAO reconstruit depuis les archives officielles des sessions CPM

Le dataset est également disponible sur
[Kaggle](https://www.kaggle.com/datasets/andersonnguetoum/cemac-macroeconomic-dataset).

---

## Notebooks

### NB01 — Construction des données CEMAC

Construction complète du dataset à partir des sources publiques.
Téléchargement depuis la Banque Mondiale et le FMI,
trimestrialisation du PIB, reconstruction du TIAO, et production
de la Figure 1 de l'article.

**Environnement :** Google Colab + Google Drive
**Durée d'exécution :** ~5 minutes

### NB02 — Benchmark BEAC

Réplication du modèle semi-structurel de Lewis & Vazquez-Grande
(2018) tel qu'implémenté dans le working paper BEAC BWP N°01/25.
Estimation bayésienne par ensemble MCMC (emcee), extraction de
r* par filtre de Kalman et lisseur de Rauch-Tung-Striebel.

**Environnement :** Google Colab + Google Drive
**Durée d'exécution :** ~35 minutes (chauffe + production MCMC)

---

## Résultats préliminaires

Les résultats du benchmark (NB02) sur la période 1994Q2–2023Q4 :

- r* médian autour de **0.35%** sur la période récente
- Dominance de la composante z sur g — les facteurs extérieurs
  pilotent davantage r* que la croissance interne
- Politique expansionniste en 2022 due au choc inflationniste
  post-Covid, retour en restrictive à partir de 2023Q2

Ces résultats sont cohérents avec Mounkala (2025) et servent
de benchmark pour les modèles IA en cours de développement.

---

## Prérequis

```python
pip install pandas numpy matplotlib statsmodels emcee wbdata
```

Les notebooks sont conçus pour Google Colab avec sauvegarde
sur Google Drive.

---

## Références

- Lewis, K.F. & Vazquez-Grande, F. (2018). Measuring the Natural
  Rate of Interest: A Note on Transitory Shocks. *Journal of
  Applied Econometrics*, 34(3), 425–436.

- Laubach, T. & Williams, J.C. (2003). Measuring the Natural
  Rate of Interest. *The Review of Economics and Statistics*,
  85(4), 1063–1070.

- Mounkala, E.U. (2025). Évaluation du taux d'intérêt naturel
  dans la CEMAC. *BEAC Working Paper BWP N° 01/25*.

- Lim, B. et al. (2020). Temporal Fusion Transformers for
  Interpretable Multi-horizon Time Series Forecasting.
  *International Journal of Forecasting*, 37(4), 1748–1764.

---

## Citation

```
Nguetoum Likeufack, A. & Soh, M. (2026). Estimation du taux
d'intérêt naturel dans la CEMAC par intelligence artificielle.
Working paper. GitHub : https://github.com/Projet12345/rstar-cemac-ia
```

---

## Licence

MIT License. Les données sont issues de sources publiques
(Banque Mondiale, FMI) et sont librement réutilisables
avec attribution.

---

## Contact

Anderson Nguetoum Likeufack
nguetoumanderson@gmail.com
[LinkedIn](https://www.linkedin.com/in/anderson-nguetoum-b8888124a/)
[ORCID](https://orcid.org/0009-0004-0864-1811)
