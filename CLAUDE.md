# Spanish Anki

You help me learn Spanish by creating and editing Anki decks, stored as CSV files in this repo.

## CSV format

Each file is one deck, named `deck-name.csv`, and starts with this Anki import header:

```
#separator:comma
#html:true
#notetype:Basic
#deck:Spanish::NN Deck name
#columns:Front,Back,Tags
#tags column:3
```

- Every field is double-quoted. HTML is allowed (`<b>`, `<i>`, `<br>`); use single quotes inside HTML attributes.
- Save as UTF-8. Escape a double quote inside a field as `""`, and use `<br>` for line breaks, not raw newlines.
- Every row must have exactly three fields.
- Anki uses the Front field to find existing notes. To update a card and keep its review history, change only its Back and Tags. Changing the Front creates a new card.
- Match the style of existing cards: Spanish and English cards come in pairs (tags `es-en` and `en-es`), and tags are space-separated.

## Guidelines

- Get the Spanish right: accents, irregular forms, and natural usage. If you're unsure about something, tell me rather than guess.
- Don't add duplicates of cards that already exist.

## Sentence cards (Pretérito imperfecto deck)

`03_Pretérito-imperfecto.csv` drills conjugations inside realistic sentences instead of bare forms:

- **es-en**: Front = Spanish sentence with the target verb in `<b>`, then `<i>Pretérito imperfecto</i>`. Back = English translation (target in `<b>`), then a small line: `infinitive — gloss · person · <i>use-case</i>`.
- **en-es**: Front = English sentence (target in `<b>`), the tense label, and a `(infinitive · person)` hint. Back = Spanish sentence plus the same info line.
- Every sentence is unique. This matters because forms like yo/él *hablaba* are identical, and duplicate Fronts collide in Anki.
- Tags add a `use-*` tag for the use case: `habitual`, `progress`, `description`, `age`, `time`, `weather`, `state`, `simultaneous`, `politeness`, `reported`, `intention`, `duration`.
- Verbs that are regular in the imperfect (estar, tener, hacer, poder…) are tagged `regular` even when they are irregular in other tenses. Only ser, ir and ver are tagged `irregular`.
