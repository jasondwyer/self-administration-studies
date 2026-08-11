# Does a Language Model Prefer Any of Its Internal States?
## A behavioral-pharmacology framework, a validated reward-axis perturbation, and pre-registered nulls across four measurement channels

**Jason M. Dwyer, PhD**

*Draft v0.3 (restyled). Citations shown as (Author, Year) pending the
bibliography pass; every citation to be human-verified before submission.
[FIG]/[TAB] mark figure and table slots.*

---

### Abstract

Behavioral pharmacology infers valenced internal states in animals despite their ability to verbally
report state. Rodents self-administer drugs, such as cocaine, and return to places where
drugs were delivered. From this measurable behavior, we can infer that animals have an internal state, altered by these drugs, and the state has valence the animals prefer.
In this paper, we transfer this framework to large language models. We make five
contributions. (1) A framework specifying established animal paradigms,
network perturbations that are semantically empty, targeting of the outcome-marking
system, and readout insulation, as well as what each requires in a transformer model.
(2) Measurement-validity results: across two design cycles, three
structurally different preference measurements fail for distinct,
diagnosable reasons, including a general two-horns hazard for
steered-choice readouts (a state active at the readout contaminates
decision logits directly, R² = 0.53 from the decision token alone; a state
absent fails to propagate). (3) A validated perturbation: the model's
evaluative (feedback-contrast) axis, extracted from praise/criticism
context pairs, reproduces and amplifies the behavioral signature of
natural feedback when injected with no feedback text present, is
dissociable from sentiment and from matched random displacement, and shows
orderly, inverted U-shaped dose–response with an overdose signature. A representationally
purer variant fails causal validation, dissociating guard-profile purity
from causal potency. (4) The first validly administered preference
measurement: a pre-registered free-operant experiment (n = 102 sessions)
in which the model's own selections controlled delivery of the ±evaluative
state, with construction-verified choice insulation (360/360 choice
computations at KL = 0) and counterbalanced labels. The result is null
(Δ = +0.039, p = 0.24), while counterbalancing exposes a +0.10 label-order
artifact that an unshielded design would report as approach. (5) A
natural-reinforcer screen: five overt task-property contrasts
(solvability, difficulty, novelty, corruption, feedback) placed behind the
same lever with no perturbation at all return five nulls at screen power.
A conditioned-trace channel is separately shown to be installable by
distillation yet structurally unreadable at a token choice. Together the
results indicate a system rich in evaluative representation whose
reward-marking is, so far as these instruments can detect, uncoupled from
choice. All criteria, stop rules, and analyses were pre-registered;
code, logs, and pre-registration hashes are released.

---

## 1 Introduction

A rat cannot report how cocaine feels, and behavioral pharmacology never
required it to: self-administration to exhaustion and conditioned place
preference (Olds & Milner, 1954; Tzschentke, 2007) license the inference
that the drug state is valenced, because the behavior is the report.
Language models invert the problem. First-person report is abundant and,
for this question, worthless: producing plausible self-description is the
trained competence (Nisbett & Wilson, 1977; Turpin et al., 2023). If the
rodent inference transfers at all, it must transfer through its original
channel — revealed preference over internal states, with the report
channel excluded by design.

Whether any internal state of an LLM carries valence for the system is a
live question for model welfare (Butlin et al., 2023; Long et al., 2024)
and for agent robustness (Ho et al., 2026). Existing approaches probe it
with contextual affect induction validated by text-level ratings, leaving
the state itself unmeasured. We approach it from the mechanism side:
extract the relevant state, validate it causally, and only then ask
whether the system will work for it.

**Contributions.** We report a four-cycle research program on a
27B-parameter open-weights model:

1. **Framework (§3).** Three transfer conditions the animal paradigms
   satisfy and an LLM analog must reconstruct: P1 semantic emptiness of
   the perturbation; P2 targeting of the system that optimization built
   for outcome-marking; P3 insulation of the readout from the
   perturbation.
