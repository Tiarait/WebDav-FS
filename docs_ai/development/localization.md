# Localization

## Source of truth (English)

`app/src/main/res/values/strings.xml`

## Locale folders

`values-{bg,de,es,fr,hi,ja,ko,nl,pl,pt,ru,uk,vi,zh}/strings.xml`

Chinese folder is **`values-zh`** (not `values-zh-rCN`).

## Scripts (repo `scripts/`)

| File | Role |
|------|------|
| `extra_translations.py` | Per-locale dict + `FORCE_UPDATE_KEYS`; imports batch helpers |
| `missing_translations_batch.py` | Large batches of new keys (status / wizard / about / settings) |
| `merge_locale_strings.py` | Rebuild each locale file in **EN key order**; keep existing text unless forced |

### Usage

```bash
cd scripts
python3 merge_locale_strings.py          # all locales
python3 merge_locale_strings.py de fr ru # subset
```

### Behavior

1. Walk English `strings.xml`.
2. Skip `translatable="false"`.
3. For each key: use `EXTRA_TRANSLATIONS` (including batch) if new or force-updated; else keep existing locale value; else fall back to English and print a warning.
4. French (and similar) apostrophes are escaped for Android resources.

### Adding strings

1. Add English to `values/strings.xml` (use ASCII `-`, not em dashes).
2. Add translations to `extra_translations.py` and/or `missing_translations_batch.py` for every locale.
3. Put key names in `FORCE_UPDATE_KEYS` only when overwriting old wording on purpose.
4. Run the merge script.
5. Spot-check a couple of `values-*/strings.xml` files; confirm key counts match EN.

Do not paste unrelated translation dumps from other apps into these scripts.

### Hygiene notes

- Remove unused keys from EN first; merge will drop them from locales when rebuilding from EN order.
- Prefer normal hyphen `-` in user-facing copy (avoid `—` / `–`).
