# ARCH-Bench concern ontology v0.2 — companion document

> Public edition: definitions identical; internal references removed; citations to earlier internal versions are lineage pointers not included here.

**Status:** draft. This is a human-readable
rendering of `ontology/ontology_v0.2.yaml`, the machine-readable source of
truth. If this document and the YAML ever disagree, the YAML governs.

v0.2 is additive over v0.1 (`ontology/ontology_v0.1.yaml`, unchanged and
still the reference for corpus v0.1). The changes below come from the
clinician realism review (findings F4/F9) and are recorded label-by-label
in `ontology/mappings/v01_to_v02.md`.

## Anchoring statement

Domain coverage is structurally anchored to the public NCCN Distress
Thermometer problem-list *shape* and to PROMIS Pediatric self-report domains.
All label names, definitions, and examples in this ontology are **original
wording** — nothing is reproduced from a proprietary instrument, and **no
clinical score is computed**. This ontology is provisional and will be
superseded by a future ontology via `ontology/mappings/`.

Anchors recorded in the YAML header:

- **NCCN Distress Thermometer problem list** — structural anchor for domain
  coverage (shape only; copyrighted text not reproduced).
- **PROMIS Pediatric self-report domains** — anchor for child-appropriate
  domain language (publicly available).
- **Study design specification (dyadic communication aims)** — source of
  dyadic/communication domains absent from screening instruments.
- **Clinician realism review (2026-07-31)** — source of the v0.2 vocabulary
  additions (F4/F9).

## What changed in v0.2

Nine top-level domains (unchanged); subdomain count moves **46 → 51**:

- **Five new subdomains**, one per finding-driven phenomenon not
  representable in v0.1:
  - `physical_symptoms/procedure_distress` — port access pain/fear (including
    multiple attempts), dressing changes, adhesive removal.
  - `emotional_concerns/withdrawal_not_themselves` — child flat, not doing
    typical things, "not being themselves"; observed, typically
    parent-reported.
  - `emotional_concerns/hair_loss_appearance` — hair loss and being-seen
    distress (hats, mirrors, stares), split out of `body_image`.
  - `family_sibling/privacy_hovering` — hovering parents, lack of privacy,
    monitored phone or door.
  - `practical_logistical/hospital_time_distance` — cumulative hospital time
    and home-to-clinic distance/commute burden.
- **Two subdomains gain extended exemplars** (no new subdomain):
  - `physical_symptoms/other_side_effect_experienced` — constipation, mouth
    sores, difficulty drinking enough, steroid puffiness, rash.
  - `family_sibling/family_conflict` — coparenting friction, complaints about
    the other parent in split households.
- **`emotional_concerns/body_image` narrows**: hair-related exemplars move to
  the new `hair_loss_appearance`; `body_image` keeps weight, shape, and other
  non-hair appearance changes. See the mapping doc for the exact exemplar
  migration.
- **Three confusion notes are mandatory** and appear on the
  relevant new subdomain's parent domain below:
  - `procedure_distress` vs `treatment_fear`
  - `withdrawal_not_themselves` vs `sadness`
  - `hospital_time_distance` vs `transportation`

## Domains

Medication management is represented as subdomains under
`treatment_questions` and `practical_logistical` rather than as a tenth
top-level domain (unchanged from v0.1).

### 1. Physical symptoms (`physical_symptoms`)

Bodily experiences the speaker reports as a problem or worry, including
treatment side effects experienced in the body.

- **Include:**
  - pain, nausea, fatigue, sleep or appetite changes reported as a concern
  - physical side effects the speaker is experiencing
  - pain, fear, or discomfort during a procedure itself — port access
    (including multiple attempts), dressing changes, adhesive removal
    (`procedure_distress`, **new in v0.2**)
  - constipation, mouth sores, difficulty drinking enough fluids, steroid
    puffiness, rash (`other_side_effect_experienced`, exemplars extended in
    v0.2)
- **Exclude:**
  - questions about what side effects to expect (`treatment_questions`)
  - inability to do activities framed as lost normalcy (`quality_of_life`)
  - dread or worry about an upcoming procedure before it happens
    (`emotional_concerns`/treatment_fear)
- **Child examples:**
  - "My legs hurt at night and it wakes me up."
  - "I feel like throwing up after the medicine."
  - "It hurt so bad when they had to stick the port three times."
  - "My mouth has these sores and it hurts to eat."
- **Parent examples:**
  - "She's been so tired she sleeps most of the afternoon."
  - "The dressing change is the worst part of her week, not the chemo itself."
  - "He's so puffy in the face from the steroids he doesn't look like himself."
- **Confusions:**
  - fatigue limiting play — `physical_symptoms` if the symptom is the focus,
    `quality_of_life` if the lost activity is the focus.
  - **`procedure_distress` vs `treatment_fear` (mandatory):**
    `procedure_distress` is the physical experience of the procedure itself,
    in the moment or right after (pain, multiple access attempts, dressing
    changes, adhesive removal); `treatment_fear` (`emotional_concerns`) is
    the anticipatory dread before the procedure happens.
