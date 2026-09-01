# Papermerge Locales

User interface translations for [Papermerge](https://papermerge.com) — the
open-source document management system.

This repository holds **only the translation files**. It is intentionally small
and dependency-free so that anyone can improve a translation, or add a new
language, with a single pull request — no need to build or run Papermerge.

Translations from this repository are bundled into the Papermerge frontend and
served from **User menu → Preferences → Interface Language**.

## Repository layout

```
localization/
  en/_default.json   ← source language (canonical key list)
  de/_default.json
  es/_default.json
  fr/_default.json
```

Each language is a directory named with its
[ISO 639-1 code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes),
containing a single namespace file, `_default.json`.

### Currently available

| Code | Language | Status |
|------|----------|--------|
| `en` | English (source) | complete |
| `de` | Deutsch (German) | partial |
| `es` | Español (Spanish) | complete |
| `fr` | Français (French) | complete |

"Partial" means some keys present in `en/_default.json` are missing; Papermerge
falls back to English for anything not translated, so a partial file is still
useful.

## File format

- Plain JSON, UTF-8, two-space indentation, sorted the same way as `en`.
- Keys are **identifiers** — e.g. `"common.save"`, `"users.delete.one.title"`.
  Keys are the same in every language. **Never translate a key**, only its
  value.
- The frontend uses [i18next](https://www.i18next.com/). A few value
  conventions come from it:

  | Convention | Example (en) | Notes |
  |------------|--------------|-------|
  | Interpolation | `"{{count}} document(s) waiting for approval"` | Keep every `{{name}}` exactly as-is; only the surrounding words are translated. |
  | Markup tags | `"Title changed from <old>{{before}}</old> to <new>{{after}}</new>"` | Keep `<old>`, `<new>`, `<word>` tags and their order; translate only the text. |
  | Plurals | `"document_one"` / `"document_other"` | Keep the `_one` / `_other` suffixes. Some languages need more plural forms (e.g. `_few`, `_many`) — add them as needed. See [i18next plurals](https://www.i18next.com/translation-function/plurals). |

- **"Papermerge"** is a product name — leave it untranslated.
- Acronyms such as **OCR**, **API**, **REST**, **URL**, **PDF**, **MIME** are
  normally kept as-is unless your language has an established equivalent.

## Contributing

### Improve an existing language

1. Fork this repository.
2. Edit the relevant `localization/<code>/_default.json`.
3. Make sure the file is still valid JSON:
   ```bash
   python3 -m json.tool localization/<code>/_default.json > /dev/null
   ```
4. Open a pull request describing what you changed and why (a short note is
   fine, e.g. "fix wrong term for 'cabinet' in German").

### Add a new language

1. Fork this repository.
2. Create `localization/<code>/` where `<code>` is the ISO 639-1 code
   (`it`, `pt`, `nl`, `pl`, …).
3. Copy `localization/en/_default.json` into it and translate the **values**.
   - You don't have to translate everything at once — untranslated keys fall
     back to English. It's fine to submit a partial file and extend it later.
   - Do keep the keys identical to `en`; don't remove keys you haven't gotten
     to yet (leave the English value, or drop the line — both work, keeping it
     is clearer).
4. Open a pull request. Mention the language and, ideally, that you're a
   fluent/native speaker.

Adding the new language to the picker in Papermerge itself is a small change on
the Papermerge side — link your PR here from a Papermerge issue, or mention it
in your PR description, and a maintainer will wire it up.

### Guidelines

- One language per pull request keeps review simple.
- Match the tone of the English source: concise, neutral, sentence case for
  sentences, title case is not required.
- If a source string is ambiguous or looks wrong, open an issue — the fix may
  belong in the English source.
- Machine-translated PRs without a human review are unlikely to be merged;
  please have a speaker of the language check the result.

## Validation

A pull request is expected to keep every file as valid JSON. Quick local check
for all languages:

```bash
for f in localization/*/_default.json; do
  python3 -m json.tool "$f" > /dev/null && echo "OK  $f" || echo "BAD $f"
done
```

## License

[MIT](./LICENSE) — © 2026 Eugen Ciur and the Papermerge contributors.

By submitting a contribution you agree to license it under the same terms.
