# Evaluation Framework

## Evaluation comes before scale

Create the fixed evaluation set before expanding the dataset or increasing training time. Otherwise the project may optimize toward attractive samples without proving general improvement.

## Evaluation layers

| Layer | Question |
|---|---|
| Loading | Can the artifact, tokenizer, and configuration reload? |
| Syntax | Is the required format valid, especially JSON and function arguments? |
| Task | Does the model select or produce the correct answer or action? |
| Language | Does behavior remain reliable in Arabic and English? |
| Unknowns | Does the model handle unsupported or ambiguous inputs appropriately? |
| Regression | What useful original behaviors became worse? |
| Provenance | Are knowledge claims traceable to eligible evidence? |

## Baseline comparison

Every experiment should compare:

1. original model;
2. trained candidate;
3. deterministic expected output where available;
4. previous accepted release where available.

## Suggested metrics

### Structured output

- valid JSON rate;
- exact schema match;
- required-field accuracy;
- tool/function selection accuracy;
- argument-value accuracy.

### Natural language

- task correctness;
- factual support;
- terminology consistency;
- Arabic quality;
- English quality;
- unsupported-claim rate.

### Operational

- load success;
- inference latency;
- peak VRAM and RAM;
- output reproducibility;
- artifact size.

## Release decision

A lower training loss is not a release criterion by itself.

A candidate is accepted only when:

- the artifact reloads;
- the fixed evaluation threshold is met;
- no critical regression appears;
- source and dataset lineage are intact;
- limitations are documented;
- the reproduction command is preserved.

## Independent evaluation

Avoid circular evaluation. The same model configuration should not generate, validate, and judge the same records without independent controls.

Use a combination of:

- deterministic validators;
- held-out records;
- source checks;
- alternative model review where justified;
- human review for high-impact domains.
