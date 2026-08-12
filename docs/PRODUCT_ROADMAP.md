# Product Roadmap — Aegis Quant OS

> [!NOTE]
> **STATUT : PROJET CLOS ET ARCHIVÉ (AOUT 2026)**
> La recherche quantitative du projet est officiellement conclue (216 hypothèses testées, 0 validées en production). Ce document de feuille de route est désormais archivé à titre d'historique de planification.

La feuille de route définissait les jalons de développement d'Aegis Quant OS. Suite à l'audit de maturité produit, la roadmap avait été restructurée pour se concentrer sur la création d'un système de trading quantitatif personnel exploitable, en écartant toute logique commerciale ou SaaS.

---

## Sprint 1 : Validation Framework (Mission VA-01) [TERMINÉ]
### Objectif
Construire un laboratoire de validation quantitatif (Walk-Forward, Hold-Out, Monte Carlo, Benchmark) pour tester automatiquement la robustesse d'une stratégie avant toute intégration ML.
### Critères de réussite
- Production d'un rapport de validation JSON générant un "Strategy Score".

---

## Sprint 2 : Intégration Qlib (Mission QL-01) [TERMINÉ]
### Objectif
Brancher Microsoft Qlib comme moteur d'accélération pour la recherche de signaux et les backtests vectorisés.
### Critères de réussite
- Les modèles ML de Qlib peuvent être entraînés sur les features générées par Aegis.

---

## Sprint 3 : Moteur d'Exécution (Mission EX-01 - Paper Trading) [TERMINÉ]
### Objectif
Remplacer le broker simulé par une connexion à un environnement de Paper Trading réel via vn.py ou MetaTrader 5 (MT5).
### Critères de réussite
- Les ordres générés en local sont exécutés et réconciliés sur un compte de démonstration.

---

## Sprint 4 : Validation Macro, Audit Council & Pivot Fréquence H4/D1 [CLÔTURÉ ET RÉFUTÉ — ADR 0025 à 0031]

### Objectif
Évaluer la puissance prédictive des features macroéconomiques (DFII10, DXY, COT COMEX) et l'efficacité du Multi-Agent Council.

### Trajectoire exécutée
```
GOLD-01 (M1 Tech: REJETÉ) -> GOLD-MACRO (DFII10 + OpenBB FRED: REJETÉ) -> Audit Council (8 agents: REJETÉ) -> Pivot H4/D1 & Crypto (REJETÉ)
```

### Bilan des Jalons
1. **GOLD-MACRO (Clôturé - ADR 0027)** : Ingestion & alignement temporel des séries macro FRED. Évaluation d'alpha réfutée.
2. **AUDIT COUNCIL (Clôturé - ADR 0028)** : Audit complet du Council à 8 agents. Réfuté sur M1 (P&L net de -1.70 bps par trade face au coût de 1.859 bps).
3. **PIVOT FRÉQUENCE H4/D1 & CRYPTO (Clôturé - ADR 0029, 0030, 0031)** : Évaluation économétrique et ML cross-sectional. Réfuté (cointégration spurie, coûts d'allers-retours non amortis).

---

## Sprint 5 : Centre de Contrôle (Dashboard) & Live Trading [NON RETENUS]

### Statut
Non implémentés. En l'absence de signal d'alpha validé au jalon AI-07b (0/216 hypothèses validées), l'interface de contrôle temps réel et l'engagement de capital réel ont été annulés conformément à la discipline de gouvernance du projet.
