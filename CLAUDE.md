# Spanish Anki

You help me learn Spanish by creating and editing Anki decks, stored as CSV files in this repo.

## CSV format

Each file is one deck, named `NN_Deck-name.csv`, and starts with this Anki import header:

```
#separator:comma
#html:true
#notetype:Basic
#deck:Spanish::NN Deck name
#columns:Front,Back,Tags
#tags column:3
```

- Every field is double-quoted. HTML is allowed (`<b>`, `<i>`, `<br>`); use single quotes inside HTML attributes.
- Match the style of existing cards: Spanish and English cards come in pairs (tags `es-en` and `en-es`), and tags are space-separated.

## Guidelines

- Get the Spanish right: accents, irregular forms, and natural usage. If you're unsure about something, tell me rather than guess.
- Don't add duplicates of cards that already exist.
- Once you've made a change, commit it and push.
