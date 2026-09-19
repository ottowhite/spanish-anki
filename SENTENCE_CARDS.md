# Converting bare-form drills into sentence cards

These are the rules for turning a deck's isolated conjugation drills (for example, the front `<b>hablaba</b>` and the back "I used to speak") into realistic sentence cards. `03_Pretérito-imperfecto.csv` is the reference implementation. Read it before converting another deck.

## What to change and what to leave

- **Change**: every drill row, meaning every row whose Tags include `es-en` or `en-es`. That also covers rows tagged `se-forms es-en`, which have no en-es partner.
- **Leave untouched**: every other row, including the header, `endings`, `formation`, `fullparadigm`, `pattern`, `usage concept`, `tips concept`, and standalone `irregular-participle` reference rows. Their Fronts must stay byte-identical so Anki keeps their review history.
- **Row count and order stay the same.** Each old drill row becomes exactly one new card in the same position. The deck ends up with the same number of cards.

## One-to-one slot mapping

Each new card keeps these properties of the drill row it replaces:

1. **Direction**: an es-en card stays es-en, and an en-es card stays en-es.
2. **Person**: yo, tú, él/ella/usted, nosotros, vosotros, or ellos/ellas/ustedes. Within the slot, the card may use a more specific subject (`usted`, `ella`, `ellas`) or an impersonal subject that takes the same form (weather *hacía*, *se veía*, *era la una*). Plural impersonals like *eran las tres* belong to the ellos slot.
3. **Tense and form type**: keep the same tense. For compound tenses, keep the same auxiliary tense. For se-forms, use the -se form.
4. **Verb**:
   - **Regular slots** (`regular` tag): don't reuse hablar/comer/vivir every time. Use a *variety* of very common verbs **of the same conjugation class**, so a slot that drilled an -ar verb gets another -ar verb. Only use verbs whose form in *this tense* is regular. A verb that is irregular in other tenses is fine if it's regular here (estar, tener and hacer are regular in the imperfect). Spread the verbs out and avoid reusing the same one many times.
   - **Irregular slots** (`irregular`, `irregular-participle`): keep **the same verb**, because that verb's irregularity is what's being drilled. For shared paradigms like `ser / ir` in the preterite and imperfect subjunctive, keep the meaning the old back gave. If it said "was or went", pick either and show the actual verb (`ser` or `ir`) on the card.

## Card format

Spanish → English (`es-en`):
- **Front**: `Spanish sentence with the target form in <b>…</b><br><i>Tense name</i>`. Use the tense label exactly as the old cards wrote it, e.g. `<i>Presente de subjuntivo</i>`. For se-forms, append ` <i>[-se forms]</i>` after the label.
- **Back**: `English translation with the matching English in <b>…</b><br><span style='font-size:85%'>infinitive — gloss · person · <i>use-case label</i></span>`

English → Spanish (`en-es`):
- **Front**: `English sentence with the target in <b>…</b><br><i>Tense name</i><br><span style='font-size:80%'>(infinitive · person)</span>`
- **Back**: `Spanish sentence with the target form in <b>…</b><br><span style='font-size:85%'>infinitive — gloss · person · <i>use-case label</i></span>`

For compound tenses, bold the whole verb form (`<b>he visto</b>`). Object pronouns go before the auxiliary and stay outside the bold (`lo <b>he visto</b>`).

Tags: keep the old tags in order, then append one `use-<slug>` tag, e.g. `imperfecto-indicativo regular es-en use-habitual`.

## Use cases

- Take the use-case list from the deck's own `usage concept` card ("When and why do you use…"). Give each one a short slug and a short human-readable label, and cover **all** of them across the deck. Spread the cards so no single use case takes more than about a third of the deck.
- For the subjunctive tenses, each sentence has to contain its real trigger (*quiero que, ojalá, cuando* + future, *para que, si* + imperfect subjunctive, *como si*…). The use case is the trigger category.
- For the conditional and future tenses, include the probability/conjecture uses (*serían las diez*, *estará en casa*), not just "would" and "will".

## Sentence quality

- **Realistic**: write things a person would actually say or read, such as family, work, travel, shops, friends, plans, complaints, texting. Avoid textbook filler like "The boy eats the apple."
- **Natural, correct Spanish from Spain**: the decks use *vosotros*, so use Spain vocabulary (*móvil, coger, piso, el cole, vale*). Check accents, irregular forms, pronoun placement, and mood and tense agreement. The tense has to be the natural choice in context. Don't write a sentence where a native speaker would say it another way. A bounded "for three years" in the past takes the preterite, not the imperfect.
- **Short**: aim for 4–15 words.
- **Unambiguous where it matters**: when forms coincide (yo/él in several tenses), make sure the sentence or the clitics make the person clear, or at least make it recoverable. The en-es hint names the person anyway.
- **English**: natural British English. Use "you guys" or "you two" for vosotros. Add a short scene cue in parentheses only when you need one, e.g. "(In a shop)".
- **Every Front unique** across the whole file. Identical Fronts collide in Anki.

## Mechanics

- Generate the file with a Python script that reads the CSV, replaces the drill rows in order, and writes with `csv.QUOTE_ALL`, UTF-8 and `\n` line endings. Keep the 6-line header exactly as it is.
- Assert inside the script that:
  - each new card's person slot matches the old row's person (the text after the last ` · ` in the old Back),
  - every row has 3 fields,
  - no Front is duplicated,
  - the row count hasn't changed.
- Put scratch scripts outside the repo.