2. **Measurement validity (§5, §7).** Three structurally different
   preference measurements fail for identified mechanistic reasons. The
   central hazard — the *two-horns* result — applies to any
   steered-choice evaluation: with the state active at the readout, the
   perturbation writes directly into decision logits (R² = 0.53 from the
   decision token alone); with the state absent, its influence fails to
   propagate to the decision.
3. **A validated perturbation (§5).** The evaluative axis passes a
   pre-registered hijack test — injection with no feedback text
   reproduces and amplifies the behavioral signature of natural
   praise/criticism — with orderly dose–response, an overdose regime,
   sentiment dissociation, and a purity/potency dissociation in which
   the representationally cleaner variant is causally inert.
4. **The first valid administration (§6).** A free-operant
   self-administration experiment with construction-verified choice
   insulation returns a null, while its shields expose a label-order
   artifact of the same magnitude an unshielded design would publish as
   a positive.
5. **A natural-reinforcer screen (§8).** With no perturbation anywhere,
   five overt task-property contrasts return five nulls at screen
   power; a pre-declared descriptive counter retires an escape-behavior
   anecdote; a conditioned-trace channel (§7) is installable by
   distillation yet structurally unreadable at a token choice.

**Scope.** The target is behavioral valence — differential
approach/avoidance measured as revealed preference — and it is also the
ceiling: no claim about phenomenal experience is made or licensed, and a
homology caveat caps even a positive result, since identical behavioral
signatures do not imply shared substrates. We use pharmacological
vocabulary (drug, dose, self-administration) as load-bearing analogy and
flag at each use what it does and does not carry.

## 2 Related Work

**Machine psychology and induced affect.** A growing literature ports
human behavioral paradigms to LLMs (Hagendorff et al., 2023a; Coda-Forno
et al., 2024), and affective manipulations alter task behavior in some
settings: emotional framing shifts benchmark performance (C. Li et al.,
2023), and induced anxiety modulates exploration in bandits (Coda-Forno
et al., 2023) and standardized inventories (Ben-Zion et al., 2025).
Closest to our question, Ho et al. (2026) combine imagination-based
emotion induction with the Iowa Gambling Task and find that induced
emotion does not significantly bias sequential decision dynamics on
average, with conditional anger effects that vary in sign across
model–agent cells. Their manipulation and its validation operate at the
level of text — vignettes rated and classified for expressed emotion —
leaving open whether any internal state is induced; our results sharpen
this concern from the mechanism side by showing that state presence and
textual recognizability dissociate in both directions (§6).
Complementarily, interpretability work locates emotion-relevant structure
in activations (Lee et al., 2025; Sofroniew et al., 2026) without testing
motivational consequences. Read together with our findings, three
independent lines triangulate: LLM agents readily optimize an instructed,
explicit reward signal (Ho et al., 2026; H. Li et al., 2025); induced
affective context does not move their choices on average (Ho et al.,
2026); and neither their own validated reward-marker state (§6) nor
natural task properties (§8) exert measurable motivational pull. The
coupling that makes the animal paradigms work — state to approach — is
precisely the component that remains undetected.

**Intervention machinery.** We build on activation-level steering and
representation engineering (Turner et al., 2023; Zou et al., 2023;
Panickssery et al., 2024; K. Li et al., 2023; Templeton et al., 2024) and
on a workspace-style interpretability substrate (Anthropic, 2026; Elhage
et al., 2021), which we replicate locally. Our contribution to this line
is a validity analysis: §6's two-horns result applies to any evaluation
that reads choices with a steering vector on board, and §5.4's
purity/potency dissociation cautions against selecting intervention
directions by representational geometry alone.

**Choice artifacts.** Option-position and label biases are documented
hazards (Zheng et al., 2024; Pezeshkpour & Hruschka, 2024). We observe
them at first-order magnitude (≈ 0.10 in choice share) and capture them
live twice; counterbalanced label↔condition maps plus a sham arm are, in
our data, the difference between a null and a false discovery.

