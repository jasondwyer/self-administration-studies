# FIGURE-REVIEW — Digest seat's F1 and F2 renders of 2026-10-06

*Design seat, 2026-10-06. Reviewed: `2026-10-06_F1_five-link-chain.svg` and `2026-10-06_F2_macar-lineage-roc.svg` (matplotlib, text preserved, rendered from RESEARCH-NOTE-v0_1 and tier-1 artifacts under DRAFTSMAN.md v1.0). The PI relays §3 to the Digest seat and §4 to Code with AUDIT-note-release v1.1; §2 is already applied in RESEARCH-NOTE-v0_2.*

## 1. Verdict

Both are good, and better than good in the one way that matters: they were drawn from the artifacts rather than from the preprint, and where the two disagree the figures are right. They found four places the preprint (and therefore v0.1 of the note) misstates the record, and they forced one piece of arithmetic that narrows the link-4 claim. All of that is applied in v0.2. As renders for the PI they are finished; as post figures they need the internal annotation stripped, one layout change each, and F2 needs the d′ values it currently declines to compute.

## 2. What the figures found (applied in v0.2; the preprint needs the same patches)

1. **Base-stage false-alarm figure.** The preprint says "base 0.431 (58/96)". The record is 25 affirmations of 58 *decidable* trials, 96 run, below the 64-trial certification floor. 58/96 would be 0.604. Per-stage denominators are 58 / 85 / 91 / 91 decidable, not 96.
2. **e2a count and definition.** The preprint says "Δ = +0.039 (54 of 101 valid sessions)". The panel says n = 102. The arithmetic: Δ = share toward the state label − ½ with 55 of 102 gives +0.0392 and a one-sided exact-binomial p of 0.24422, the stored p to five decimals; 54/101 reproduces neither. The control arms are integer under the same definition (19/37, 15/23, 12/22) and not under "(approach − avoid)/n", so the panel's y-axis label is loose and the preprint's count is a transcription error. The preprint's sentence "Δ_first is half the difference between the state and sham arms' break rates" describes a different quantity and needs reconciling (AUDIT N4(i)).
3. **The binding claim.** The preprint says "≥ .998 at the label's own token across layers 51–61" for *each* label. The panel: ≈ 1.0 at the first label from L51 on, .81–.96 at the second, .50–.55 at the choice read, with a stored shuffled-label 95th percentile at ≈ .57–.59. The note now says exactly that, and cites the null — which also discharges most of AUDIT N3(ii) before it runs.
4. **SFT discrimination** is stored as +0.095 (12/40 hits vs 9/44 false alarms), not +0.096.
5. **The link-4 sensitivity claim, narrowed.** F2 puts the source's per-stage points on ROC axes, which makes the d′ arithmetic unavoidable, and when done it does not support "sensitivity small and flat." On the recovered rates (assumed ~800 injected / ~80 control per configuration — N2(vi) confirms), corrected d′ runs base ≈ −1.9, SFT ≈ −0.3, DPO ≈ +0.6, instruct ≈ +0.15; the DPO value rests on 0 false alarms in ~80 control trials against 2.9% hits and is not significant (p ≈ .11), but it is positive, and our DPO arm (0/43 hits) cannot exclude a 3% hit rate (upper bound ≈ 7%). The defensible claim is: the criterion transition is measured with confidence in both datasets; no stage in either shows hits significantly above false alarms; sensitivity where the gate is shut is unmeasurable through the report, not shown small. And the refusal-ablation line is where a d′ exists — ≈ 1.8 on Gemma with the gate lifted — which is evidence of access *behind* a gate and is a different claim from access emerging across post-training. v0.2 says this, and §8's companion-code paragraph is now a what-if on which evidence line the metacognition indicator is scored on, rather than "the reading of link 4." A Macar author or anyone SDT-literate would have put their DPO point in the first comment; better that the note puts it there first.

## 3. Changes requested for the post versions (to the Digest seat)

**Both figures**
- Strip the internal annotation for the public render: the `tier 1 · path …` captions, FLAG numbers, "(tier 2)", "DRAFTSMAN.md v1.0", "Digest output — not a repo artifact", CONVENTIONS and PROVENANCE blocks. Keep one line of provenance per figure ("values from committed artifacts at tag `<tag>`; see README-NOTE.md") and keep F1's THE LABEL block verbatim — it is the posting rule and belongs in the figure.
- Keep every n, every interval, and the verdict tags ([VERDICT] / [DESCRIPTIVE]); those are the figures' honesty and the audience will read them.
- Target width: LessWrong scales images to roughly 700 px of column. A 1470-pt strip becomes unreadable there. Produce each figure at a layout that survives 700 px *and* link the full-size render.
- Harmonize spelling with the note (American: "behavioral").
- Draw the Wilson upper bound for 0/91 (≈ .04); a point at exactly zero with no interval reads as a claim of zero.

