# Aegis Quant OS

![Python Version](https://img.shields.io/badge/python-3.11-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Architecture](https://img.shields.io/badge/architecture-Clean%20%7C%20Hexagonal-orange)
![Status](https://img.shields.io/badge/research-concluded%20(216%20hypotheses%20tested)-red)

Aegis Quant OS est une plateforme de recherche quantitative et d'évaluation d'hypothèses d'alpha en Clean Architecture / Domain-Driven Design (DDD). Le système isole la logique du domaine financier (actifs, positions, signaux, gestion des risques) des adaptateurs d'infrastructure (moteurs de backtest, gateways de données, modélisation ML, agrégation multi-agents).

---

## Contexte du Projet

Le projet Aegis Quant OS a été conçu pour appliquer une méthodologie scientifique rigoureuse à l'exploration d'alphas systématiques sur plusieurs classes d'actifs (indices synthétiques, métaux précieux, crypto-monnaies). L'objectif initial était de construire un pipeline de décision automatisé appuyé sur un filtrage strict des coûts d'exécution, une correction statistique pour tests multiples et une architecture modulaire réutilisable.

---

## Résultat de la Recherche Quantitative (Bilan Final)

Conformément au principe de transparence et de rigueur scientifique (règle anti-survente), le projet a procédé à l'évaluation systématique de **216 hypothèses d'alpha**.

### Tableau Récapitulatif des 216 Hypothèses Évaluées

| Famille d'hypothèses | Périmètre & Instruments | Nb Hypothèses | Statut | Référence ADR |
|---|---|---|---|---|
| Signaux M1/M5 Univariés | Synthétiques Deriv (Crash 1000, Boom 1000) | 180 | 0 validées (Réfutées post-coût d'exécution) | ADR 0019, 0020, 0024 |
| Indicateurs Techniques Or | XAUUSD M1 (GOLD-01) | 12 | 0 validées (IC non significatif hors-échantillon) | ADR 0025 |
| Features Macroéconomiques | XAUUSD M1 (FRED DFII10, DXY) | 12 | 0 validées (|t-stat| <= 1.93) | ADR 0026, 0027, 0028 |
| Cointégration & Positionnement H4/D1 | XAUUSD H4/D1, COT COMEX 088691, DXY | 8 | 0 validées (Cointégration spurie, P&L net négatif) | ADR 0029, 0030 |
| Trend-Following Crypto & ML Ranking | BTC/USD, ETH/USD 24/7, LightGBM Cross-Sectional | 4 | 0 validées (Sur-ajustement & friction 10 bps) | ADR 0031 |
| **Total** | **4 familles de méthodes, 3 classes d'actifs** | **216** | **0 / 216 (0.0% validées en production)** | **ADR 0017 à 0031** |

Tous les signaux directionnels univariés usuels sur séries temporelles de prix et de macroéconomie ont été formellement réfutés après déduction des péages d'exécution réels (1.859 bps A/R sur Deriv/Or et 10.0 bps sur Crypto Spot) et application des corrections FDR (False Discovery Rate) et Bonferroni.

---

## Statut de Clôture du Projet

Le projet de recherche Aegis Quant OS est **officiellement clos**.

La réfutation empirique des 216 hypothèses constitue un résultat scientifique valide et documenté : l'absence d'edge exploitable net de frais d'exécution sur les horizons et univers étudiés démontre l'inaptitude des signaux testés à générer du P&L réel. Aucune stratégie n'a atteint le jalon de validation AI-07b (engagement de capital réel).

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
git clone https://github.com/votre-username/AegisQuantOS.git
cd AegisQuantOS
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