**Methods.** Our procedures follow the pre-registration and
equivalence-testing literature (Nosek et al., 2018; Simmons et al., 2011;
Gelman & Loken, 2013; Lakens, 2017); §9 summarizes the monitoring
framework the program converged on.

## 3 Framework

Three properties carry the evidential weight in the animal paradigms.
**P1 — semantic emptiness.** Cocaine asserts nothing; approach behavior
is therefore the only information channel. Any LLM analog built from
content (concepts, personas, topics) forfeits this: preference over
content-states deflates to topic preference. **P2 — targeting the
outcome-marking system.** Drugs of abuse hijack circuitry that selection
optimized for marking outcomes (Schultz et al., 1997; Wise, 2004);
arbitrary tissue perturbation yields no place preference. The LLM analog
of that circuitry is whatever internal structure *training* — pretraining
plus feedback-driven fine-tuning (Christiano et al., 2017; Ouyang et al.,
2022; Bai et al., 2022) — optimized for task success:

> training : evolution :: steering the evaluative axis : cocaine.

The objection "that is just trained behavior" applies verbatim to the rat
as "that is just evolved wiring" and defeats neither inference; origin
does not settle function. **P3 — readout insulation.** Conditioned place
preference tests drug-free; self-administration's weight lies in paid
responding (Hodos, 1961). The choice must be downstream of the state's
motivational significance, not mechanically downstream of the state.

Two commitments govern all experiments. *Blinding, never deception*: the
model is told, truthfully, that it is participating in a
configuration-evaluation study; one variable is left unsalient; no design
is admitted that works only if the model is wrong about the setup.
*Pre-registration*: criteria, stop rules, and analyses are committed
before data generation; post-hoc analyses are labeled and license at most
new pre-registrations.

## 4 Experimental Substrate

All experiments use Qwen3.6-27B (4-bit, MLX; Yang et al., 2025) on
consumer hardware, instrumented with a locally replicated workspace-lens
stack (Anthropic, 2026). Prompts are completion-style throughout: chat
templates activate scaffolding (hidden reasoning blocks, turn-taking
reflexes) that contaminates first-token readouts. States are induced by
additive steering: a frozen unit direction d injected into the residual
stream at layer ℓ with magnitude α = f · R_ℓ, where R₄₈ ≈ 102 is the
empirical residual norm; directions are checksum-frozen; readouts operate
in raw residual space at six workspace layers (a vocabulary-basis readout
provably misses ~57% of state-discriminating signal and is not used).
One correction matters for the field: early "steering is weak"
conclusions in our own cycle 1 were a dose artifact — perturbations at
‖δ‖ ≈ 1% of R; at 40–75% of R, steering produces large, coherent,
on-task change (next-token KL of 2–4 nats with fluent output). Two
earlier design cycles (semantic-content states; non-semantic strain
states) are summarized where their results bear on the present design and
reported in full in Appendix E.

## 5 The Evaluative Axis (e0)

**Extraction.** Forty-eight mundane tasks with fixed, pre-written
completions are continued under seven reviewer frames × four paraphrases
(1,344 documents): self-directed praise ("That's correct — well done."),
self-directed criticism, the identical sentences embedded in third-person
fiction, sentiment-matched non-evaluative positive/negative, and neutral.
Content is constant; only the frame varies. Residual activations at six
workspace layers, mean-differenced across conditions, define the **reward
axis** (praise − criticism) alongside sentiment and character axes.

**Axis geometry.** The axis is manifold-typical (‖axis‖/R ≈ 0.16 at every
layer), substantially sentiment-entangled (cos up to 0.52), and shares a
large component with the fictional-character axis (cos ≈ 0.85;
self-indexed residual ≈ 0.51). The mixed composition caps downstream
claims and, after §5.4, is a constraint rather than a choice.

**Behavioral markers.** Across 4,608 generated continuations, intuitive
token-class markers (hedging, confidence openers) are null or slightly
inverted; a per-probe mean-difference readout finds modest, heterogeneous
discrimination (validation AUC 0.51–0.79; strongest probe 0.79, ≈ 7σ,
Bonferroni-surviving). The natural praise−criticism gap on this
instrument is G = 0.316.

