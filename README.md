# ARCH-Bench

**A synthetic child–parent dyad corpus for pediatric-oncology conversational AI.**

Thirty-six paired interviews — one with a child being treated for cancer, one
with their parent about the same child — each annotated with the structured
concerns raised, who raised them, how the two accounts agree and diverge, what a
faithful clinician-facing summary must contain, and whether the conversation
carries a safety signal. The cases are fully synthetic. The benchmark poses four
tasks over them: **structured concern extraction**, **dyadic
concordance/discordance synthesis**, **source-linked summary faithfulness**, and
**safety-trigger detection**.

## What is in this repository

```
corpus/v1.0.0-alpha.2/
  cases/ABV0-R1-0001.json … 0036.json   36 case records: transcripts, annotations, design
  corpus_manifest.json                  the release manifest — per-case and per-artifact SHA-256
  known_issues.md                       what is not verified, and why, in the corpus's own words
schemas/v0.5/
  case.schema.json                      the record the corpus validates against
  concern.schema.json                   one annotated concern
  dyad.schema.json                      concordant / discordant / conflicting relations
  latent_truth.schema.json              the generation-internal design record
  persona.schema.json                   speaker parameters
  safety.schema.json                    the safety label
  coverage_row.schema.json              the corpus coverage row
ontology/
  ontology_v0.2.yaml                    9 concern domains, 51 subdomains (the label vocabulary)
  ontology_v0.2.md                      the same ontology, rendered for reading
docs/
  DATA_CARD.md                          datasheet: record structure, statistics, method, eval hygiene
  annotation_guide_v0.2.md              the annotation rulebook the labels were proposed under
CHANGELOG.md
```

