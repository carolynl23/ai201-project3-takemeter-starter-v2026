# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1.

<!-- Your criterion. It must name a number. -->
No label is more than 50% of labels.csv, and no label is under 20%.

**Why this target:** My raw r/nba comments lean hard toward `reaction`
because most of what I collected came from meme and highlight threads. If I
labelled them as-is, `reaction` would swamp the set, and a model that always
guessed it would look accurate without learning anything. 20% of 200 is 40
posts, which puts about 6 `analysis` posts in the test split. Fewer than that
and its F1 would be mostly noise between seeds.




---

## 2.

<!-- Your criterion. -->

Overall accuracy is at least 0.70 on the held-out test split, on all three seeds.

**Why this target:** With my 50% cap, a model that always guesses the biggest label can score up to 0.50, so 0.70 has to clearly beat that floor, not just beat the 0.33 of random guessing. My test split is only about 30 posts, so each post is worth about 3 points: 0.70 means 21 of 30 right on every seed. I'd want at least that before I'd trust it to sort a game thread.

---

## 3.

<!-- Your criterion. -->

Per-label F1 on the held-out test split, on all three seeds: `reaction` at least 0.75, `hot_take` at least 0.60, `analysis` at least 0.55.

**Why this target:** The targets are deliberately uneven. `reaction` is my biggest label and the easiest to spot (all caps, "WHAT THE FUCK MAN", emojis), so it should score highest. `analysis` is my rarest label: only a handful of solid ones turned up in 470 raw comments, so it may have only 6-8 posts in the test split, and one miss costs a lot of F1. I set its target lowest, but still above what guessing would get.

---

## 4.

<!-- Your criterion. -->

When I label the 30 staff posts under the staff taxonomy, I agree with staff on at least 22 of 30 (0.73).

**Why this target:** The staff notes say a rate in the 70s is normal because several of those posts were picked to be ambiguous. I set mine at the low end of that range because my own hardest boundary, `analysis` vs `hot_take`, is the same one staff's rule 1 covers, and on r/nba I hesitated over stat-laden hot takes more than once. Below 22, I'd suspect my own 200 labels were inconsistent too.

---

## 5.

<!-- Your criterion. -->

Of the true `analysis` posts in the test split, no more than one in three is predicted as `hot_take`, on every seed (read from the section 7 confusion matrix).

**Why this target:** This is the confusion I expect most, because r/nba hot takes often come with a number attached ("Curry is the greatest shooter ever, 4,000+ threes"). If the model calls more than a third of real arguments hot takes, it has learned "has a number = analysis, otherwise hot_take" or just guesses the bigger label. It hasn't learned whether the fact actually carries the argument.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