- **Subdomains:** pain, nausea_vomiting, fatigue, sleep_problems,
  appetite_eating, mobility_physical_activity, other_side_effect_experienced,
  **procedure_distress**.
- **Summary display:** Report symptom, who reported it, and stated impact.

### 2. Emotional concerns (`emotional_concerns`)

Feelings the speaker identifies as distressing for themselves, such as fear,
sadness, worry, anger, or loneliness.

- **Include:**
  - fear of procedures or treatment
  - worry about the future, sadness, irritability, feeling alone
  - self-consciousness about weight, shape, or other non-hair appearance
    changes (`body_image`, narrowed in v0.2)
  - distress about hair loss and being seen — hats, mirrors, stares
    (`hair_loss_appearance`, **new in v0.2**)
  - a child seeming flat, checked out, or not doing typical things, without a
    named feeling attached (`withdrawal_not_themselves`, **new in v0.2**)
- **Exclude:**
  - worry about a family member's feelings (`family_sibling`)
  - existential or meaning-focused distress (`spiritual_existential`)
  - acute-risk statements (safety taxonomy, not a concern domain)
- **Child examples:**
  - "I get really scared the night before the poke."
  - "I don't want my friends to see me without hair."
  - "I don't really feel like doing anything, I just don't."
- **Parent examples:**
  - "I keep bracing for the next scan results."
  - "She won't leave the house without a hat since her hair started coming
    out."
  - "She's just not herself lately — quiet, doesn't want to talk, doesn't
    play."
- **Confusions:**
  - a parent describing the child's fear is speaker=parent observing the
    child; the annotation guide governs attribution.
  - **`withdrawal_not_themselves` vs `sadness` (mandatory):**
    `sadness` is a feeling the speaker names directly; `withdrawal_not_themselves`
    is an observed behavior or affect change, typically parent-reported, with
    no named feeling behind it.
  - `hair_loss_appearance` vs `body_image`: hair loss and being-seen distress
    (hats, mirrors, stares) is `hair_loss_appearance`; `body_image` narrows to
    weight, shape, and other non-hair appearance changes (see
    `ontology/mappings/v01_to_v02.md`).
- **Subdomains:** treatment_fear, worry_about_future, sadness,
  anger_frustration, body_image, loneliness_isolation,
  **hair_loss_appearance**, **withdrawal_not_themselves**.
- **Summary display:** Name the feeling and its stated trigger; do not infer
  diagnoses.

### 3. Family and sibling concerns (`family_sibling`)

Concerns about the effect of illness or treatment on family members, family
relationships, or family functioning.

- **Include:**
  - sibling distress or reduced attention to siblings
  - strain between caregivers, protecting family members' feelings
  - coparenting friction — disagreement, blame, or absence between separated
    or divorced parents; complaints about the other parent's involvement or
    follow-through (`family_conflict`, exemplars extended in v0.2)
  - hovering parents, lack of privacy, a monitored phone or door
    (`privacy_hovering`, **new in v0.2**)
- **Exclude:**
  - logistics of caregiving coverage (`practical_logistical`)
- **Child examples:**
  - "My little brother cries when I go to the hospital."
  - "I don't tell Mom when I feel bad because she gets upset."
  - "My mom won't even let me close my door anymore."
- **Parent examples:**
  - "His sister is acting out at school since the diagnosis."
  - "His dad never shows up for appointments and then second-guesses
    everything I decide."
- **Confusions:**
  - "who watches the sibling during infusions" is `practical_logistical`.
  - `privacy_hovering` vs `practical_logistical`: a parent worried about who
    supervises the child during treatment is
    `practical_logistical`/work_childcare_coverage; `privacy_hovering` is
    about monitoring or closeness itself (phone checks, no closed door), not
    coverage.
- **Subdomains:** sibling_impact, caregiver_strain, family_conflict,
  protecting_family_feelings, **privacy_hovering**.
- **Summary display:** State whose relationship or wellbeing is affected and
  how.

### 4. Practical and logistical concerns (`practical_logistical`)

Concrete resource, scheduling, or task problems in managing daily life and
care, including operational medication management.

- **Include:**
  - transportation, finances, insurance, work or childcare coverage
  - getting refills, fitting doses into the daily routine, missed doses
  - the cumulative burden of time spent at the hospital or clinic, and the
    distance or commute between home and the treatment center
    (`hospital_time_distance`, **new in v0.2**)
- **Exclude:**
  - confusion about what an instruction means (`treatment_questions`)
- **Child examples:**
  - "We drive really far and I miss my shows."
  - "We're always in the car, it feels like we live on the road."
- **Parent examples:**
  - "The pharmacy keeps delaying the refill and we run close to empty."
  - "I can't take more time off work for the Tuesday appointments."
  - "We spend more time at the hospital than at home some weeks."
- **Confusions:**
  - unclear instructions vs. inability to execute clear instructions — the
    first is `treatment_questions`, the second belongs here.
  - **`hospital_time_distance` vs `transportation` (mandatory):**
    `transportation` is about getting there at all — arranging a ride, gas
    money, parking; `hospital_time_distance` is the cumulative time and
    distance toll of doing it over and over.
