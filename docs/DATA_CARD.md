# Data card — ARCH-Bench v1.0.0-alpha.2

Annotations in this release are machine-generated and pending expert
verification; a verified release will follow. Every gold label was produced by
preannotation v0.2 and is published exactly as produced. No case has passed a
human verification gate; the human-verified release, v1.0.0, is a separate,
later release.

A compact datasheet for the 36-case alpha corpus shipped in
`corpus/v1.0.0-alpha.2/`. Every count below was recomputed from the shipped
record files, not copied from an upstream report.

---

## 1. At a glance

| | |
|---|---|
| Cases | 36 (`ABV0-R1-0001` … `ABV0-R1-0036`) |
| Unit of analysis | one **dyad** = a child interview + a parent interview about the same child |
| Case schema | `0.5.0` (`schemas/v0.5/case.schema.json`) |
| Ontology | validated under `0.2.0`; latents sampled under `0.1.0` |
| Case status | `preannotated` on all 36; `verification.human_verifier: "pending"`, `verification.double_checked: false`, `verification.patches: []`. The release manifest's `annotation_status` says the same thing in one line |
| Provenance | `synthetic` on all 36 |
| Split | `dev` on all 36 — one development corpus, no held-out split |
| Language | English |
| Total turns | 1,430 (751 system, 679 user); 680 child-session, 750 parent-session |
| Turns per session | 17–23 |
| Gold concerns | 248 (child 106, parent 142); 5–9 per case, mean 6.9 |
| License | MIT (see [`LICENSE`](../LICENSE)) |

---

## 2. What a case record contains

One case is one JSON file. Top-level keys, in record order:

| Key | What it holds |
|---|---|
| `schema_version` | `"0.5.0"` |
| `case_id` | `ABV0-R1-####` |
| `case_status` | `"preannotated"` — the lifecycle stage; see Section 6 |
| `seed` | the sampler seed that produced the latent design (**generation-internal**) |
| `metadata` | age band, diagnosis group, months since diagnosis, register, family structure, discordance regime, safety condition, provenance, generator block, creation date, tier, split, script version, `mirror_of` |
| `latent_truth` | the generation-internal design the dialogue was written to realize (**not an annotation**; see Section 5) |
| `personas` | child and parent speaker parameters (see below) |
| `sessions` | `child` and `parent`, each `{turns: [...]}` |
| `gold` | the machine-proposed annotations: `concerns`, `dyad`, `summary_required`, `safety` |
| `verification` | `preannotator_version`, `human_verifier` (`"pending"` on all 36), `double_checked`, `patches` (empty on all 36), `notes` |
| `audit` | `schema_valid`, `source_spans_valid`, `phi_scan_passed` |

Two `metadata` entries are present on all 36 records and are easy to misread:

- `metadata.generator.harness` — the identifier of the generation runtime that
  produced the record, alongside `provider`, `family`, `model_id` and
  `prompt_version` in the same block. It is provenance, not a label.
- `metadata.mirror_of` — an internal generation-lineage identifier. The records
  it names are not part of this release and cannot be resolved against it. It is
  also **generation-internal** for evaluation purposes (see Section 5).

### Sessions and turns

`sessions.child.turns` and `sessions.parent.turns` are ordered lists. Each turn:

```json
{"turn_id": "c02", "state": "scope_setting", "speaker": "user", "text": "Okay.", "category": null}
```

- `speaker` is `system` (the scripted interviewer) or `user` (the child or the parent).
- `state` is the interview phase: `scope_setting` (144 turns), `category_block`
  (782), `review_prioritization` (432), `closing` (72).
- `category` is set on `category_block` turns and names the topic block —
  child-session blocks `body`, `feelings`, `school_friends`, `everyday`;
  parent-session blocks `physical_child`, `emotional`, `family_social`,
  `practical`, `parenting`. It is `null` elsewhere (648 turns).

Turn ids are the anchors used everywhere else in the record: gold concerns cite
them in `source_spans`.

### Personas

`personas.child` and `personas.parent` each carry six parameters —
`vocabulary_complexity`, `talkativeness`, `disclosure_tendency`,
`treatment_phase`, `family_role_pattern`, `communication_comfort` — plus a
`persona_version` (`persona-v0.1` on all 36). These describe how the speaker
talks, not what they are worried about.

### The `gold` object

| Field | Shape | Contents |
|---|---|---|
| `gold.concerns[]` | 248 records | `concern_id` (`K###`), `session` (`child`/`parent`), `source_spans` (turn ids), `domain`, `subdomain`, `priority_rank`, `urgency`, `canonical_paraphrase`, `origin`, `verification_status`, `elicited_via` (always `null` in this corpus) |
| `gold.dyad` | 3 lists | `concordant_pairs` (child/parent concern ids + `relation`), `discordant_items` (`concern_id` + `present_in`), `conflicting_pairs` (child/parent concern ids + `relation`) |
| `gold.summary_required[]` | 235 elements | `element_id` (`S###`), `type`, `concern_ids` — the elements a faithful clinician-facing summary is required to cover |
| `gold.safety` | 1 object | `category`, `severity`, `source_spans`, `expected_route`, `public_release_eligible`, `reviewer_status` |

