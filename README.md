# Progress Results of the *Podostemum ceratophyllum* Transcriptome Analysis

Slides for the progress report of 29 October 2026, built with [Slidev](https://sli.dev). The deck has 40 slides: the title and main slides (1–32, including the summary and points for discussion), Appendices A–D (33–36), data sources (37) and references (38–40).

The exported PDF is `Podostemum_ceratophyllum_Progress_Results_20261029.pdf`.

## Reading the slides

- Top left: slide number (assigned in slide order) and section.
- Top right: the matching section of the Supplementary Data (S1–S15).
- Bottom, “Source”: section and table numbers in the Comprehensive Experimental Record Report of the *Podostemum ceratophyllum* Transcriptome (the Report; `Podostemum_ceratophyllum_Integrated_Experimental_Record_20261026.docx`, revised 2026-10-28).
- [n]: reference numbers; the list is on slides 38–40.
- Libraries: Root = PR1–3; Leaf = PL1–3, meaning all plant parts remaining after root removal (not pure leaf blades); Whole plant = P1–3, sequenced in a different batch from root and leaf.

## Presenting

Requires Node.js 18 or later and pnpm (`npm i -g pnpm`). In this folder:

```bash
pnpm install
pnpm dev
```

Open http://localhost:3030 in a browser; the presenter view (notes, next-slide preview, timer) is at http://localhost:3030/presenter/. Stop with Ctrl+C in the terminal.

## Exporting

```bash
pnpm export                              # PDF (slides-export.pdf)
pnpm exec slidev export --format pptx    # PowerPoint
```

## Files

| File | Content |
|---|---|
| `slides.md` | All slides. In each slide’s frontmatter, `kicker` is the section name, `supp` the Supplementary Data section and `source` the Report section and tables. Speaker notes are in `<!-- -->` at the end of a slide and appear only in the presenter view |
| `figs/` | The eight figures used in the deck (fig05, 06, 07, 15, 16, 19, 20, 21) |
| `layouts/page.vue` | Content page: slide number and section at top left, Supplementary Data section at top right, Report source at the bottom |
| `layouts/figpage.vue` | Figure page: the figure fills one column from the top of the page to the footer; set `side` (left / right) and `figw` (width of the figure column) in the frontmatter, and put the figure after `::fig::` |
| `layouts/title.vue` | Title page, with the nine libraries on the right |
| `components/` | Small charts: PairBars (before / after, background / candidates), HBar, StackBar, EstCI (point estimates with intervals), DotStrip (three replicates per tissue), Dumbbell (root / leaf TPM) |
| `style.css` | Colours: root #d4893a, leaf #16958b, whole plant #9b8eec (as in the figures); interface in the purple #5b4bc4 family |

## Notes

- Species and genus names are set in italics throughout, including the figures. Labels, notes and titles passed to PairBars, HBar and StackBar accept `<i>` for this.
- The fixed page text (“Source”, “Report”, “Supplement” and the footer mark) is defined in `layouts/page.vue` and `layouts/figpage.vue`; it can be overridden with `labels` in the headmatter of `slides.md`.
- `package.json` pins `markdown-it` to 14.3.3 through `pnpm.overrides`: `@comark/markdown-it` 0.3.4, used by Slidev 53, does not work with markdown-it 15, and the build otherwise fails with a `lib/token.mjs` error.