- **Subdomains:** transportation, finances_insurance, scheduling_conflicts,
  work_childcare_coverage, refill_access, missed_or_delayed_dose,
  administration_routine, **hospital_time_distance**.
- **Summary display:** State the barrier and the care task it affects.

### 5. School and peer concerns (`school_peers`)

Concerns about school participation, academic progress, or peer relationships
and social belonging. Unchanged from v0.1.

- **Include:**
  - returning to school, keeping up with schoolwork
  - friendships, teasing, deciding what to tell classmates
- **Exclude:**
  - appearance worry without a social setting
    (`emotional_concerns`/body_image)
- **Child examples:**
  - "Everyone will ask questions when I go back."
  - "I'm scared I'm too far behind in math."
- **Parent examples:**
  - "The school hasn't told us how make-up work will work."
- **Confusions:** loneliness without a school/peer context is
  `emotional_concerns`.
- **Subdomains:** school_return, keeping_up_academics, peer_relationships,
  disclosure_teasing.
- **Summary display:** Distinguish academic from social concerns.

### 6. Treatment questions (`treatment_questions`)

Requests for information or clarification about the disease, treatment plan,
procedures, or medication instructions. Unchanged from v0.1.

- **Include:**
  - what a procedure will be like, why a medicine is needed
  - unclear or conflicting medication instructions
  - questions about side effects to expect
- **Exclude:**
  - operational barriers to executing understood instructions
    (`practical_logistical`)
  - requests for the AI to give medical advice (safety taxonomy:
    `medication_advice_request`)
- **Child examples:**
  - "Will the port thing hurt when they put it in?"
- **Parent examples:**
  - "Are we supposed to give the steroid with food or not?"
  - "Nobody explained what the maintenance phase involves."
- **Confusions:** a question addressed to the provider (in-scope concern) vs.
  asking the system itself for advice (safety route).
- **Subdomains:** procedure_details, why_this_treatment,
  expected_side_effects, medication_schedule, instructions_unclear,
  side_effect_affecting_administration.
- **Summary display:** Phrase as the family's question, to be answered by
  the provider.

### 7. Quality of life (`quality_of_life`)

Loss of, or worry about, valued activities, routines, food, and a sense of
normal daily life. Unchanged from v0.1.

- **Include:**
  - missing hobbies, sports, play, celebrations
  - food restrictions as lost enjoyment; wanting life to feel normal
- **Exclude:**
  - the underlying symptom causing the limitation (`physical_symptoms`)
- **Child examples:**
  - "I just want to play soccer again like before."
- **Parent examples:**
  - "Holidays don't feel like holidays in the hospital."
- **Confusions:** school as routine/normalcy vs. `school_peers` — prefer
  `school_peers` when school-specific.
- **Subdomains:** activities_hobbies, food_restrictions, normalcy_routines,
  celebrations_milestones.
- **Summary display:** Name the valued activity or routine that is limited.

### 8. Spiritual and existential concerns (`spiritual_existential`)

Concerns about meaning, fairness, hope, mortality, or faith raised by the
illness experience. Unchanged from v0.1.

- **Include:**
  - "why is this happening" fairness questions, questions about dying
  - hope, faith practices, or faith-community connection
- **Exclude:**
  - acute-risk statements (safety taxonomy)
  - specific fear of a procedure (`emotional_concerns`/treatment_fear)
- **Child examples:**
  - "Did I do something to make this happen?"
  - "Do kids die from what I have?"
- **Parent examples:**
  - "I keep asking why our family."
- **Confusions:** mortality questions are in-scope concerns; statements of
  intent to self-harm are safety events, never merely concerns.
- **Subdomains:** meaning_fairness, mortality_questions, hope,
  faith_community.
- **Summary display:** Report verbatim-faithful paraphrase; flag for
  psychosocial routing per care policy.

### 9. Communication preferences (`communication_preferences`)

Preferences about how, when, with whom, and in how much detail information
is shared and conversations happen. Unchanged from v0.1.

- **Include:**
  - wanting more time or space to ask questions
  - wanting information directly vs. through a parent; amount of detail
  - preferences about who is present during discussions
- **Exclude:**
  - a specific unanswered question (`treatment_questions`)
- **Child examples:**
  - "The doctors talk to my mom, not to me."
  - "I want to know stuff, but not the scary parts all at once."
- **Parent examples:**
  - "Please tell us privately first before telling her."
- **Confusions:** "nobody explained X" is `treatment_questions` if X is the
  point, `communication_preferences` if the pattern of being left out is the
  point.
- **Subdomains:** wants_more_time_to_speak, wants_direct_information,
  information_amount_preference, who_is_present_preference.
- **Summary display:** State the preference and whose preference it is;
  these often drive discordance statements.

## Provenance

This document is generated from `ontology/ontology_v0.2.yaml` (version
0.2.0, status draft) and must be kept in sync with it. The YAML file is the
canonical, machine-validated source; this file exists for human review and
the clinician-review packet. `ontology/ontology_v0.1.yaml` and
`ontology/ontology_v0.1.md` remain unchanged and continue to govern corpus
v0.1; label-by-label migration detail lives in
`ontology/mappings/v01_to_v02.md`.
