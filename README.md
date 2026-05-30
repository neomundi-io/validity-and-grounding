> 🇬🇧 **English version:** [README_EN.md](./README_EN.md)

# Validité et grounding

**Évaluer la validité factuelle d’une réponse générée sans la confondre avec sa stabilité en temps réel.**

Ce dépôt public de recherche est consacré aux méthodes expérimentales d’évaluation de la validité factuelle, du risque sémantique et du grounding des réponses générées par des systèmes d’IA.

Il s’inscrit dans l’initiative **NeoMundi Research**.

La question centrale est simple :

> **Comment évaluer la validité factuelle, le risque sémantique et l’ancrage documentaire d’une réponse générée par IA, indépendamment de sa stabilité pendant l’exécution ?**

---

## 1. Objectif

Les systèmes d’IA générative peuvent produire des réponses fluides, cohérentes et stables tout en étant factuellement fausses ou insuffisamment fondées.

NeoMundi distingue donc plusieurs signaux complémentaires :

* **la stabilité de génération en temps réel**, c’est-à-dire le comportement du système pendant la production de la réponse ;
* **la validité factuelle**, c’est-à-dire la probabilité que les affirmations générées soient correctes ;
* **le grounding**, c’est-à-dire le fait qu’une affirmation soit soutenue par un corpus documentaire fourni ;
* **le risque d’hallucination**, c’est-à-dire le risque que la réponse contienne des affirmations non fondées, fabriquées ou sémantiquement fragiles.

> **Stable ne veut pas dire vrai.**

L’objectif n’est pas de produire un score unique opaque.

L’objectif est de documenter des signaux distincts, interprétables, auditables et combinables selon le contexte de gouvernance.

---

## 2. Séparer stabilité et validité

Une réponse peut être stable mais fausse.

Une réponse peut également contenir des informations factuellement utiles tout en présentant une instabilité de génération.

NeoMundi sépare donc deux couches méthodologiques :

```txt
G = signal de stabilité de génération en temps réel
V = signal de validité factuelle et de grounding
```

Cette séparation évite de confondre stabilité comportementale et exactitude factuelle.

En pratique :

* **G** aide à déterminer si la génération reste stable pendant son exécution ;
* **V** aide à déterminer si le contenu généré est factuellement soutenu ;
* les deux signaux peuvent être combinés dans une architecture de gouvernance ;
* ils ne doivent pas être fusionnés trop tôt dans un score unique difficile à expliquer.

> **La stabilité n’est pas la vérité.
> Le grounding n’est pas la gouvernance.
> La gouvernance responsable exige des signaux distincts.**

---

## 3. Llama auto-hébergé comme juge

NeoMundi intègre un **juge Llama auto-hébergé dans un pipeline d’inférence souverain**.

Cette architecture permet d’exécuter une fonction critique de qualification sur une infrastructure maîtrisée, sans dépendre systématiquement d’un service d’inférence externe.

> **Llama auto-hébergé comme juge dans un pipeline d’inférence souverain.**

Le juge peut contribuer à identifier :

* les hallucinations factuelles potentielles ;
* les affirmations suspectes ou non soutenues ;
* les tensions sémantiques ;
* les phrases exactes nécessitant une revue humaine ;
* les éléments appelant une vérification complémentaire.

Cette évolution renforce trois exigences fondamentales :

* **confidentialité**, en réduisant l’exposition inutile des données sensibles ;
* **résilience**, en limitant la dépendance aux services externes ;
* **souveraineté opérationnelle**, en permettant l’exécution d’une fonction critique sur une infrastructure maîtrisée par NeoMundi.

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

Le principe important est que les phrases suspectes soient extraites comme des sous-chaînes exactes de la réponse originale.

Elles ne doivent pas être réécrites ou paraphrasées.

Cela rend le signal inspectable par un humain.

---

## 4. Juges externes et validation méthodologique

Des modèles externes peuvent également être utilisés pour :

* comparer les verdicts ;
* mesurer les accords et désaccords ;
* préparer des revues méthodologiques ;
* effectuer des tests croisés ;
* contribuer à une validation multi-juges.

Ces modèles externes ne constituent pas nécessairement la source unique de vérité.

