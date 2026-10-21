# EdStats — Tableau de liaison des DataFrames

Source : `Project1_Session2_load_and_understand.html` (section 3)

## Clés de chaque DataFrame

| DataFrame | Fichier | Lignes | Clé pays | Clé indicateur | Année |
|---|---|---:|---|---|---|
| `data` | EdStatsData.csv | 886 930 | `Country Code` | `Indicator Code` | une colonne par année (1970 … 2100) |
| `country` | EdStatsCountry.csv | 241 | `Country Code` | — | — |
| `series` | EdStatsSeries.csv | 3 665 | — | `Series Code` | — |
| `country_series` | EdStatsCountry-Series.csv | 613 | `CountryCode` | `SeriesCode` | — |
| `footnote` | EdStatsFootNote.csv | 643 638 | `CountryCode` | `SeriesCode` | `Year` (texte : `'YR2001'`) |

## Liaisons

| Liaison | Colonnes de jointure | Cardinalité | Correspondance brute | Après normalisation (`strip` + `upper`) |
|---|---|---|---|---|
| `data` → `country` | `Country Code` = `Country Code` | N → 1 | 241 / 242 codes (VGB absent de `country`) | inchangé |
| `data` → `series` | `Indicator Code` = `Series Code` | N → 1 | 3 612 / 3 665 codes | 3 665 / 3 665 |
| `country_series` → `country` | `CountryCode` = `Country Code` | N → 1 | 211 / 211 codes | — |
| `country_series` → `series` | `SeriesCode` = `Series Code` | N → 1 | 21 / 21 codes | — |
| `country_series` → `data` | `CountryCode` + `SeriesCode` | 0 ou 1 note par couple (pays, indicateur) | — | — |
| `footnote` → `data` (pays) | `CountryCode` = `Country Code` | N → 1 | 239 / 239 codes | — |
| `footnote` → `data` (indicateur) | `SeriesCode` = `Indicator Code` | N → 1 | 1 469 / 1 558 codes | 1 482 / 1 482 |
| `footnote` → valeurs de `data` | `CountryCode` + `SeriesCode` + `Year` | 0 ou 1 note par valeur | 91,2 % des notes visent une cellule remplie | — |

## Points à surveiller

- Les noms des colonnes de clé diffèrent selon les fichiers : `Country Code` / `CountryCode` et `Indicator Code` / `Series Code` / `SeriesCode`. Il faut les renommer avant de faire une jointure.
- Les codes d'indicateur diffèrent parfois par un espace en fin de code ou par la casse (`'SE.SEC.DURS.LO '`, `UIS.E.1.Guk`). Il faut normaliser avec `.str.strip().str.upper()`.
- `footnote.Year` est du texte. On obtient l'année avec `footnote["Year"].str[2:].astype(int)`.
- 462 indicateurs n'ont pas le même nom dans `data` et dans `series`.
