# System Architecture – Nexus Intelligence Platform

Ce document décrit l’architecture cible de la plateforme.

## Vue d’ensemble

La plateforme est une application SaaS B2B composée de :

- **apps/api** : API backend (FastAPI) exposant les services métier (cases, entités, graphes, rapports).
- **apps/web** : frontend Next.js pour les analystes et décideurs.
- **packages/core** : modèles métiers partagés (Pydantic), clients internes, constantes.
- **packages/ai-agents** : orchestration des agents IA (Extraction, Vérité, Analyste, Rédacteur, Juge).
- **infra** : scripts de conteneurisation (Docker) et de déploiement (Kubernetes).

## Stack technique (cible)

- Backend : Python 3, FastAPI
- Frontend : Next.js (TypeScript, React)
- Base relationnelle : PostgreSQL
- Graphe : Neo4j (ou ArangoDB)
- Index texte : OpenSearch / Elasticsearch
- Vecteurs (RAG) : Qdrant ou Milvus/Qdrant + pgvector
- IA : modèles Mistral (Mistral 7B self‑host en priorité, modèles plus grands via API entreprise si besoin)

## Principes d’architecture

- **Monorepo** : un seul dépôt Git pour API, frontend, packages et infra.
- **Séparation claire** :
  - `apps/` : ce qui s’exécute (API, web).
  - `packages/` : ce qui est partagé (logique, modèles, agents).
  - `infra/` : ce qui déploie.
- **IA avec garde‑fous** :
  - agents spécialisés (Extraction, Vérité, Analyste, Rédacteur, Juge),
  - séparation stricte entre faits, hypothèses et signaux faibles,
  - traçabilité des sources et des décisions.

## Évolutions prévues (non implémentées pour le MVP)

- Connecteurs OSINT (registres, sanctions, presse, etc.).
- Intégration Neo4j pour la vue graphe.
- Intégration OpenSearch pour l’adverse media.
- Intégration d’un orchestrateur IA (Mistral, RAG, cache).
