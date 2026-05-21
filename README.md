# Nexus Intelligence Platform

Plateforme d’intelligence économique et de due diligence augmentée, combinant OSINT, graphe de connaissances et agents IA spécialisés.  
Objectif : aider les équipes M&A, risk, compliance et intelligence économique à analyser plus vite, plus profond, avec des rapports auditables.

## Stack (cible)

- Backend : Python 3 + FastAPI
- Frontend : Next.js (TypeScript)
- Base relationnelle : PostgreSQL
- Plus tard : Neo4j (graphe), OpenSearch (texte), Qdrant/milvus (vecteurs), modèles LLM auto‑hébergés (Mistral 7B, etc.)

## Structure du dépôt

```text
apps/
  api/          # API FastAPI (services métier)
  web/          # Frontend Next.js (interface analyste)
packages/
  core/         # Modèles métiers, schémas, clients communs
  ai-agents/    # Orchestration des agents IA (Extraction, Vérité, Analyste, etc.)
infra/
  docker/       # Dockerfiles
  k8s/          # Manifests / charts pour le déploiement
docs/
  SYSTEM_ARCHITECTURE.md
  AGENTS_SPEC.md
```

## Démarrage (MVP)

> À remplir quand l’API et le frontend seront initialisés.

Exemple attendu à terme :

```bash
# API
cd apps/api
uvicorn app.main:app --reload

# Frontend
cd apps/web
npm install
npm run dev
```

## Sécurité & bonnes pratiques Git

- **Ne jamais committer de secrets** : clés API, tokens, mots de passe, certificats, dumps de base.
- Les fichiers suivants sont ignorés dans `.gitignore` :

```gitignore
.env
*.env
.env.*
*.pem
*.key
```

- Toute configuration sensible doit rester en dehors du dépôt (fichiers `.env` locaux non suivis par Git, gestion de secrets côté infra).
