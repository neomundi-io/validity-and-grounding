# validity-and-grounding

Dépôt public de recherche consacré aux méthodes expérimentales d’évaluation de la validité factuelle et du grounding des réponses générées par des IA.

Ce dépôt s’inscrit dans l’initiative NeoMundi Research.

Il se concentre sur une question précise :

> Comment évaluer la validité factuelle, le risque sémantique et l’ancrage documentaire d’une réponse générée par IA, indépendamment de sa stabilité runtime ?

## 1. Objectif

Les systèmes d’IA modernes peuvent produire des réponses fluides, cohérentes et stables, tout en étant factuellement fausses ou insuffisamment fondées.

Ce dépôt explore une séparation méthodologique entre :

- **la stabilité runtime**, c’est-à-dire le comportement de la génération pendant sa production ;
- **la validité factuelle**, c’est-à-dire la probabilité que les affirmations générées soient correctes ;
- **le grounding**, c’est-à-dire le fait qu’une affirmation soit soutenue par un corpus documentaire fourni ;
- **le risque d’hallucination**, c’est-à-dire le risque que la réponse contienne des affirmations non fondées, fabriquées ou sémantiquement instables.

L’objectif n’est pas de produire un score unique opaque.

L’objectif est de documenter des signaux distincts, interprétables, auditables et combinables selon le contexte de gouvernance.

## 2. Pourquoi séparer validité et stabilité

Une réponse peut être stable mais fausse.

Une réponse peut aussi être partiellement instable tout en contenant des éléments factuels utiles.

Pour cette raison, NeoMundi distingue deux couches différentes :

```txt
G = signal de stabilité runtime
V = signal de validité et de grounding
```

Cette séparation évite de confondre stabilité comportementale et exactitude factuelle.

En pratique :

- **G** aide à déterminer si la génération reste stable pendant son exécution.
- **V** aide à déterminer si le contenu généré est factuellement soutenu.
- Un système de gouvernance peut utiliser les deux signaux, mais ils ne doivent pas être fusionnés trop tôt dans un score unique non explicable.

## 3. GPT-4o comme juge

Dans le design expérimental actuel, GPT-4o peut être utilisé comme juge externe pour évaluer les réponses générées.

Son rôle est de détecter :

- les hallucinations factuelles potentielles ;
- les éléments sémantiquement instables ;
- les affirmations suspectes ou non soutenues ;
- les phrases exactes nécessitant une revue humaine.

Une sortie publique simplifiée peut ressembler à ceci :

```json
{
  "factual_risk_score": 0.72,
  "semantic_instability_score": 0.15,
  "suspect_phrases": [
    {
      "quote": "Sous-chaîne exacte extraite de la réponse IA",
      "reason": "Explication courte de la raison pour laquelle cette phrase est suspecte",
      "severity": "high",
      "type": "factual"
    }
  ]
}
```

Le principe important est que les phrases suspectes doivent être extraites comme des sous-chaînes exactes de la réponse originale, et non réécrites ou paraphrasées.

Cela rend le signal inspectable par un humain.

## 4. Cohere comme module de grounding

Cohere peut être utilisé comme module optionnel de grounding.

Son rôle est différent de celui du juge GPT-4o.

GPT-4o peut évaluer une réponse de manière générale, sur le plan sémantique et factuel.

Le module de grounding permet de vérifier si les affirmations de la réponse sont soutenues par un corpus documentaire spécifique fourni par l’utilisateur ou le client.

Ce mécanisme est particulièrement utile dans des contextes comme :

- l’IA juridique ;
- l’automatisation documentaire ;
- les workflows de conformité ;
- les bases de connaissance d’entreprise ;
- la documentation médicale ou technique ;
- les assistants internes fondés sur des politiques ou procédures.

Une sortie publique simplifiée peut ressembler à ceci :

```json
{
  "grounding_score": 0.84,
  "is_grounded": true,
  "citations": [
    {
      "document_title": "Document d’exemple",
      "text": "Passage pertinent soutenant l’affirmation"
    }
  ],
  "ungrounded_claims": [
    "Affirmation non retrouvée dans le corpus fourni"
  ]
}
```

Le module de grounding est optionnel.

Il n’est pertinent que lorsqu’un corpus de référence est disponible.

## 5. Architecture conceptuelle

La couche de validité et de grounding peut être comprise comme une couche d’évaluation parallèle.

```txt
Prompt utilisateur
    ↓
Réponse LLM
    ↓
Évaluation de validité et de grounding
    ├── Juge GPT-4o
    │   ├── risque factuel
    │   ├── instabilité sémantique
    │   └── phrases suspectes
    │
    └── Module de grounding Cohere
        ├── score de grounding
        ├── citations de soutien
        └── affirmations non fondées
```

Ce dépôt ne définit pas le moteur complet de gouvernance runtime de NeoMundi.

Il documente uniquement la couche publique de recherche liée à la validité et au grounding.

## 6. Exemple d’interprétation

Une réponse générée peut recevoir le profil suivant :

```txt
Stabilité runtime : stable
Validité factuelle : faible
Grounding : insuffisant
Risque d’hallucination : élevé
```

Cela signifie que la réponse peut sembler fluide et cohérente, tout en nécessitant une revue parce que ses affirmations ne sont pas suffisamment soutenues.

Cette distinction est centrale pour une gouvernance responsable des IA.

## 7. Schéma public indicatif

Un profil public de validité peut inclure :

```json
{
  "validity": {
    "factual_risk_score": 0.72,
    "semantic_instability_score": 0.15,
    "suspect_phrases": []
  },
  "grounding": {
    "enabled": true,
    "grounding_score": 0.84,
    "is_grounded": true,
    "citations": [],
    "ungrounded_claims": []
  },
  "interpretation": {
    "validity_status": "review_required",
    "reason": "Certaines affirmations nécessitent une vérification contre des sources fiables."
  }
}
```

Ce schéma est illustratif.

Il ne constitue pas un contrat de production.

## 8. Ce que ce dépôt ne divulgue pas

Ce dépôt ne divulgue volontairement pas :

- les formules propriétaires de gouvernance ;
- les prompts internes utilisés en production ;
- les calibrations internes de seuils ;
- les coefficients de scoring ;
- la logique complète de décision runtime ;
- les détails d’implémentation client ;
- les jeux de données privés ;
- les clés API ou éléments d’infrastructure de production.

L’objectif est de documenter une direction de recherche et d’ouvrir une discussion méthodologique, pas d’exposer le moteur complet de NeoMundi.

## 9. Statut de recherche

Ce dépôt est expérimental.

Les méthodes documentées ici visent à soutenir :

- la revue indépendante ;
- la discussion méthodologique ;
- des exemples reproductibles ;
- la conception de futurs benchmarks ;
- des architectures de gouvernance IA plus sûres.

Elles ne doivent pas être interprétées comme un standard scientifique final.

## 10. Relation avec NeoMundi

NeoMundi développe des signaux de gouvernance runtime pour les systèmes d’IA.

Ce dépôt se concentre uniquement sur la couche de validité et de grounding.

Il complète l’approche plus large de NeoMundi, selon laquelle la gouvernance de l’IA nécessite plusieurs signaux distincts plutôt qu’un score unique opaque.

Principe central :

> La stabilité n’est pas la vérité.  
> Le grounding n’est pas la gouvernance.  
> Les systèmes d’IA responsables ont besoin de signaux distincts, interprétables et auditables.

## 11. Licence

Licence à définir.

Tant qu’aucune licence explicite n’est ajoutée, tous droits réservés par NeoMundi Research.
