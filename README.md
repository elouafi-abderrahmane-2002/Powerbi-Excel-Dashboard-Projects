# 📊 Power BI & Excel Dashboard Projects

Des données brutes ne veulent rien dire tant qu'on ne les visualise pas correctement.
Ce repo est ma collection de dashboards analytiques construits avec Power BI — chaque
projet part d'un jeu de données réel, passe par une phase de nettoyage Excel/SQL/Python,
et finit par un rapport interactif exploitable par des décideurs non-techniques.

---

## Ce que contient ce repo

```
Powerbi-Excel-Dashboard-Projects/
│
├── 📁 Sales-Dashboard/
│   ├── data/           ← dataset brut (CSV/Excel)
│   ├── cleaning/       ← scripts SQL & Python de nettoyage
│   ├── report.pbix     ← fichier Power BI final
│   └── README.md       ← contexte et insights clés
│
├── 📁 HR-Analytics/
│   ├── data/
│   ├── cleaning/
│   ├── report.pbix
│   └── README.md
│
├── 📁 Finance-KPI/
│   ├── data/
│   ├── cleaning/
│   ├── report.pbix
│   └── README.md
│
└── 📁 Customer-Churn/
    ├── data/
    ├── cleaning/
    ├── report.pbix
    └── README.md
```

---

## Pipeline de chaque projet

```
  Données brutes
  (CSV / Excel / SQL Server)
         │
         │  Étape 1 : Nettoyage
         ▼
  ┌─────────────────────────┐
  │  Excel / SQL / Python   │
  │                         │
  │  - Supprimer doublons   │
  │  - Gérer valeurs nulles │
  │  - Normaliser formats   │
  │  - Agréger tables       │
  └────────────┬────────────┘
               │
               │  Étape 2 : Modélisation
               ▼
  ┌─────────────────────────┐
  │      Power Query        │
  │                         │
  │  - Transformations M    │
  │  - Relations entre      │
  │    tables (star schema) │
  │  - Calendrier DAX       │
  └────────────┬────────────┘
               │
               │  Étape 3 : Visualisation
               ▼
  ┌─────────────────────────┐
  │       Power BI          │
  │                         │
  │  - Mesures DAX (KPIs)   │
  │  - Graphiques interac.  │
  │  - Filtres & slicers    │
  │  - Drill-through pages  │
  └─────────────────────────┘
               │
               ▼
  Dashboard exploitable par
  les équipes métier & direction
```

---

## Exemple de mesure DAX — Taux de croissance YoY

```dax
Revenue YoY Growth % =
VAR CurrentRevenue = [Total Revenue]
VAR PreviousRevenue =
    CALCULATE(
        [Total Revenue],
        SAMEPERIODLASTYEAR('Calendar'[Date])
    )
RETURN
DIVIDE(
    CurrentRevenue - PreviousRevenue,
    PreviousRevenue,
    0
)
```

Ce genre de mesure permet d'afficher automatiquement la croissance par période
sans toucher aux données sources — c'est la puissance de DAX.

---

## Exemple de nettoyage Python avant import Power BI

```python
import pandas as pd

df = pd.read_csv('sales_raw.csv')

# Supprimer les lignes sans montant
df = df.dropna(subset=['amount'])

# Normaliser les dates
df['date'] = pd.to_datetime(df['date'], dayfirst=True)

# Catégoriser les montants
df['segment'] = pd.cut(df['amount'],
                        bins=[0, 500, 2000, float('inf')],
                        labels=['Small', 'Medium', 'Large'])

df.to_csv('sales_clean.csv', index=False)
# → prêt à être importé dans Power BI via Power Query
```

---

## Ce que j'ai appris

Le plus utile : comprendre la différence entre une **mesure** et une **colonne calculée**
en DAX. Une colonne calculée est évaluée ligne par ligne au chargement — elle est stockée.
Une mesure est évaluée à la volée selon le contexte du filtre actif. Utiliser une colonne
là où il faudrait une mesure multiplie inutilement la taille du modèle et ralentit tout.

L'autre chose : le **star schema**. Charger toutes les données dans une seule table
"fonctionne" — mais ça rend les relations impossibles et les calculs croisés très lents.
Apprendre à séparer les tables de faits et de dimensions correctement a tout changé.

---

*Projet réalisé dans le cadre de ma formation ingénieur — ENSET Mohammedia*
*Par **Abderrahmane Elouafi** · [LinkedIn](https://www.linkedin.com/in/abderrahmane-elouafi-43226736b/) · [Portfolio](https://my-first-porfolio-six.vercel.app/)*