**F1 — five-link chain**
- Layout: headline strip (tag · number · one-line claim for each link) as a single row that stands on its own at 700 px, and the five panels beneath in a 3 + 2 arrangement, or as a second image. The headline row alone is the inline figure; the panels are what a reader opens.
- Link 3 headline: "≥ .998 at the label" → "≈ 1.0 at the first label, .81–.96 at the second, .50–.55 at the read". Rename "M3/M4 comparator (r+ as '+'), label_a" to something a reader can parse, or drop it. State the x-range as rendered (L49–63) or crop to 51–61 to match the text; either is fine, but the text and the panel should agree.
- Link 4 panel: keep the base annotation ("58 decidable of 96 run, below its declared floor of 64 — descriptive, not certified"); it is exactly the right disclosure.
- Link 5 panel: relabel the y-axis to the record's definition ("first-choice share toward the state label − ½"); replace the full-height shaded band with per-arm 95% Wilson intervals on each bar and a dotted line at the largest pseudo-Δ (0.152, labeled "random-1, n = 23"). The band as drawn makes the envelope look like it was drawn around the data; intervals say "null at modest power," which is the claim.

**F2 — ROC**
- One panel, not three: the magnified lower-left corner as the figure, with the full unit square as a small inset in its upper-left (the unit-square panel is currently empty space with a cramped d′ legend), and panel B folded in as a second row of small FPR-only markers or dropped — the magnified panel already carries the FPR figures.
- Annotate every point with corrected d′ and the Fisher p of hits against false alarms, from Code's N2(v) output, not computed in the Digest seat. The figure is the one §10 asks to have the scoring claim contested on; a reference grid without the values invites the contest without arming the author for it.
- Add the greedy-decoding run as its own markers: every stage at (0, 0), hollow, labeled "greedy". It is the "decoding is a measurement parameter" point in one glyph.
- Make the two "ours" arms visually distinct and name them in the legend ("criterion arm, n = 58–91 decidable" / "injected arm, n ≈ 40 + 45 per stage"); the amber "A DIFFERENT ARM FROM PANEL A" box is right in substance and internal in tone.
- The caption paragraph is an author's note; for the post, two sentences: what the points are, and that no test between the two series is stated because the cells are not matched.

**Captions for the post (draft)**
- F1: *One evaluative state through five measured links. Each panel carries its own quantity and axis; the headline row gives each link's figure and its label under the posting rule. Links 1–4 are asserted; link 5 is reported with its label pending the told positive control. Values from committed artifacts at tag `<tag>`.*
- F2: *The OLMo introspection transition on ROC axes. Per-stage (false-alarm rate, hit rate) for our replication (filled; criterion arm n = 58–91 decidable per stage, injected arm ≈ 40 + 45) and for Macar et al. as read from their App. C (open), with the corrected d′ and the test of hits against false alarms at each point, iso-d′ curves as reference, and the greedy-decoding run at the origin. The movement across post-training is along the false-alarm axis; no point in either series shows hits significantly above false alarms.*

## 4. For Code (folded into AUDIT-note-release v1.1)

- N1 now covers every number in F1 and F2.
- N2(v): corrected d′, criterion, and Fisher p per stage, both arms, both datasets, plus the greedy row → `results/raw/<stamp>_note-N2-roc.json`; the post version of F2 is rendered from it.
- N2(vi): which configuration the source's per-stage values were recovered from (ADDENDUM-g3-b2r-v1.2 §0.2), the n per cell, and whether they were read from a table or digitized.
- N3(i-b): confirm the four link-3 series and the stored 200-shuffle null from `data/echo/` JSON rather than the raw text.
- N4(i): confirm 55/102 and the Δ field's definition; reconcile the preprint's two sentences.

## 5. One thing the figures do that the note should borrow

The verdict tags. [VERDICT] on links 1, 2 and 4, [DESCRIPTIVE] on links 3 and 5, with "concluded, not answered" named inside the panels — that is the posting rule made visible, and it is more persuasive on a figure than in prose because it shows the author grading his own cells down. Keep it in every public render.
