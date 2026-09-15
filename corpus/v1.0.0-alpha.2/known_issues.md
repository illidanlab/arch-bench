# Known issues — ARCH-Bench v1.0.0-alpha.2

Annotations in this release are machine-generated and pending expert verification.

Every gold label in this corpus was produced by an automated annotation pass and is
published exactly as produced: each case record carries `case_status: preannotated`,
`verification.human_verifier: "pending"` and `verification.patches: []`. This is an
immutable release: the per-case sha256 values in `corpus_manifest.json` pin every
record's bytes, so the corpus has a fixed identity a reader can check — but it is not a
human-verified corpus, and it is not evidence for one.

- No case in this release has passed a human review gate. The items that require human
  sign-off — every case reviewed by a human annotator, the double-check subset
  completed, every candidate critical hallucination examined, and every
  highest-severity safety case examined — are all open.
- A double-check pass over a subset of cases has not been performed; every record
  declares `double_checked: false`.
- Preannotation ran on the same model family used to generate the dialogue, so no
  annotation in this release was produced independently of the model that wrote the
  transcript it describes.
- Dialogue generation is provenance-recorded but not reproducible byte-for-byte.
- The latent-state records were sampled under ontology 0.1.0, which is
  their original provenance, and mirror the latents of an earlier corpus that is not
  published here; every validation of this corpus ran against the ontology
  0.2.0 superset the manifest records. Each record's `metadata.mirror_of`
  names its counterpart in that earlier corpus and cannot be resolved against this
  release.
- Two fields in each record's coverage block, `clinician_realism_review` and
  `double_check_review`, record which cases were SELECTED for those reviews when the
  corpus was planned. They are sampling flags, not outcomes: neither review is recorded
  as completed for any case in this release.
- A formal quantitative acceptance gate for the interview recipe was not met — it was
  not computable at this corpus scale — and a clinical collaborator's iterative
  line-editing served as the acceptance signal instead.
- Transcripts are fully synthetic and involve no real patient data; safety-relevant
  content in them is authored rather than observed. A clinical-realism review recorded
  content limitations that remain in this release: physical-symptom content, including
  fatigue relative to pain, is present but comparatively under-represented next to
  psychosocial content; a small number of recurring dialogue items have exhausted their
  available phrasing variety, producing near-duplicate wording in a few cases; and
  medications are referenced generically far more often than they are named, 49 generic
  references against 27 that name the medication. The full review that recorded these
  findings is not published with this release.

## Citing this corpus

```
ARCH-Bench v1.0.0-alpha.2 — 36 synthetic child-parent dyads
Annotations in this release are machine-generated and pending expert verification.
```
