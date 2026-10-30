# Progress Results of the *Podostemum ceratophyllum* Transcriptome Analysis

Slides for the progress report of 29 October 2026, built with [Slidev](https://sli.dev). The deck has 40 slides: the title and main slides (1–33, including the summary and points for discussion), Appendices A–C (34–36), data sources (37) and references (38–40).

The exported PDF is `Podostemum_ceratophyllum_Progress_Results_20261029.pdf`.

## Reading the slides

- Top left: slide number (assigned in slide order) and section.
- Top right: the matching section of the Supplementary Data (S1–S15).
- Bottom, “Source”: section and table numbers in the Comprehensive Experimental Record Report of the *Podostemum ceratophyllum* Transcriptome (the Report; `Podostemum_ceratophyllum_Integrated_Experimental_Record_20261026.docx`, revised 2026-10-29).
- [n]: reference numbers (1–50); the list is on slides 38–40.
- Libraries: Root = PR1–3; Leaf = PL1–3, meaning all plant parts remaining after root removal (not pure leaf blades). Three unsplit whole-plant libraries (P1–3, another sequencing run) went into the assembly and are not used in any comparison; every comparison, table and figure in the deck uses root and leaf, and the few pooled values that still include the whole plant (unknown-sequence estimates on slides 27–28, gene counts in the slide 12 footnote and fig19) say so.
- Under each analysis title, a line starting with **Method** names the methods and tools of that step.

## Presenting

Requires Node.js 18 or later and pnpm (`npm i -g pnpm`). In this folder:

```bash
pnpm install
pnpm dev
```

Open http://localhost:3030 in a browser; the presenter view (notes, next-slide preview, timer) is at http://localhost:3030/presenter/. Stop with Ctrl+C in the terminal.

## Website

Every push to `main` builds the slides with GitHub Actions (`.github/workflows/deploy.yml`) and publishes them with GitHub Pages at https://keguangcheng.com/podostemum_results/. The repository’s Pages source must be set to “GitHub Actions” (Settings → Pages → Build and deployment → Source). The workflow can also be started by hand from the Actions tab (Deploy pages → Run workflow).

To check the site build locally:

```bash
pnpm exec slidev build --base /podostemum_results/
```

## Exporting

```bash
pnpm export                              # PDF (slides-export.pdf)
pnpm exec slidev export --format pptx    # PowerPoint
```

## Files

| File | Content |
|---|---|
| `slides.md` | All slides. In each slide’s frontmatter, `kicker` is the section name, `supp` the Supplementary Data section and `source` the Report section and tables. Speaker notes are in `<!-- -->` at the end of a slide and appear only in the presenter view |
| `figs/` | The figures used in the deck: fig05, 06, 07, 16, 19, 21 (root and leaf only, redrawn on 2026-10-29), fig22 (pie chart of the non-plant fragments), fig23 (orthologues of the 28 protein markers in the nine-species family trees), fig24 (STM, CUC3 and PLT clades from these trees) and fig15 |
| `layouts/page.vue` | Content page: slide number and section at top left, Supplementary Data section at top right, Report source at the bottom |
| `layouts/figpage.vue` | Figure page: the figure fills one column from the top of the page to the footer; set `side` (left / right) and `figw` (width of the figure column) in the frontmatter, and put the figure after `::fig::` |
| `layouts/title.vue` | Title page, with the six root and leaf libraries on the right |
| `components/` | Small charts: PairBars (before / after, background / candidates), HBar, StackBar, EstCI (point estimates with intervals), DotStrip (three replicates each for root and leaf), Dumbbell (root / leaf TPM) |
| `style.css` | Colours: root #d4893a, leaf #16958b (as in the figures); interface in the purple #5b4bc4 family. The `.method` class styles the method line under each title |

## Notes

- Revision of 29 October 2026: the whole plant was removed from all figures, tables and text (the whole-plant slide, the expression-profile slide of unknown sequences and Appendix D were taken out); a pie chart of the non-plant RNA was added as slide 5; each analysis slide has a method line; the nine-species family trees were added to the open items.
- Later on 29 October 2026: the results of the nine-species family trees (Report 4.3.7) were added as slides 19–20 (fig23, fig24); the marker levels on slide 18, the STM slide and its heatmap (fig16, orthology column only), Appendix B, the open items, the summary and the points for discussion were updated to these trees (high 14, medium 2, not detected 6, TDIF-class motif evidence 2 among the 24 vegetative targets); reference 50 was added.
- Species and genus names are set in italics throughout, including the figures. Labels, notes and titles passed to PairBars, HBar and StackBar accept `<i>` for this.
- The fixed page text (“Source”, “Report”, “Supplement” and the footer mark) is defined in `layouts/page.vue` and `layouts/figpage.vue`; it can be overridden with `labels` in the headmatter of `slides.md`.
- `package.json` pins `markdown-it` to 14.3.3 through `pnpm.overrides`: `@comark/markdown-it` 0.3.4, used by Slidev 53, does not work with markdown-it 15, and the build otherwise fails with a `lib/token.mjs` error.