Ils peuvent servir de couches complémentaires de comparaison, d’arbitrage ou de validation.

L’objectif est de réduire la dépendance à un juge unique et de rendre les zones d’incertitude plus visibles.

---

## 5. Cohere comme module optionnel de grounding

Cohere peut être utilisé comme module optionnel de grounding.

Son rôle est différent de celui du juge Llama auto-hébergé.

Le juge contribue à qualifier le risque factuel et sémantique d’une réponse.

Le module de grounding vérifie si les affirmations sont soutenues par un corpus documentaire spécifique fourni par l’utilisateur ou le client.

Ce mécanisme est particulièrement utile dans des contextes comme :

* l’IA juridique ;
* l’automatisation documentaire ;
* les workflows de conformité ;
* les bases de connaissance d’entreprise ;
* la documentation médicale ou technique ;
* les assistants internes fondés sur des politiques ou procédures.

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

---

## 6. Architecture conceptuelle

La couche de validité et de grounding peut être comprise comme une couche d’évaluation parallèle au signal de stabilité en temps réel.

```txt
Prompt utilisateur
    ↓
Réponse LLM
    ↓
Qualification de validité et de grounding
    ├── Juge Llama auto-hébergé
    │   ├── risque factuel
    │   ├── tension sémantique
    │   ├── phrases suspectes
    │   └── éléments à vérifier
    │
    ├── Comparaison multi-juges optionnelle
    │   ├── accords
    │   ├── désaccords
    │   └── arbitrage méthodologique
    │
    └── Module de grounding Cohere optionnel
        ├── score de grounding
        ├── citations de soutien
        └── affirmations non fondées
```

> **Mesurer. Qualifier. Vérifier. Interpréter. Gouverner.**

Ce dépôt ne définit pas l’intégralité du moteur de gouvernance en temps réel de NeoMundi.

Il documente la couche publique de recherche consacrée à la validité factuelle et au grounding.

---

## 7. Exemple d’interprétation

Une réponse générée peut recevoir le profil suivant :

```txt
Stabilité en temps réel : stable
Validité factuelle : faible
Grounding : insuffisant
Risque d’hallucination : élevé
```

Cela signifie que la réponse peut sembler fluide et cohérente tout en nécessitant une revue, car ses affirmations ne sont pas suffisamment soutenues.

Cette distinction est centrale pour une gouvernance responsable de l’IA.

---

## 8. Schéma public indicatif

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

---

## 9. Ce que ce dépôt ne divulgue pas

Ce dépôt ne divulgue volontairement pas :

* les formules propriétaires de gouvernance ;
* les prompts internes utilisés en production ;
* les calibrations internes de seuils ;
* les coefficients de scoring ;
* la logique complète de décision en temps réel ;
* les détails d’implémentation client ;
* les jeux de données privés ;
* les clés API ;
* les détails sensibles de l’infrastructure de production.

L’objectif est de documenter une direction de recherche et d’ouvrir une discussion méthodologique.

L’objectif n’est pas d’exposer le moteur complet de NeoMundi.

---

## 10. Statut de recherche

Ce dépôt est expérimental.

Les méthodes documentées ici visent à soutenir :

* la revue indépendante ;
* la discussion méthodologique ;
* des exemples reproductibles ;
* la conception de futurs benchmarks ;
* des architectures de gouvernance IA plus sûres ;
* des pipelines d’inférence plus souverains.

Elles ne doivent pas être interprétées comme un standard scientifique final.

---

## 11. Relation avec NeoMundi

NeoMundi développe une couche de mesure et de gouvernance en temps réel pour les systèmes d’IA.

Ce dépôt se concentre uniquement sur la couche de validité factuelle et de grounding.

Il complète une architecture plus large dans laquelle plusieurs signaux restent distincts, auditables et interprétables.

Principe central :

> **La stabilité n’est pas la vérité.
> Le grounding n’est pas la gouvernance.
> Les systèmes d’IA responsables ont besoin de signaux distincts, interprétables et auditables.**

---

## 12. Droits d’utilisation

Ce dépôt est publié à des fins de lecture et d’examen méthodologique uniquement.

Aucune licence de réutilisation n’est accordée.

Tous droits réservés par NeoMundi Research.

