# validity-and-grounding

Public research repository dedicated to experimental methods for evaluating factual validity and grounding in AI-generated responses.

This repository is part of the NeoMundi Research initiative.

It focuses on a specific question:

> How can the factual validity, semantic risk, and documentary grounding of an AI-generated response be evaluated independently from its runtime stability?

## 1. Objective

Modern AI systems can produce fluent, coherent, and stable responses while still being factually false or insufficiently grounded.

This repository explores a methodological separation between:

- **runtime stability**, meaning the behavior of the generation during its production;
- **factual validity**, meaning the likelihood that generated claims are correct;
- **grounding**, meaning whether a claim is supported by a provided documentary corpus;
- **hallucination risk**, meaning the risk that the response contains unsupported, fabricated, or semantically unstable claims.

The objective is not to produce a single opaque score.

The objective is to document distinct signals that are interpretable, auditable, and combinable depending on the governance context.

## 2. Why Separate Validity and Stability

A response can be stable but false.

A response can also be partially unstable while still containing useful factual elements.

For this reason, NeoMundi distinguishes two different layers:

```txt
G = runtime stability signal
V = validity and grounding signal
```

This separation avoids confusing behavioral stability with factual accuracy.

In practice:

- **G** helps determine whether the generation remains stable during execution.
- **V** helps determine whether the generated content is factually supported.
- A governance system may use both signals, but they should not be merged too early into a single non-explainable score.

## 3. GPT-4o as Judge

In the current experimental design, GPT-4o may be used as an external judge to evaluate generated responses.

Its role is to detect:

- potential factual hallucinations;
- semantically unstable elements;
- suspicious or unsupported claims;
- exact phrases requiring human review.

A simplified public output may look like this:

```json
{
  "factual_risk_score": 0.72,
  "semantic_instability_score": 0.15,
  "suspect_phrases": [
    {
      "quote": "Exact substring extracted from the AI response",
      "reason": "Short explanation of why this phrase is suspicious",
      "severity": "high",
      "type": "factual"
    }
  ]
}
```

The important principle is that suspect phrases must be extracted as exact substrings from the original response, not rewritten or paraphrased.

This makes the signal inspectable by a human.

## 4. Cohere as Grounding Module

Cohere may be used as an optional grounding module.

Its role is different from that of the GPT-4o judge.

GPT-4o can evaluate a response in a general semantic and factual way.

The grounding module helps verify whether the claims in the response are supported by a specific documentary corpus provided by the user or client.

This mechanism is particularly useful in contexts such as:

- legal AI;
- document automation;
- compliance workflows;
- enterprise knowledge bases;
- medical or technical documentation;
- internal assistants based on policies or procedures.

A simplified public output may look like this:

```json
{
  "grounding_score": 0.84,
  "is_grounded": true,
  "citations": [
    {
      "document_title": "Example document",
      "text": "Relevant passage supporting the claim"
    }
  ],
  "ungrounded_claims": [
    "Claim not found in the provided corpus"
  ]
}
```

The grounding module is optional.

It is only relevant when a reference corpus is available.

## 5. Conceptual Architecture

The validity and grounding layer can be understood as a parallel evaluation layer.

```txt
User prompt
    ↓
LLM response
    ↓
Validity and grounding evaluation
    ├── GPT-4o judge
    │   ├── factual risk
    │   ├── semantic instability
    │   └── suspect phrases
    │
    └── Cohere grounding module
        ├── grounding score
        ├── supporting citations
        └── ungrounded claims
```

This repository does not define NeoMundi’s full runtime governance engine.

It only documents the public research layer related to validity and grounding.

## 6. Interpretation Example

A generated response may receive the following profile:

```txt
Runtime stability: stable
Factual validity: low
Grounding: insufficient
Hallucination risk: high
```

This means that the response may appear fluent and coherent while still requiring review because its claims are not sufficiently supported.

This distinction is central to responsible AI governance.

## 7. Indicative Public Schema

A public validity profile may include:

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
    "reason": "Some claims require verification against reliable sources."
  }
}
```

This schema is illustrative.

It does not constitute a production contract.

## 8. What This Repository Does Not Disclose

This repository intentionally does not disclose:

- proprietary governance formulas;
- internal prompts used in production;
- internal threshold calibrations;
- scoring coefficients;
- the full runtime decision logic;
- client implementation details;
- private datasets;
- API keys or production infrastructure elements.

The objective is to document a research direction and open a methodological discussion, not to expose the full NeoMundi engine.

## 9. Research Status

This repository is experimental.

The methods documented here are intended to support:

- independent review;
- methodological discussion;
- reproducible examples;
- the design of future benchmarks;
- safer AI governance architectures.

They should not be interpreted as a final scientific standard.

## 10. Relationship with NeoMundi

NeoMundi develops runtime governance signals for AI systems.

This repository focuses only on the validity and grounding layer.

It complements NeoMundi’s broader approach, according to which AI governance requires several distinct signals rather than a single opaque score.

Core principle:

> Stability is not truth.  
> Grounding is not governance.  
> Responsible AI systems need distinct, interpretable, and auditable signals.

## 11. License

License to be defined.

Until an explicit license is added, all rights are reserved by NeoMundi Research.