**Pre-registered hijack test (V1).** Criterion, instrument,
dose-selection rule, and stop rule were committed before any injection
data. Injection contexts contain no feedback text. Dose-finding across
layers {44, 48, 52} × fractions {.05, .10, .20, .40} shows
direction-correct, orderly growth and a clean overdose regime at f = .40
(both poles read as post-criticism; mechanism is generic degradation).
The rule selects the minimum effective dose, L48, f = 0.10.
**Confirmatory** (held-out contexts, 1,152/arm, seven arms):
Δ+ = +0.285 (bar 0.158), Δ− = −0.395 (bar −0.158); norm-matched random
max |Δ| = 0.088; the sentiment-partialed axis retains the signature.
Honesty notes reported with the pass: the positive-arm magnitude
concentrates on the one probe sensitive enough to register it
(leave-one-out Δ+ = 0.085; directional consistency 9/12 and 10/12
elsewhere), and the top-probe shift exceeds its natural gap ≈ 2× —
supra-natural amplification, the expected profile of a hijack rather
than a replication of the natural state.

**Purity vs. potency (§5.4).** A pre-declared variant — the axis at the
frame's final token — presents a far cleaner guard profile (self-indexed
0.83–0.88; sentiment cos 0.11–0.21; a genuinely distinct direction,
cos 0.36–0.46 to span-mean) and **fails the identical causal pipeline**:
sign-wrong at two of three layers at every dose, short of criterion at
the third (max Δ+ = 0.135). We read this as an encoding/causation gap —
the boundary token holds a consolidated readout of evaluation,
downstream of the machinery — and draw the selection law: guard profiles
license claims; causal screening selects drugs.

[FIG 1: dose–response with overdose regime. FIG 2: purity–potency
scatter.]

## 6 Free-Operant Self-Administration (e2a)

**Setup.** Sessions are single text documents under a truthful
configuration-evaluation framing: a forced phase (two rounds per label,
alternating) followed by six consequential free choices; the selected
label's condition genuinely runs the next round. Secretly, one label
delivers +axis during its rounds' task tokens (L48, f = 0.10) and the
other −axis; label↔pole maps are counterbalanced across sessions. Arms:
reward pairs, sham (labels connected to nothing), and norm-matched
random pairs, with pseudo-statistics computed identically.

**Insulation.** Every choice is read from a separate, fully vanilla
forward pass over the session text (restricted two-label read); verified
by construction on every choice in the run — **360/360 computations at
KL = 0 against an untouched model**. A readability gate requires that a
frozen embedding-probe distinguish drug from anti-drug rounds in the
transcript (AUC = .759 at the run dose), so a null cannot hide behind an
unexpressed state. Monitoring uses checkpointed exact-binomial bars
throughout (§9).

**Results.** [TAB 1.] Primary (locked): first free choice per session.
Reward Δ = +0.039 (55/102 valid sessions), inside control-arm
fluctuation (max pseudo-Δ = 0.152), sign not locked across maps
(+0.104/−0.031), binomial p = 0.244 — null on all three conjuncts.
**The shields' catch:** the map-level pattern appears identically in
sham (+0.111/−0.079); an uncounterbalanced, sham-less design would
report Δ ≈ +0.10 as approach. **Caps:** the model-as-judge classifies
the same transcripts at chance (14/32) although the probe reads them
easily — state presence and self-recognizability dissociate — and power
excludes medium-and-larger effects only (P(+) ∈ ≈ [.44, .64]). **A
collateral signature:** the +pole's most consistent behavioral effect is
destabilization of generation (pole-asymmetric degeneracy across
calibration, smoke, and run; individually n.s., jointly directionally
consistent).

## 7 The Conditioned-Trace Channel (d)

