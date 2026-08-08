# Vision : Aegis Quant OS

## Objectif Fondamental
Aegis Quant OS est un **Système d'Exploitation de Trading Quantitatif Personnel**, piloté par l'Intelligence Artificielle.
Son but unique est de fournir à un opérateur unique (vous) un contrôle absolu, automatisé et transparent sur l'ingestion de données, la prise de décision (via des agents IA) et l'exécution d'ordres sur les marchés financiers.

## Anti-SaaS (Ce que le système N'EST PAS)
- **Pas de Multi-Tenancy** : Le système ne gère qu'un seul portefeuille global.
- **Pas de Monétisation** : Aucune logique d'abonnement, de licence ou de paiement n'est incluse.
- **Pas de Portail Public** : L'interface est un Dashboard local (Local-First) sécurisé, tournant exclusivement sur l'infrastructure privée de l'opérateur.

## Principes Architecturaux
1. **Souveraineté des Données** : Les données et stratégies restent locales et privées.
2. **Modularité Absolue** : L'adoption de la Clean Architecture permet de changer n'importe quel fournisseur (Données, LLM, Broker) sans impacter le cœur algorithmique.
3. **Risk First** : La gestion du risque prévaut sur toute décision d'investissement. Le système peut tout couper ("Kill Switch") à la moindre anomalie critique.

## Addendum v2.0 — Autonomie et contrôle
Le "contrôle absolu" de l'opérateur (principe 1) s'exerce au niveau du gate déterministe et du
kill switch — pas d'une approbation trade par trade. Voir ADR 0032. Aucun humain dans la boucle
décisionnelle ; le remplacement de l'humain est le coupe-circuit de risque (Module 1), non négociable
et non ajustable par le système lui-même.

