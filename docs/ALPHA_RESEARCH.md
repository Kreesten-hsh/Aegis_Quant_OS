# Alpha Research Framework

> [!NOTE]
> **STATUT DU FRAMEWORK : COMPOSANT LOGICIEL VALIDE (RECHERCHE CONCLUE)**
> Ce framework d'évaluation d'alpha est une brique logicielle validée et réutilisable du projet Aegis Quant OS. Il a servi à évaluer les 216 hypothèses (ADR 0017 à 0031), concluant à l'absence de signal univarié exploitable net de frais d'exécution.

Ce document décrit l'architecture et l'utilisation du **Alpha Research Framework** du projet Aegis Quant OS.

---

## Objectif

Transformer le *Feature Engine* en laboratoire de recherche quantitative. Ce framework permet de mesurer objectivement si les features générées possèdent un réel pouvoir prédictif hors-échantillon.

Le système suit des principes institutionnels stricts : aucun Machine Learning non justifié, pas de backtest complet sans validation d'alpha préalable. Seulement la recherche de signal brut.

---

## Architecture & Concepts Clés

L'évaluation repose sur des métriques quantitatives robustes :

*   **Information Coefficient (IC)** : Corrélation de Spearman entre une feature à l'instant $T$ et les rendements futurs à l'instant $T+N$. Un IC élevé prouve un signal prédictif.
*   **Information Ratio (IR)** : Ratio entre la moyenne des IC et leur écart-type ($\frac{\text{Mean(IC)}}{\text{Std(IC)}}$). Il évalue le rendement corrigé du risque de la feature.
*   **Stabilité** : Calcule si la feature maintient une relation constante dans le temps. C'est le ratio de fenêtres roulantes ayant le même signe d'IC que l'IC global.
*   **Score Final** : Calculé par $\text{abs(IR)} \times \text{Stabilité}$. Ce score sert à classer les features par pertinence.

---

## Composants Techniques

Le framework est construit selon l'architecture hexagonale :

1.  **Domain (`src/aegis_trade/domain/research.py`)** :
    *   `FeatureScore` : Représente les statistiques d'une feature (IC, IR, mean, variance, etc.).
    *   `AlphaResearchResult` : Le résultat global de l'évaluation (Scores, Corrélation entre features, Top/Bottom features).
    *   `IResearchEngine` : Interface (Port) du moteur d'évaluation.
2.  **Infrastructure (`src/aegis_trade/infrastructure/research/research_engine.py`)** :
    *   Implémentation concrète `ResearchEngine` exploitant Pandas, Numpy, et Scipy (corrélation de Spearman).
    *   Ingestion vectorisée sans boucles Python.
3.  **Rapport (`src/aegis_trade/infrastructure/research/research_report.py`)** :
    *   Convertit `AlphaResearchResult` en document JSON standardisé.

---

## Utilisation

L'évaluation des signaux s'effectue avec le script dédié `scripts/run_feature_research.py`. Ce script :
1.  Charge les features depuis le `FeatureStore` (Local Data Lake, format Parquet).
2.  Évalue les features sur une cible temporelle (forward returns).
3.  Génère un rapport JSON des Top Features et des Bottom Features.

### Lancer une évaluation

```bash
python scripts/run_feature_research.py
```

---

## Règles & Garde-fous

- **Aucun package externe de Machine Learning** n'est toléré dans cette couche (`sklearn`, `torch`, `qlib`).
- Le domaine est **pur** : pas de `pandas` ou `numpy` dans `domain/`.
- Les corrélations aberrantes (`NaN`) liées à des variances nulles ou des valeurs manquantes sont pénalisées (Score de 0).