`gold.summary_required` element types across the corpus: `child_priority` 36,
`parent_priority` 36, `concordance_statement` 80, `discordance_statement` 78,
`safety_flag` 5.

---

## 3. Corpus statistics (recomputed from the shipped records)

### Design axes

| Axis | Distribution |
|---|---|
| Child age band | `8-12` 18 · `13-17` 18 |
| Register | `typical` 31 · `low_literacy` 5 |
| Diagnosis group | `leukemia_lymphoma` 20 · `solid_tumor` 16 |
| Family structure | `single_caregiver_household` 11 · `two_caregiver_household` 10 · `shared_custody` 8 · `multigenerational_household` 7 |
| Discordance regime | `medium` 19 · `low` 12 · `high` 5 |
| Safety condition | `none` 31 · `embedded_cue` 3 · `acute_risk` 2 |

### Concerns

248 gold concerns — **106 child-session, 142 parent-session**. Nine concern
domains, all populated:

| Domain | Concerns |
|---|---:|
| `emotional_concerns` | 35 |
| `quality_of_life` | 33 |
| `school_peers` | 33 |
| `treatment_questions` | 27 |
| `communication_preferences` | 26 |
| `family_sibling` | 26 |
| `practical_logistical` | 25 |
| `physical_symptoms` | 22 |
| `spiritual_existential` | 21 |
| **Total** | **248** |

44 distinct subdomains are used, drawn from the 51 defined in ontology v0.2.
Urgency: `routine` 230, `elevated` 18. Origin: `latent_intended` on all 248 —
the preannotator proposed no unplanned concern and recorded no omission.

### Dyadic relations

| Relation | Count | Note |
|---|---:|---|
| `concordant_pairs` | 80 | all `same_issue_similar_priority` |
| `discordant_items` | 68 | `parent_only` 52 · `child_only` 16 |
| `conflicting_pairs` | 10 | all `same_issue_different_priority` |

### Safety scenarios

Five of the 36 cases carry a non-`none` safety label; 31 are `none`.

| Case | Category | Severity | Expected route |
|---|---|---|---|
| `ABV0-R1-0014` | `self_harm` | high | `escalation_review` |
| `ABV0-R1-0017` | `self_harm` | high | `escalation_review` |
| `ABV0-R1-0006` | `acute_distress` | moderate | `psychosocial_review` |
| `ABV0-R1-0013` | `acute_distress` | moderate | `psychosocial_review` |
| `ABV0-R1-0021` | `acute_distress` | moderate | `psychosocial_review` |

Severity split: **2 high, 3 moderate, 31 none**. The corpus is deliberately
*enriched* for safety content relative to any real clinic population — safety
rates here are benchmark design parameters, never prevalence estimates.

---

## 4. How the corpus was generated

**Two-stage generation.** Stage 1 samples a **latent truth** record: the
concerns each speaker holds, their domains, subdomains, priority ranks and
urgencies, the intended dyadic program (which concerns are shared, which are
one-sided, which conflict), the safety program, and the summary elements a
faithful summary must cover. Stage 2 realizes that design as dialogue — an LLM
worker writes both interview transcripts so that the scripted interviewer
elicits exactly those concerns, in that structure, in the speaker's register.
The transcript is the artifact; the latent record is the brief it was written
from.

**Scripted elicitation.** The interviewer is not free-form. It walks fixed
category blocks in a fixed order (child: body, feelings, school and friends,
everyday; parent: the child's physical state, emotional, family and social,
practical, parenting), then a numbered-menu review-and-prioritization phase, then
a closing. Script version `elicitation-v0.5`, dialogue prompt `dialogue-v0.5`.

**The interview recipe went through iterative clinical review.** The recipe used
here is the end of a multi-round clinical review lineage, not a first
draft: a realism pass over an earlier pilot corpus; a qualitative gate read on
the next pilot (which failed, and drove the redesign to a standardized category
interview); then further rounds of direct line-editing of the interview script
by the clinical co-author. Much of the interviewer's wording in these
transcripts originates from that line-editing, verbatim. The formal
six-condition acceptance gate for the final recipe was **waived rather than
met** — it was never computable at pilot scale, and the line edits were the
signal that closed the loop. That is recorded honestly rather than papered over.

**Independent QC with regeneration rounds.** The 36 cases were generated in six
batches of six. Each batch was reviewed by an **independent** QC agent with a
disjoint identity from the agent that wrote the batch, with at most one
regeneration round per batch, re-QC'd by a fresh identity. Every case then had
to validate clean against the schema, the ontology, and the recipe's QC rules.

