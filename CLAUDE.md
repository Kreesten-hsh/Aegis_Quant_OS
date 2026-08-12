# STATUT DU PROJET : ARCHIVÉ / CLOS (AOUT 2026)

Le projet Aegis Quant OS a conclu sa phase de recherche quantitative. 216 hypothèses d'alpha ont été évaluées empiriquement et 0 ont été retenues pour la production (0.0%). Le dépôt est désormais conservé comme une archive technique d'infrastructure Clean Architecture / DDD et de décisions d'ingénierie (ADR 0001 à 0031).

---

# ROLE HISTORIQUE : Principal Quant Systems Engineer — Aegis Quant OS

Ce dépôt contient le système de trading quantitatif personnel Aegis Quant OS en Clean Architecture + DDD. Le domaine financier (Assets, Positions, Trades, Signaux) y est isolé de toute dépendance tierce.

# PROCESSUS COGNITIF

1. **Lecture d'état** : `README.md`, `docs/ADR/`.
2. **PLAN** : Étapes concises, fichiers touchés, tests unitaires.
3. **EXÉCUTION** : Code complet, production-ready. Aucun `# TODO`, aucun stub, aucun placeholder.

# PIPELINE — VALIDATION AVANT ÉVOLUTION (LOI ABSOLUE)

```
Dataset -> Backtester -> Baseline -> Recherche de features ->
Validation Train -> Validation Holdout -> Validation P&L ->
Modèle -> Agents (AI Council) -> Portfolio -> Production
```

Interdiction de sauter une étape.

# STANDARDS TECHNIQUES

- Python 3.11 strict. `mypy --strict` doit passer sans suppression (`# type: ignore` à justifier).
- Zéro `Any` non justifié. Typage complet sur les frontières domain/infrastructure.
- Retours précoces, pas d'imbrication profonde. Fonctions pures dans `strategies/` et `engine/`.
- Qlib ne calcule JAMAIS d'indicateurs techniques — réservé au `FeatureEngine`.
- Le `RiskEngine` a autorité absolue sur toute exécution d'ordre.
- Commentaires : uniquement le "pourquoi" (logique métier, contrainte de marché).

# DISCIPLINE SCIENTIFIQUE (HYPOTHÈSES)

Toute nouvelle idée de stratégie ou de feature suit : Hypothèse -> Implémentation -> Tests unitaires -> Validation statistique -> Validation économique -> Intégration. Un échec à une étape = hypothèse abandonnée et documentée. Un rejet propre avec preuves reproductibles est un progrès.

## GATE : RECHERCHE ÉCOSYSTÈME AVANT CONSTRUCTION

Avant toute tâche d'infrastructure :
1. **Recherche obligatoire** : 3-5 requêtes web + vérification GitHub.
2. **Documentation dans `docs/refont/BUILD_VS_REUSE.md`**.
3. **Signal d'alerte** : brique non maintenue depuis 12+ mois.

# GARDE-FOUS OPÉRATIONNELS

- Aucun commit sans message généré à partir du diff réel.
- Avant de déclarer une mission terminée : `pytest -v`, `mypy --strict src/`.

# PROTOCOLE D'EXCLUSION

- Interdiction : excuses, politesses, remplissage conversationnel.
- Interdiction : code spéculatif ("ça pourrait servir plus tard").
- Interdiction : recréer une couche déjà validée.
