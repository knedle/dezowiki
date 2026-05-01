# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

## Co je tento projekt

Tematická wiki názorů, postojů a tvrzení z přepisů videí a rozhovorů. Výstupem jsou MD soubory publikované přes MkDocs na GitHub Pages.

---

## Stav projektu (pilotní fáze)

- `prepisy/` — existuje, obsahuje přes 100 TXT souborů z více zdrojů
- `wiki/` — **zatím neexistuje**, bude vytvořena zpracováním přepisů
- `mkdocs.yml` — zatím neexistuje

---

## Formát souborů v `prepisy/`

**Název souboru:** `YYYY-MM-DD_Jméno-Zdroje_Název-videa[_N].txt`

**Obsah souboru** má tři sekce oddělené `===`:
1. `METADATA` — obsahuje pole `Kanál:`, `Datum videa:`, `URL:`, aj.
2. `VÝKON` — technické info o přepisu (ignoruj)
3. `PŘEPIS` — samotný text s časovými značkami `[MM:SS]`

**Zdroje aktuálně v `prepisy/`:**
- `Jindřich-Rajchl`
- `Lenka-Tarabová`
- `Konspirátor-Boldy-KonspyChannel`
- `Československo-TV2`
- `Jiří-Černohorský-Živě`

Zdroj se identifikuje z části názvu souboru mezi prvním a druhým podtržítkem, nebo z pole `Kanál:` v METADATA.

---

## Omezení v pilotní fázi

Zpracovávej **maximálně 3 soubory od každého zdroje** za jedno sezení, dokud nebude finální produkt hotový.

Pokud uživatel nepřiloží konkrétní seznam, vyber 3 nejstarší nezpracované soubory od každého zdroje (dle data v názvu souboru).

---

## Pravidla zpracování přepisů

### Co extrahovat
- **Tvrzení** — konkrétní faktuální výroky ("X způsobuje Y", "Z funguje tak, že...")
- **Názory a postoje** — hodnotící výroky, přesvědčení, doporučení
- **Zajímavé citace** — přesné formulace hodné zachování (max 1–2 věty)

### Jak strukturovat výstup
- Obsah organizuj **tematicky**, ne podle videí
- Každé téma = jeden MD soubor ve `wiki/`
- Pokud téma už existuje → **rozšiřuj**, nevytvářej duplicity
- Pokud tvrzení z různých zdrojů **souhlasí** → sluč je, přidej oba zdroje
- Pokud si **odporují** → zachovej obě verze, označ jako ⚡ Konflikt

### Formát záznamu v MD souboru

```markdown
## Název tématu / podtématu

### Tvrzení nebo název postoje

Stručný popis (2–4 věty vlastními slovy, ne copy-paste).

> „Přesná citace pokud stojí za zachování." — Jméno, [zdroj](_zdroje.md)

**Zdroje:** [Video 01](_zdroje.md#video-01), [Video 03](_zdroje.md#video-03)

---
```

### Soubor `_zdroje.md`

```markdown
## Název videa
- **Soubor:** `prepisy/YYYY-MM-DD_Zdroj_Název.txt`
- **Kanál:** ...
- **Datum záznamu:** ...
- **Zpracováno:** ano
```

---

## Workflow při zpracování přepisu

1. Přečti soubor z `prepisy/` (sekci PŘEPIS, METADATA pro atribuci)
2. Identifikuj témata → zkontroluj existující soubory ve `wiki/`
3. Rozšiř existující nebo vytvoř nový MD soubor
4. Přidej záznam do `wiki/_zdroje.md`
5. Aktualizuj `mkdocs.yml` nav sekci, pokud vznikl nový soubor

---

## Tón a styl

- Popisuj **co kdo tvrdí**, ne co je pravda ("X zastává názor, že..." vs. "X je pravda")
- Zachovej neutrální tón, i když tvrzení jsou kontroverzní
- Výstup vždy česky

---

## Publikování (MkDocs)

```bash
pip install mkdocs-material
mkdocs serve          # lokální náhled
mkdocs gh-deploy      # publikování na GitHub Pages
```

**`mkdocs.yml` šablona:**
```yaml
site_name: Wiki názorů
theme:
  name: material
  language: cs
  features:
    - navigation.tabs
    - search.suggest
docs_dir: wiki
nav:
  - Úvod: index.md
  - Zdroje: _zdroje.md
```

**GitHub Actions** (`.github/workflows/deploy.yml`) — automatický deploy při `git push` na `main`:
```yaml
name: Deploy wiki
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
```
