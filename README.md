# Aegis Quant OS

![Python Version](https://img.shields.io/badge/python-3.11-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Architecture](https://img.shields.io/badge/architecture-Clean%20%7C%20Hexagonal-orange)
![Status](https://img.shields.io/badge/research-concluded%20(256%20hypotheses%20tested)-red)

Aegis Quant OS est une plateforme de recherche quantitative et d'évaluation d'hypothèses d'alpha en Clean Architecture / Domain-Driven Design (DDD). Le système isole la logique du domaine financier (actifs, positions, signaux, gestion des risques) des adaptateurs d'infrastructure (moteurs de backtest, gateways de données, modélisation ML, agrégation multi-agents).

---

## Contexte du Projet

Le projet Aegis Quant OS a été conçu pour appliquer une méthodologie scientifique rigoureuse à l'exploration d'alphas systématiques sur plusieurs classes d'actifs (indices synthétiques, métaux précieux, crypto-monnaies). L'objectif initial était de construire un pipeline de décision automatisé appuyé sur un filtrage strict des coûts d'exécution, une correction statistique pour tests multiples et une architecture modulaire réutilisable.

---

## Résultat de la Recherche Quantitative (Bilan Final par ADR)

Conformément au principe de transparence et de rigueur scientifique (règle anti-survente), le tableau ci-dessous récapitule l'ensemble des hypothèses d'alpha évaluées, ventilées et sourcées par leur Architecture Decision Record (ADR) d'origine :

### Tableau Récapitulatif des Hypothèses Évaluées et Réfutées

| Famille / Périmètre d'évaluation | Nb Hypothèses | Résultats & Signatures Statistiques | Statut Final | Référence ADR |
|---|---|---|---|---|
| **Features M1 Crash 1000 & Boom 1000 (SIG-02)** | 25 | 0/25 significatives ($|t| \le 1.87$, win rates holdout 25.8%–30.3%) | 0 validées | [ADR 0024](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0024-sig-02-rejected-feature-research-skipped.md) |
| **Features M1 Or XAUUSD (GOLD-01)** | 25 | 0/25 significatives ($|t| \le 1.31$, gate coût 1.859 bps validé dès H5) | 0 validées | [ADR 0025](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0025-gold-cost-and-tradability.md) |
| **Gold Groupe A (Technique D1 & H4)** | 76 | 0/76 significatives post-correction FDR Benjamini-Hochberg ($q=0.05$) | 0 validées | [ADR 0030](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0030-h4-d1-feature-research-and-macro-discovery.md) |
| **Gold Groupe B (Macro FRED D1 & H4)** | 52 | 0/52 (2 DXY level rejetées pour régression spurie non cointégrée) | 0 validées | [ADR 0030](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0030-h4-d1-feature-research-and-macro-discovery.md) |
| **Gold Groupe C (Positionnement COT 088691)** | 16 | 0/16 significatives (Net Spec Level $I(0)$, $t$-stat HAC $\le 0.13$) | 0 validées | [ADR 0030](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0030-h4-d1-feature-research-and-macro-discovery.md) |
| **Synthétiques H4 (Crash/Boom H4 Spike)** | 60 | 0/60 significatives post-correction FDR Benjamini-Hochberg ($q=0.05$) | 0 validées | [ADR 0030](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0030-h4-d1-feature-research-and-macro-discovery.md) |
| **Crypto Trend 24/7 & ML Cross-Sectional** | 2 | Option A (Trend 24/7 réfuté post-frais 10 bps) ; Option B (Rank IC = -0.0087) | 0 validées | [ADR 0031](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0031-crypto-trend-and-ml-cross-sectional-ranking-rejected.md) |
| **Total Cumulé** | **256** | **0 / 256 (0.0% validées en production)** | **0 validées** | **ADR 0017 à 0031** |

Tous les signaux directionnels univariés usuels sur séries temporelles de prix et de macroéconomie ont été formellement réfutés après déduction des péages d'exécution réels (1.859 bps A/R sur Deriv/Or et 10.0 bps sur Crypto Spot) et application des corrections FDR (False Discovery Rate) et Bonferroni.

---

## Statut de Clôture du Projet

Le projet de recherche Aegis Quant OS est **officiellement clos**.

La réfutation empirique des hypothèses constitue un résultat scientifique valide et documenté : l'absence d'edge exploitable net de frais d'exécution sur les horizons et univers étudiés démontre l'inaptitude des signaux testés à générer du P&L réel. Aucune stratégie n'a atteint le jalon de validation AI-07b (engagement de capital réel).

Le projet n'est pas poursuivi sous forme de trading actif ni de pivot cognitif/LLM.

---

## Composants Réutilisables et Architecture

Bien que la recherche d'alpha n'ait pas produit de signal exploitable en production, le projet laisse un ensemble de briques architecturales et logicielles réutilisables :

1. **Architecture Clean / Domain-Driven Design (DDD)** :
   - Couche Domaine (`src/aegis_trade/domain/`) isolée de toute dépendance logicielle externe.
   - Entités métiers immuables (`Capital`, `Position`, `TradeRecord`, `RiskGate`).
2. **Pipelines de Données et Ingestion Multi-Sources** :
   - Pipeline d'ingestion paginé Deriv WebSocket/REST (`DerivHistoricalData`).
   - Audit et alignement causal strict du jeu de données XAUUSD Dukascopy (11.6 ans d'historique D1/H4).
   - Module de filtrage des données de positionnement CFTC COT sur le code exact COMEX Gold (`088691`).
3. **Multi-Agent Council avec Veto de Liquidité/Exécution** :
   - Moteur d'agrégation de votes multi-agents (`src/aegis_trade/application/council/orchestrator.py`).
   - Mécanisme de veto strict déclenché par une confiance >= 0.8 sur l'agent de liquidité ou d'exécution, forçant l'exposition à zéro.
4. **Discipline Méthodologique et Outillage Quantitatif** :
   - Moteur de calcul du péage d'exécution et des fenêtres de tradabilité (`src/aegis_trade/domain/tradability.py`).
   - Registre d'ADRs (Architecture Decision Records 0001 à 0031) documentant chaque décision et rejet avec preuves reproductibles.

---

## Installation et Exécution des Tests

### Prérequis

- Python 3.11
- `uv` (recommandé) ou `venv` standard

### Procédure d'installation

```bash
git clone https://github.com/Kreesten-hsh/Aegis_Quant_OS.git
cd Aegis_Quant_OS
uv venv
source .venv/bin/activate
uv pip install -e . pytest
```

### Exécution des tests

```bash
pytest -v
```

---

## Structure du Dépôt et Registre ADR

- `src/aegis_trade/` : Code source principal (Clean Architecture : `domain/`, `application/`, `infrastructure/`).
- `docs/ADR/` : Registre complet des 31 décisions d'architecture et de recherche (ADR 0001 à 0031).
- `docs/refont/BUILD_VS_REUSE.md` : Matrice d'évaluation et de réutilisation des bibliothèques open-source.
- `docs/research/` : Rapports d'audit et de recherche quantitatives.
- `scripts/` : Outillage de recherche, d'ingestion et de diagnostic.

---

## Licence

Ce projet est distribué sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.
