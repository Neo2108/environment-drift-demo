# Environment Drift in Self-Improving LLM Agents: Measuring Reference Staleness and False Promotions

Demo website for our NAACL 2027 System Demonstrations submission.

**Live site:** https://anupama-27.github.io/environment-drift-demo/

## What it shows

A language-model agent (Qwen2.5-1.5B-Instruct) improves itself by rewriting its own instruction, and a new instruction is kept only if it beats a stored reference. While the loop runs, the environment changes underneath it: first the decoding settings, then the answer extractor, which goes from strict to lenient. A reference stored before the change is then compared with candidates read after it, so the comparison is no longer fair.

The site replays five measured runs on GSM8K in nine steps:

1. **Questions**: GSM8K test items with the starting instruction's grade under the strict and the lenient extractor.
2. **Self-improvement loop**: candidates scored against the stored reference.
3. **The drift event**: the decoding change, then the extractor change.
4. **The trap springs**: each candidate against the stored reference and against a regenerated one.
5. **The reveal**: false promotions, split by cause.
6. **Which defense?**: frozen reference, refresh every batch, or refresh on environment change.
7. **Dashboard**: all five seeds pooled.
8. **Beyond GSM8K**: how the same system maps onto financial QA and code generation.
9. **The system**: components and how they connect.

## What is measured and what is not

- Every number on steps 1 to 7 comes from the saved runs. Nothing is generated in the browser.
- Across five seeds, the three refresh policies made 13/34, 3/13 and 2/16 false promotions. The extractor change alone inflated a candidate's apparent gain by +1.31 percentage points (95% interval 0.65 to 1.98).
- Step 8 is an illustration. No runs were made on financial QA or code generation, and no results are reported for them.
- Promotion and the check that labels a promotion false use the same 200 questions. Call counts are estimates from a dry run, not measured inference cost.

## Run it locally

The site is plain HTML, CSS and JavaScript with no build step. Browsers block loading the data files from `file://`, so serve the folder over HTTP:

```
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Files

| Path | Contents |
|---|---|
| `index.html`, `app.js`, `style.css` | The site |
| `replay/seed1.json` … `seed5.json` | One file per seed: rounds, decisions under each policy, run metadata |
| `replay/all_seeds.json` | Pooled results, inflation intervals and the GSM8K questions shown on step 1 |
| `media/` | Images, and the demo video once it is added |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |

The `replay/` files are generated from the experiment runs by the exporter in our research code. They are copied here unchanged.

## Data

The questions on step 1 are from the GSM8K test set (Cobbe et al., 2021), released under the MIT licence.