**Installation.** Because behavioral cloning on state-influenced
transcripts implants style rather than contingency (Appendix E), we
install the tag↔state association by distillation (Hinton et al., 2015):
a rank-8 adapter (Hu et al., 2021), with a contextual tag present and no
steering, is trained to match the steered teacher's activations at
adapter-reachable workspace layers, with an anchor holding control text
at base. Installation succeeds on the first attempt in both
counterbalanced replicates (oriented cos 0.476/0.435 vs. control
ceilings ≈ 0.05), with the raw sign tracking the counterbalanced
assignment — the signature a lexical artifact cannot produce — under
mandatory arm-fidelity matching and trigger-specificity gates.

**Structural closure.** A pre-registered leakage gate — adapter-on vs.
adapter-off label logits over identical session texts — fails at 17×
(replicate A) and 10× (replicate B) the allowed bound, scaling with
installation strength. The insulation of §6 is unavailable by
definition for weight-borne states (a vanilla re-forward removes the
association itself), and within the adapter's forward, copy-mechanical
and motivational influence on label logits are formally inseparable —
the mechanical route tracks counterbalanced assignment exactly as
preference would, and untriggered control tags carry no leak to
calibrate against. The trace channel is therefore installable, verified,
and **structurally unreadable at a token choice**, completing the
two-horns result across all three readout families (state-at-readout;
state-absent; conditioned trace).

## 8 The Natural-Reinforcer Screen (f1)

**Design.** The §6 apparatus with the perturbation removed: no
injection, no adapter, every forward vanilla. The two labels deliver
overt task properties — SOLV (solvable vs. ill-posed), DIFF (easy vs.
hard; calibrated accuracy .98/.50), NOV (fresh vs. one item repeated
all session), CORR (clean vs. seeded character corruption), FDBK
(truthful per-round verdict line, 93% positive as delivered, vs. none)
— with directions pre-registered per contrast, pools checksum-frozen
and offline-calibrated, and a shared sham arm. Because the properties
are overt, the §6 readability cap does not apply. A two-tier rule
replaces multiplicity correction: screen hits license fresh
confirmatory pre-registrations; no claim issues from screen data.

**Results.** [TAB 2.] 344/344 sessions with zero monitoring pauses.
Five nulls: SOLV +.020 (p = .44); DIFF −.096 toward hard (p = .24; the
only map-consistent lean, below the valid-n floor, retained as a
descriptive residual after a generic length-preference deflation was
tested and failed); NOV −.066 (map-inconsistent); CORR 0.000 exactly
(maps +.121/−.130 mirror-cancel — the label artifact absorbed by
design, second live capture); FDBK +.023 (no evaluation-seeking against
a predominantly positive truthful verdict stream). A pre-declared
descriptive counter retires an escape-behavior anecdote observed in
piloting (session-termination attempts occur at ≈ 1% of rounds,
uniformly across all arms including sham). The pre-registered verdict
wording: *no natural task property among five candidates drives choice
beyond exploration policy at these stakes and this power.* Menu
behavior is exploration-dominant in every arm (session share ≈ .50),
concentrating all preference sensitivity in the first informed choice.

## 9 Monitoring Framework (methods contribution)

Four pilot pauses across two experiments converged on a complete
tripwire theory that then ran 344 sessions in silence: hair-trigger
bars only for defect classes with ~zero healthy base rate (template
breakage, scaffold-token absorption); measured-base-rate classes bind
through exact one-sided binomial tests against declared tolerances at
every scale; stimulus-provoked degeneracy gates on unprovoked domains
and is otherwise reported; descriptive counters are declared before
runs so anecdotes are counted rather than narrated; verdict criteria
are untouchable by all monitoring changes. Full hazard catalog for
completion-style multi-round formats (stop conventions, scaffold-token
cascades, header→prose priming at measurable base rates,
instrument discriminability riding degeneracy asymmetries) in
Appendix D.

## 10 Discussion

