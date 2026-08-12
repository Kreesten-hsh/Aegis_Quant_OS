# Backlog Officiel — Aegis Quant OS

> [!NOTE]
> **STATUT : PROJET CLOS ET ARCHIVÉ (AOUT 2026)**
> La recherche d'alpha est terminée (216 hypothèses évaluées, 0 validées). Ce document de backlog est archivé comme registre historique des travaux accomplis et réfutés.

Ce document liste les missions structurées de l'OS de trading et retrace le déroulement du pipeline quantitatif.

---

## TRAJECTOIRE FINALE — Bilan au 2026-08-12

```
GOLD-01 (M1 Tech: REJETÉ - ADR 0025) -> GOLD-MACRO (FRED DFII10: REJETÉ - ADR 0027) -> Audit Council (8 agents: REJETÉ - ADR 0028) -> Pivot H4/D1 & Crypto (REJETÉ - ADR 0029, 0030, 0031)
```

1. **GOLD-01 (Clôturé & Rejeté - ADR 0025)** : Coût A/R mesuré à 1.859 bps. Gate économique `domain/tradability` validé dès H5 (>75.6% tradable), mais 0/25 indicateurs techniques significatifs de H5 à H120. Réfute définitivement l'hypothèse des indicateurs techniques simples sur M1.
2. **GOLD-MACRO (Clôturé & Rejeté - ADR 0027)** : Ingestion et alignement des Taux Réels 10 ans FRED (`DFII10`) et DXY sur 75k barres M1 Gold. 0/6 features macro significatives de H5 à H240 ($|t| \le 1.93$).
3. **Audit du Council à 8 agents (Clôturé & Rejeté - ADR 0028)** : Audit quantitatif du Council déterministe avec veto strict (Run 1 Purifié: 36.71% d'exposition, net -1.69 bps ; Run 2 Sparse: 51.95% d'exposition, net -1.81 bps). Le mouvement brut moyen capté (+0.16 bps) est 11x inférieur aux allers-retours (1.859 bps). Réfuté sur M1.
4. **Pivot Fréquence H4/D1 & Trend-Following Crypto (Clôturé & Rejeté - ADR 0029, 0030, 0031)** : Évaluation de la cointégration DXY/Gold et des modèles ML LightGBM cross-sectionnels. Réfutés post-coûts d'exécution et correction FDR/Bonferroni.

---

## Phase 1 : Cœur du Moteur de Simulation

### BT-01 : Modular Backtesting Engine (Backtest Core)
- **Objectif** : Implémenter le moteur de simulation (Boucle séquentielle, Simulated Broker, Performance Metrics).
- **Statut** : COMPLETED

### ST-01 : Strategy Framework
- **Objectif** : Créer l'architecture de stratégies hiérarchiques (Core, Composites).
- **Statut** : COMPLETED

### PM-01 : Portfolio Management
- **Objectif** : Implémenter le Portfolio Manager (Sizing, Rééquilibrage via `GlobalRiskAdapter`).
- **Statut** : COMPLETED

### RM-01 : Risk Management
- **Objectif** : Fonctionnalité couverte par la réunification PM-01 (`GlobalRiskAdapter`).
- **Statut** : COMPLETED

---

## Phase 2 : Validation Scientifique & Machine Learning

### VA-01 : Institutional Validation Framework
- **Objectif** : Construire un laboratoire de validation (Walk-Forward, Hold-Out, Monte Carlo, Benchmark) pour tester la robustesse économique.
- **Statut** : COMPLETED

### QL-01 : Qlib Adapter
- **Objectif** : Intégrer Microsoft Qlib pour un backtesting factoriel.
- **Statut** : COMPLETED

### ML-01 : Machine Learning / AI Decision Engine
- **Objectif** : Ajouter les modèles ML (LightGBM, PyTorch) et réintégrer l'AI Council.
- **Statut** : COMPLETED

### VA-02 : Barème monotone et seuils dérivés du coût
- **Objectif** : Rendre le `ScoringEngine` strictly monotone en PnL net réel.
- **Statut** : COMPLETED (ADR 0017, ADR 0018)

### SIG-01 : Horizon 1 barre sur Crash 1000 — REJETÉ
- **Objectif** : Établir si un edge net de frais existe sur `forward_return_1`.
- **Statut** : REJETÉ (ADR 0019)

### DATA-01 : Historique Crash 1000 suffisant pour valider un horizon long
- **Objectif** : Ingérer un historique paginé M1 Deriv (75 000 barres, 52 jours).
- **Statut** : COMPLETED (ADR 0022)

### COST-01 / COST-02 : Mesurer le coût de transaction Deriv réel
- **Objectif** : Mesurer le coût aller-retour réel (0.745 bps sur Crash 1000, 1.063 bps sur Boom 1000).
- **Statut** : COMPLETED (ADR 0021, script `scripts/measure_deriv_live_round_trip.py`)

### SIG-02 : Redéfinition de l'horizon cible
- **Objectif** : Mesurer la faisabilité économique sur horizons 5M et 10M.
- **Statut** : REJETÉ (ADR 0024 - 0/25 features significatives)

### FE-01 : Recherche de features obligatoire
- **Objectif** : Mesurer l'Information Coefficient (IC Spearman) et la significativité ($|t| > 2.0$).
- **Statut** : COMPLETED (script `scripts/run_feature_research.py`)

### GOLD-01 : Troisième actif réel (XAUUSD)
- **Objectif** : Mesurer Gold de bout en bout (coût A/R 1.859 bps).
- **Statut** : REJETÉ (ADR 0025)

---

## Phase 3 : Production & Temps Réel

### EX-01 : Execution Engine (Event-Driven)
- **Objectif** : Moteur événementiel (EventBus) pour router les ordres.
- **Statut** : COMPLETED

### LIVE-01 / LIVE-02 : Dashboards & Live Trading
- **Statut** : ANNULÉS (Jalon AI-07b non atteint : 0/216 hypothèses validées).
