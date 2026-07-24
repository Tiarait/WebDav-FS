# Localization

## Source of truth (English)

`app/src/main/res/values/strings.xml`

## Locale folders

`values-{bg,de,es,fr,hi,ja,ko,nl,pl,pt,ru,uk,vi,zh}/strings.xml`

(Note: Chinese folder is **`values-zh`**, not `values-zh-rCN`.)

## Scripts

| File | Role |
|------|------|
| `scripts/extra_translations.py` | Dict of new/updated keys per locale; `FORCE_UPDATE_KEYS` |
| `scripts/merge_locale_strings.py` | Rebuild each locale file in **EN key order**; keep existing text unless forced |

### Usage

```bash
cd scripts
python3 merge_locale_strings.py          # all locales
python3 merge_locale_strings.py de fr ru # subset
```

### Behavior

1. Walk English `strings.xml`.
2. Skip `translatable="false"`.
3. For each key: use `EXTRA_TRANSLATIONS` if new or force-updated; else keep existing locale value; else fall back to English and print a warning.
4. French (and similar) apostrophes are escaped for Android resources.

### Adding strings

1. Add English to `values/strings.xml`.
2. Add translations to `extra_translations.py` for every locale you care about.
3. Put key names in `FORCE_UPDATE_KEYS` if you must overwrite old wording.
4. Run the merge script.
5. Spot-check a couple of `values-*/strings.xml` files.

Do not paste unrelated translation dumps from other apps into these scripts.