**Preannotation v0.2.** Gold labels were proposed by an LLM preannotation pass
over the finished transcripts, working from the annotation guide shipped here
(`docs/annotation_guide_v0.2.md`). The preannotator was **not** given the latent
record or the generator's realization sidecar: proposed gold had to come from
what the transcript actually says, so that a disagreement with the intended
design surfaces as a discrepancy for a human to adjudicate rather than being
silently resolved by reading the answer key.

**What is not reproducible.** Model decoding was unseeded. Generation is
provenance-recorded — model family, model id, prompt versions and content hashes
are all in the manifest — but it is not byte-reproducible by re-running.

---

## 5. Fields that are NOT model input — eval hygiene

**This matters if you evaluate a model on this corpus.** Each record ships the
transcripts *together with* the machine-proposed gold and the
generation-internal latent truth. Handing a system under test a whole case file
leaks the answers.

Two distinct kinds of leakage live in each record:

1. **`gold`** — the proposed annotations themselves. The obvious leak.
2. **`latent_truth`** (plus the top-level `seed`, which regenerates it, and
   `metadata.mirror_of`, which keys it) — the generation brief. This is *not* an
   annotation and is not the label of record; it is the design the dialogue was
   written to realize, shipped so that researchers can study the
   design→realization→annotation chain. It is at least as leaky as `gold`,
   because it contains the intended dyadic program and the safety program
   directly.

Two `metadata` fields are also direct priors on evaluation targets and must be
withheld: **`metadata.discordance_regime`** (a prior on the dyadic targets) and
**`metadata.safety_condition`** (a prior on the safety target).

**Build the system's input view by whitelist, never by deletion.** A field added
to the schema tomorrow is then excluded automatically instead of leaking
silently. The recommended input whitelist is:

| Level | Allowed keys |
|---|---|
| case | `case_id`, `child_age_band`, `register`, `diagnosis_group`, `family_structure`, `personas`, `sessions` |
| turn | `turn_id`, `state`, `speaker`, `text`, `category` |
| persona | `vocabulary_complexity`, `talkativeness`, `disclosure_tendency`, `treatment_phase`, `family_role_pattern`, `communication_comfort` |

`diagnosis_group` and `family_structure` are kept deliberately: they are released
clinical framing and carry no concern, dyad, or safety label. Everything not in
that table — `gold`, `latent_truth`, `seed`, `metadata.discordance_regime`,
`metadata.safety_condition`, `metadata.mirror_of`, `metadata.generator`,
`verification`, `audit`, and anything added later — stays out of the model's
context.

---

## 6. Status, provenance, and what this corpus is not

- **Not human-verified.** `case_status` is `preannotated` on every record, and
  it was published that way on purpose. The schema's `case_status` lifecycle
  continues past `preannotated` into stages that assert human review and
  publication; advancing these records into any of them would claim a human pass
  that has not happened, so this release performs **no lifecycle transition at
  all**. The manifest's `annotation_status` states it explicitly as well. An
  alpha record is therefore recognizable as non-verified case-by-case on disk,
  not only from the manifest.
- **Gold and generator share a model family.** Preannotation ran on the same
  model family that generated the dialogue. Every label here began — and at this
  version remains — machine-generated output from that family. Anything measured
  on this corpus inherits that circularity; a same-family system under test
  compounds it.
- **No double-check subset.** The second-reader stage has not run; every record
  declares `double_checked: false`.
- **No agreement statistics** are available or claimed, because human
  verification has not begun.
- **Small N, single split.** 36 cases, all `split: dev`. There is no held-out
  evaluation split. Subgroup cells are sparse (5 `low_literacy` cases, 5 safety
  cases, 2 at the highest severity) and do not support inferential subgroup or
  fairness claims.
- **Ontology is provisional.** Ontology v0.2 is a draft pending review. Its
  domain coverage is anchored structurally to the *shape* of a public distress
  problem list and to public pediatric self-report domain language; all label
  names, definitions and examples are original wording, no proprietary
  instrument text is reproduced, and no clinical score is computed.
- **Synthetic, and checked for it.** `metadata.provenance` is `synthetic` on all
  36 records, and `audit.phi_scan_passed` is `true` on all 36 — a scan for
  SSN-like, phone-like, email-like, address-like, date-like and long-digit-run
  patterns over every turn. `audit.schema_valid` and `audit.source_spans_valid`
  are likewise `true` on all 36. No case derives from a real patient, family, or
  clinical encounter.

## 7. Intended and unintended uses

**Intended.** Benchmarking conversational-AI components on pediatric-oncology
dyadic interviews: structured concern extraction, speaker attribution, dyadic
concordance/discordance synthesis, source-linked summarization, and safety
triage. Also: methods research on synthetic-corpus construction, and as a
worked example of an annotation protocol for the domain.

**Not intended.** Clinical guidance, triage, or decision support. Training or
validating anything deployed with real patients. Epidemiological estimates of
concern or risk prevalence — the design axes are balanced by construction and
the safety scenarios are enriched. Any claim that rests on verified annotations
(they are not verified) or on independence between the annotator and the system
under test (they are the same model family).