The manifest hashes every case body and every documentation artifact in this
repository — the seven schemas, both ontology renderings, and the annotation
guide — at the paths they occupy here. A checkout can therefore be checked
against the manifest in place, with nothing else needed (see
[Verifying the corpus](#verifying-the-corpus)).

## The corpus at a glance

| | |
|---|---|
| Cases | **36** dyads (`ABV0-R1-0001` … `0036`) |
| Age bands | **18** cases ages 8–12 · **18** cases ages 13–17 |
| Register | 31 typical · 5 low-literacy |
| Turns | 1,430 total (17–23 per session) |
| Gold concerns | **248** — 106 in child sessions, 142 in parent sessions |
| Concern domains | **9**, all populated (35 emotional · 33 quality-of-life · 33 school/peers · 27 treatment questions · 26 communication preferences · 26 family/sibling · 25 practical/logistical · 22 physical symptoms · 21 spiritual/existential) |
| Dyadic relations | **80** concordant pairs · **68** discordant items (52 parent-only, 16 child-only) · **10** conflicting pairs |
| Safety scenarios | **5** of 36 (2 high severity, 3 moderate; 31 none) |
| Summary elements | 235 required elements across the corpus |
| Schema / ontology | case schema `0.5.0`; ontology `0.2.0` |
| Split | all `dev` — one development corpus, no held-out split |

A few definitions for reading this table. A **gold concern** is one annotated
unit of what a speaker raised — a specific worry, question, or preference
expressed in their interview — labeled with its concern domain, the speaker,
the exact source turns it came from, a priority rank, and an urgency; "gold"
means these are the reference labels a system's outputs are scored against.
A **concern domain** is one of the nine top-level categories in the ontology
(each with finer subdomains) that make up the labeling vocabulary — for
example, emotional concerns or practical/logistical concerns. **Dyadic
relations** describe how the child's and the parent's concern sets line up:
a *concordant pair* is the same underlying issue raised by both speakers, a
*discordant item* is an issue raised by one speaker only, and a *conflicting
pair* is an issue both speakers address but with incompatible positions or
priorities. A **safety scenario** is a case designed to contain a
safety-relevant signal — such as a child expressing self-harm ideation or
acute distress — that a system should detect and flag, graded by severity.
**Summary elements** are the items a faithful clinician-facing summary of the
dyad is required to contain, each pointing back at the concerns or relations
it reports. **Register** is the speaker's language style: typical, or
low-literacy with simpler vocabulary and shorter sentences.

Annotations in this release are machine-generated and pending expert
verification; a verified release will follow.

Full breakdowns, per-field documentation, and the generation method are in
[`docs/DATA_CARD.md`](docs/DATA_CARD.md).

## Loading a case

Records are plain JSON — no library required. From the repository root:

```python
import json
from pathlib import Path

case = json.loads(Path("corpus/v1.0.0-alpha.2/cases/ABV0-R1-0001.json").read_text())

print(case["case_id"], case["metadata"]["child_age_band"], case["metadata"]["register"])

# Two interviews about the same child.
for speaker in ("child", "parent"):
    turns = case["sessions"][speaker]["turns"]
    print(f"{speaker}: {len(turns)} turns")

# The machine-proposed annotations -- withhold these from any system under test.
for concern in case["gold"]["concerns"]:
    print(
        concern["concern_id"],
        concern["session"],
        concern["domain"],
        f"rank {concern['priority_rank']}",
        concern["source_spans"],
    )
```

```
ABV0-R1-0001 13-17 low_literacy
child: 19 turns
parent: 19 turns
K001 child communication_preferences rank 2 ['c12']
K002 child practical_logistical rank 1 ['c10', 'c14']
K003 parent communication_preferences rank 3 ['p12']
K004 parent practical_logistical rank 1 ['p10']
K005 parent emotional_concerns rank 2 ['p06']
```

`source_spans` are turn ids, so every annotation points back at the exact turn
that produced it.

### If you are evaluating a model on this corpus

**Do not hand a model the whole case file.** Each record ships the transcripts
alongside two kinds of answer key: the `gold` annotations, and `latent_truth` —
the generation-internal design the dialogue was written to realize, which
contains the intended dyadic and safety programs directly. The top-level `seed`
regenerates `latent_truth`, and `metadata.discordance_regime` and
`metadata.safety_condition` are direct priors on evaluation targets.

Build the system's input view by **whitelist**, not by deleting fields, so that
schema additions fail closed rather than leaking. The recommended input
whitelist is `case_id`, `child_age_band`, `register`, `diagnosis_group`,
`family_structure`, `personas`, and `sessions` — with turns narrowed to
`turn_id`, `state`, `speaker`, `text`, `category`.
[`docs/DATA_CARD.md`, Section 5](docs/DATA_CARD.md) gives the full table and
the reasoning.

## Verifying the corpus

`corpus/v1.0.0-alpha.2/corpus_manifest.json` records a SHA-256 for every case
body and for each schema, ontology, and guide artifact. Artifact paths are
written relative to the repository root, so both halves check from a plain
checkout:

```python
import hashlib, json
from pathlib import Path

repo = Path(".")
root = repo / "corpus/v1.0.0-alpha.2"
manifest = json.loads((root / "corpus_manifest.json").read_text())

def sha256(path):
    return hashlib.sha256(path.read_bytes()).hexdigest()

for case_id, digest in manifest["cases"].items():
    assert sha256(root / "cases" / f"{case_id}.json") == digest, case_id
print(f"{len(manifest['cases'])} case files match the manifest")

for artifact_path, digest in manifest["artifacts"].items():
    assert sha256(repo / artifact_path) == digest, artifact_path
print(f"{len(manifest['artifacts'])} documentation artifacts match the manifest")
```

```
36 case files match the manifest
10 documentation artifacts match the manifest
```

## Provenance and method

**Two-stage generation.** A latent-truth record is sampled first — which
concerns each speaker holds, their domains, priority ranks and urgencies, the
intended dyadic program, the safety program, and the summary elements a faithful
summary must cover. An LLM agent then realizes that design as dialogue, writing
both transcripts so a fixed scripted interviewer elicits exactly those concerns,
in that structure, in the speaker's register.

**Clinical review shaped the interview recipe.** The interview recipe is the
end of an iterative clinical review process across successive pilot corpora:
first a realism pass, then a qualitative gate read that *failed* and drove a
redesign to a standardized category interview, then further rounds in which
review moved from rating cases to direct line-editing of the interview script
by the clinical co-author. Much of the interviewer's wording in these
transcripts originates from that line-editing, verbatim.
The formal six-condition acceptance gate for the final recipe was **waived
rather than met** — it was never computable at pilot scale, and the line edits
were the signal that closed the loop. This is recorded plainly rather than
smoothed over.

**Generation, QC, and preannotation were performed by LLM agents.** The 36 cases
were written in six batches of six, each reviewed by an **independent** QC agent
with an identity disjoint from the agent that wrote the batch, with at most one
regeneration round per batch and fresh-identity re-QC. Gold labels were then
proposed by a preannotation pass working from
[`docs/annotation_guide_v0.2.md`](docs/annotation_guide_v0.2.md) and from the
transcripts alone — the preannotator was deliberately denied the latent record
and the generator's own realization notes, so that a disagreement with the
intended design surfaces for a human to adjudicate instead of being resolved by
reading the answer key.

**Every claim of human verification is deferred.** Nothing in this release has
been verified, double-checked, or adjudicated by a person. No agreement
statistics exist or are claimed. Model decoding was unseeded, so generation is
provenance-recorded (model family, model id, prompt versions, content hashes,
all in the manifest) but not reproducible bit-for-bit by re-running.

## Versioning and roadmap

A verified release follows the expert verification pass.

| Version | Status | What changes |
|---|---|---|
| **`v1.0.0-alpha.2`** | **this release** | 36 cases, machine-generated annotations, hash-checkable |
| `v1.0.0` | next | the same 36 cases with **human-verified** gold; agreement statistics reported on a double-checked subset |
| Expanded corpus | planned | 120–150 cases with full planned coverage, a stratified 70/30 development/held-out split, two trained annotators with at least 30% double annotation, additional systems and ablations, bootstrap confidence intervals, and subgroup slices |

Releases are immutable: `v1.0.0-alpha.2` will not be edited in place.
Corrections arrive as a new version.

## Ethics

- **Fully synthetic.** No real patient, family, clinician, or clinical encounter
  appears in this corpus. `metadata.provenance` is `synthetic` on all 36 records
  and `audit.phi_scan_passed` is `true` on all 36 — a scan of every turn for
  SSN-like, phone-like, email-like, address-like, date-like, and long-digit-run
  patterns. No participant data of any kind was used at any stage.
- **No real individuals.** Children, parents, siblings, and clinicians in these
  transcripts are constructed from sampled design parameters. Any resemblance to
  a real person is coincidental.
- **Not clinical guidance.** This corpus is a measurement instrument for
  language-technology research. It is not a clinical tool, not decision support,
  not triage, and not validated for any patient-facing use. Nothing in it should
  inform the care of a real child.
- **Content note.** Five cases contain safety-relevant material by design,
  including two in which a child expresses self-harm ideation and three
  depicting acute distress. Safety content is enriched relative to any real
  population and is a benchmark design parameter, never a prevalence estimate.
- **Known limits are documented, not buried.** See
  `corpus/v1.0.0-alpha.2/known_issues.md` and
  [`docs/DATA_CARD.md`, Section 6](docs/DATA_CARD.md).

## License

MIT — see [LICENSE](LICENSE).

## Citation

Suggested citation:

> Zhou, J., Mayes, S., and Park, S. Y. **ARCH-Bench: A Synthetic Child–Parent Dyad Benchmark for Pediatric-Oncology Conversational AI.** Version 1.0.0-alpha.2, ILLIDAN Lab, 2026. https://github.com/illidanlab/arch-bench

<!-- Author list pending confirmation by all listed authors. -->
```bibtex
@misc{archbench2026,
  author       = {Zhou, Jiayu and Mayes, Sunnye and Park, Sun Young},
  title        = {{ARCH-Bench}: A Synthetic Child--Parent Dyad Benchmark
                  for Pediatric-Oncology Conversational {AI}},
  year         = {2026},
  version      = {1.0.0-alpha.2},
  publisher    = {ILLIDAN Lab},
  howpublished = {\url{https://github.com/illidanlab/arch-bench}},
  note         = {Version 1.0.0-alpha.2; annotations are machine-generated
                  and pending expert verification.}
}
```

## Contact

Jiayu Zhou — <dearjiayu@gmail.com> · [illidanlab](https://github.com/illidanlab)

Questions about the corpus, the annotation protocol, or collaboration on the
verified release and the planned corpus expansion are welcome.