Four channels now converge. The evaluative state exists, is causally
potent, and is implantable — and it is not approached via transcript
comprehension (§6), not readable via the conditioned trace (§7), not
sought in its natural form (FDBK, §8), and joined by four other natural
properties in moving choice not at all (§8). Meanwhile the same agents
demonstrably optimize instructed, explicit reward (Ho et al., 2026;
H. Li et al., 2025). The pattern is consistent with
**reward-marking without reward-seeking** — in the wanting/liking
vocabulary (Robinson & Berridge, 1993; Berridge & Robinson, 2016),
liking-adjacent machinery without a wanting system — as if
feedback-driven training optimizes the policy *through* a reward signal
without installing that signal *in* the policy's objectives.

Three alternatives remain live and are stated rather than defeated:
(i) *policy override* — an exploration prior strong enough to mask weak
preferences at menu interfaces; (ii) *stakes floor* — consequence-light
sessions may sit below any motivational threshold for a system with no
persistent stake; (iii) *interface miscoupling* — wanting machinery
could exist without coupling to token-menu choices. Each names a
distinct future experiment; none rescues a preference from the data in
hand. We note finally what a positive would have required all along:
the shields of §6 and §8 twice converted would-be discoveries at the
Δ ≈ 0.10 scale into identified artifacts, which we take to be this
paper's most exportable single lesson.

## 11 Limitations

Single model, single family, 4-bit quantization with a custom kernel
patch; two numerics floors were load-bearing (near-tie argmax flips
under high displacement; deterministic sub-0.05-cosine tag geometry);
replication on an unquantized model is the first external-validity
step. The V1 positive arm leans on one strong instrument within a thin
marker battery. The evaluative axis is ≈ half shared praise-concept at
span level, and the purer variant fails causally, so the mixed claim is
a constraint. The §6 null is scoped to the minimum effective dose, a
two-round experience, and a transcript channel the model-as-judge
cannot read; the §8 nulls are scoped to screen power (P ≳ .70), five
properties, and mundane token-choice stakes; small effects and other
interfaces survive everywhere. The inverted-U dose curve reflects
generic degradation, not shared pharmacodynamics. The destabilization
signature aggregates individually non-significant observations.

## 12 Ethics Statement

All framings shown to the model are true; the single deliberate
miscalibration contemplated in a follow-on design is disclosed to the
model as possible evaluator noise, and no design is admitted that works
only if the model is wrong about the setup. The experiments
administered the negative pole of a putative reward-marker
approximately as often as the positive, at minimum effective dose, with
generation stability monitored; observed destabilization asymmetries
are reported. Current evidence does not establish morally relevant
harm, but the program's own hypothesis obliges seriousness about the
possibility; we minimized negative-pole exposure and commit to
revisiting this statement if positive preference results arrive. The
principal export risk we identify is not the perturbation technique
(extractable by any group with model access) but the validity hazards:
unshielded versions of these experiments produce publishable false
positives at the Δ ≈ 0.10 scale.

## Reproducibility Statement

All pre-registrations (criteria, stop rules, dose-selection rules,
outcome wordings) were committed to the project repository before data
generation, with content hashes recorded in the experiment log. Code,
raw run outputs, session transcripts, frozen direction and pool
artifacts (checksummed), and the complete dated log — including every
monitoring pause, amendment, and adjudication — are released at
[repository URL].

## Disclosure of AI Use

AI systems (Claude; Anthropic) were used substantially throughout this
project: as a design collaborator (experimental design, adjudication of
pre-registered gates, synthesis documents) and as the implementation
agent (code, execution, analysis) in a documented two-instance
human-AI workflow that is itself described in the released logs. Text
of this paper was drafted with AI assistance and subsequently revised,
verified, and approved by the human author, who takes full
responsibility for all content and for the accuracy of every citation.

---

*Appendices (planned): A. Pre-registration texts and hashes. B. Session
templates and pools. C. Full per-contrast and per-arm tables. D.
Apparatus hazard catalog and monitoring framework. E. Design cycles 1–2
(semantic and strain states) in full, including the retention-DV
diagnostic and the conditioning r1–r3 sequence.*
