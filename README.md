# Aegis Quant OS 🛡️📈

![Python Version](https://img.shields.io/badge/python-3.11-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Architecture](https://img.shields.io/badge/architecture-Clean%20%7C%20Hexagonal-orange)
![Status](https://img.shields.io/badge/status-pivot--v2.0--cognitive--pipeline--in--progress-orange)

**Aegis Quant OS** est un système de trading quantitatif et d'évaluation d'hypothèses d'alpha d'inspiration institutionnelle. Le système sépare rigoureusement la logique de domaine, les moteurs de risque, les garde-fous d'exécution et les adapteurs d'infrastructure (LLMs, Open-Source ML, Moteurs de Backtest, Broker Gateways).

---

## 📌 STATUT ACTUEL DE LA RECHERCHE (AOUT 2026)

> [!IMPORTANT]
> **Rigueur Scientifique & Transparence Absolue (Règle Anti-Survente)** :
> - **Hypothèses Évaluées** : **216 hypothèses** (Signaux M1/M5, Indicateurs Techniques univariés, Features Macro FRED DFII10/DXY, Microstructure Spike, Positionnement CFTC COT 088691, Trend-Following Crypto 24/7, ML Cross-Sectional Ranking).
> - **Signaux Validés en Production** : **0 / 216 (0.0%)**.
> - Tous les signaux directionnels univariés usuels sur séries de prix individuelles ont été formellement **réfutés** après déduction des péages d'exécution et correction des tests multiples FDR / Bonferroni (ADR 0025 à ADR 0031).

---

## 🏛️ CE QUI EST OPÉRATIONNEL ET ROBUSTE

1. **Architecture Hexagonale & DDD** : Couche Domaine isolée de toute dépendance tierce, garantissant l'absence de fuite d'infrastructure.
2. **Garde-Fous d'Exécution & Péage (Execution Budget Gates)** :
   - Mesure exacte du péage d'exécution ($1.859\text{ bps}$ sur Deriv / Or et $10.0\text{ bps}$ sur Crypto Spot).
   - Validation stricte par horizon $H$ (ADR 0021).
3. **Multi-Agent Council avec Veto de Liquidité/Exécution (`orchestrator.py`)** :
   - Moteur d'agrégation de votes multi-agents avec pondération dynamique.
   - Veto impératif émis dès que `LiquidityAgent` ou `ExecutionAgent` vote `WAIT` avec une confiance $\ge 0.8$, réinitialisant la taille de position à zéro.
4. **Integration des Frameworks Open-Source (Matrice `BUILD_VS_REUSE.md`)** :
   - Connecteurs pour `VectorBT`, `Microsoft Qlib`, `Freqtrade` et `pandas-ta-classic`.
5. **Jeux de Données Validés & Audités** :
   - `XAUUSD` Dukascopy 11.6 ans (D1 et H4) à alignement causal strict.
   - Données CFTC COT filtrées sur le code exact **`088691`** (Gold COMEX 100 oz Standard).

---

## 🛠️ TECHNOLOGIES ET FRAMEWORKS

- **Langage** : Python 3.11
- **Architecture** : Clean Architecture / Ports & Adapters
- **Frameworks Quantitatifs** : `vectorbt`, `pandas-ta-classic`, `scipy`, `numpy`, `pandas`, `lightgbm`
- **Infrastructure LLM** : Ollama (Adapteur local déterministe avec cache SHA-256 — Modèle primaire: `llama3.1:8b` / `Q4_K_M`, fallback local: `llama3.2:1b`)

---

## ⚙️ INSTALLATION ET EXÉCUTION DES TESTS

1. **Cloner le dépôt** :
   ```bash
   git clone https://github.com/votre-username/AegisQuantOS.git
   cd AegisQuantOS
   ```

2. **Créer et activer un environnement virtuel** :
   ```bash
   python3.11 -m venv .venv
   source .venv/bin/activate
   ```

3. **Installer les dépendances** :
   ```bash
   pip install -e .
   ```

4. **Exécuter la suite complète de tests** :
   ```bash
   pytest -v
   ```

---

## 📂 STRUCTURE DU DÉPÔT ET DECISIONS (ADRs)

- **`src/aegis_trade/`** : Code source (Clean Architecture : `domain/`, `application/`, `infrastructure/`).
- **`docs/ADR/`** : Registre complet des décisions d'architecture et de recherche (0001 à 0032).
- **`docs/refont/BUILD_VS_REUSE.md`** : Matrice de réutilisation des frameworks Open-Source.
- **`docs/research/`** : Rapports de recherche quantitatifs probants.
- **`docs/archive/`** : Archives historiques et métadonnées brutes.

---

## 🎯 PROCHAINE ÉTAPES DE RECHERCHE — PIVOT V2.0

À la suite de la réfutation empirique des 216 hypothèses statistiques univariées (ADR 0017–0031), Aegis Quant OS pivote vers un **pipeline cognitif sémantique v2.0** piloté par un LLM local autonome sans humain dans la boucle décisionnelle. Le système associe un moteur de raisonnement sémantique prompt-engineeré, une mémoire d'expérience vectorielle (RAG), et le veto déterministe impératif du `MultiAgentCouncil`. Pour plus de détails sur l'audit de réutilisation et l'architecture v2.0, consulter l'[ADR 0032 — Pivot vers le Pipeline Cognitif Sémantique v2.0](file:///mnt/WindowsData/Aegis%20Quant%20OS/docs/ADR/0032-pivot-cognitive-pipeline-and-reuse-audit.md).
