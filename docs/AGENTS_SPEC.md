# Agents IA – Spécifications (V1)

Ce document décrit les deux premiers agents IA de la plateforme : l’agent d’Extraction et l’agent de Vérité (Corroboration).

## 1. Agent Extraction

### Rôle

Extraire, à partir d’un document (texte brut ou structuré), les entités et événements pertinents pour l’intelligence économique et la due diligence.

### Entrées

- `document_id` : identifiant interne du document.
- `text` : contenu textuel (après OCR si besoin).
- `metadata` :
  - `source_type` (ex: registry, news, court_record, internal),
  - `source_name`,
  - `published_at`,
  - `language`, etc.

### Sorties (JSON)

```json
{
  "document_id": "DOC-123",
  "entities": [
    {
      "id": "E1",
      "type": "company",
      "name": "Example Corp",
      "jurisdiction": "FR",
      "confidence": 0.94
    },
    {
      "id": "E2",
      "type": "person",
      "name": "Jane Doe",
      "role": "Director",
      "confidence": 0.91
    }
  ],
  "events": [
    {
      "id": "EV1",
      "type": "lawsuit",
      "date": "2024-10-12",
      "parties": ["E1", "E2"],
      "amount": null,
      "confidence": 0.88
    }
  ],
  "uncertain_spans": [
    {
      "text": "un montage financier complexe",
      "reason": "ambigu",
      "offset_start": 120,
      "offset_end": 150
    }
  ]
}
```

### Contraintes

- Ne pas inventer de faits.
- Marquer comme `uncertain` ce qui n’est pas suffisamment clair.
- Respecter la langue d’origine dans les noms propres.

---

## 2. Agent Vérité / Corroboration

### Rôle

Évaluer la fiabilité des faits extraits en fonction des sources disponibles, détecter les contradictions et les reprises circulaires (plusieurs articles reprenant la même info).

### Entrées

- Liste de faits (provenant de l’agent Extraction et/ou d’autres sources structurées).
- Pour chaque fait :
  - `fact_id`
  - `fact_type` (ex: ownership, lawsuit, sanction, role_change)
  - `value` (structure selon le type)
  - `sources` : liste de sources avec:
    - `source_id`
    - `source_type` (official, professional, open_web)
    - `source_name`
    - `published_at`
    - `independence_hint` (ex: same_wire_service, independent)
    - `reliability_hint` (historique, réputation)

### Sorties (JSON)

```json
{
  "facts": [
    {
      "fact_id": "F1",
      "confidence": 0.92,
      "supporting_sources": ["S1", "S3"],
      "contradicting_sources": [],
      "duplicate_groups": [
        ["S2", "S4"]
      ],
      "labels": ["well_corroborated", "official_source_present"]
    },
    {
      "fact_id": "F2",
      "confidence": 0.55,
      "supporting_sources": ["S5"],
      "contradicting_sources": ["S6"],
      "duplicate_groups": [],
      "labels": ["needs_review", "conflicting_information"]
    }
  ]
}
```

### Contraintes

- Ne jamais générer de nouveaux faits.
- Se limiter à :
  - scorer la fiabilité,
  - signaler les contradictions,
  - regrouper les sources qui répètent la même information.
- En cas de doute important, marquer le fait `needs_review` pour intervention humaine.

---

Ces spécifications seront utilisées plus tard pour implémenter `packages/ai-agents` et les prompts des modèles Mistral.
