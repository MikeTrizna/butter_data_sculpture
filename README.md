# Butter Chart

A chart of America's butter cold-storage stocks over a typical year, built to be carved out of three sticks of butter for a butter sculpting competition. It comes with a dramatic story to read aloud beside it.

This README explains how the project was made. **Almost all of the code, charts, cutting patterns, and narration were written by AI** (Anthropic's Claude, through Claude Code in VS Code), with me directing it. I want to be clear about which parts came from me and which came from the AI.

## What's here

| File | What it is | Who made it |
|---|---|---|
| `initial_prompt.txt` | The detailed spec I wrote for the data-analysis notebook | Me |
| `butter_cold_storage.ipynb` | Downloads 112 years of USDA butter-stock data and charts the seasonal cycle | Claude (Sonnet 5.5) |
| `data/raw/` | Cached USDA NASS Quick Stats data, plus metadata on which series was used | Produced by Claude's notebook |
| `figures/butter_cold_storage.pdf` (and `01_`–`05_*.pdf`) | Printable charts: radial cycle, heatmap, monthly subseries, typical-year mean/median | Claude (Sonnet 5.5) |
| `butter_sculpture.ipynb` | Works out whether the mean radial bar chart can be carved from 3 butter sticks, and how | Claude (Sonnet 5.5) |
| `figures/butter_sculpture_patterns.pdf` | Full-size (1:1) printable cutting templates | Claude (Sonnet 5.5) |
| `butter_sculpture_story.md` | "The Churning of the Year," an over-the-top narration full of puns | Claude (Opus 5.5) |
| `requirements.txt` | Pinned Python dependencies | Claude |
| `README.md` | This file | Claude (Opus 5.5), from the files and session history, at my request |

## How it was made

Everything happened on one evening, November 17, 2026, over less than an hour, in four Claude Code sessions. Each step below gives the prompt I used and a summary of what the AI did.

### 1. The data and charts (about 20 minutes)

I wrote a long, specific prompt ahead of time; it's saved in [`initial_prompt.txt`](initial_prompt.txt). It covered:

- **Data:** use the USDA NASS Quick Stats API, look up the parameter values instead of guessing them, cache the raw data, and stop rather than quietly switching sources if the API fails.
- **Cleaning and validation:** handle suppressed values, flag duplicates, and check against known published figures (Dec 2023 ≈ 199.5M lb, May 2024 ≈ 379.75M lb, an all-time high near 747M lb in 1992).
- **Charts:** a radial "cycle" chart, a heatmap, and seasonal subseries panels, in matplotlib only.
- **Print requirements:** US Letter size, vector output, embedded fonts, grayscale-safe colormaps, and a source note on every page.
- **Final checks:** restart and run all, then open the PDF and fix any clipped or overlapping labels.

(The prompt file holds two overlapping drafts of the same spec. I pasted it in, stopped the first try after a few seconds, and restarted with a line added about my Python environment.)

**What Claude did:**

- Explored the API and picked the only matching series, `BUTTER, COLD STORAGE - STOCKS, MEASURED IN LB`, explaining why.
- Pulled 1,324 monthly values from Dec 1915 to Sep 2026.
- Ran the sanity checks. It found that my "747M lb" record was actually **June** 1992. The true all-time high is **755.8M lb in July 1992**.
- Built the charts, rendered the PDF, looked at each page, and fixed two layout problems: month labels colliding with a caption, and a footer overlapping the colorbar.

**My follow-up:**

> "This looks fantastic. Based on the final visualization, can you also produce a chart that just looks at median or mean values across all years in the dataset? And can you also print that out as a normal non-radial bar chart?"

This added the typical-year radial chart and bar chart (pages 5–6 of the PDF).

### 2. Turning the chart into a butter sculpture (about 7 minutes)

> "Ok, this is going to go in a totally wacky direction. Read through the analysis done in 'butter_cold_storage.ipynb', and pick the Mean value radial bar chart at the end. Now I am supposed to compete in a butter stick sculpting competition. We will be issued 3 standard us east code butter sticks. I want to somehow slice each of the sticks into 4 slices, and then use each of those to form a month from the radial chart. Can you create a Jupyter notebook that works out whether this is possible, and works towards a pattern that I can print?"

I also told it that it could install missing packages, such as shapely.

**What Claude did** (in `butter_sculpture.ipynb`):

- **Checked the stick size.** It assumed 4.75 × 1.25 × 1.25 in sticks and confirmed they predict about the label weight (within 2.3%).
- **Compared two ways to slice.** Cutting each stick across into short chunks was compared with cutting it lengthwise into planks. Lengthwise planks allow bars about twice as long.
- **Searched for the best design.** It tried a range of bar widths and inner radii to find the largest scale where every month's wedge still fits inside its plank.
- **Chose the final numbers:**
  - 19° wedges around a 0.95 in inner circle.
  - A scale of 1.10 in per 100M lb.
  - A finished chart about 6.8 in across.
  - July is the tightest fit, with only 0.02 in to spare.
- **Made 1:1 printable templates.** They include a calibration ruler, a cut guide for each stick, the assembled chart, and one template per month.

Claude listed its assumptions for me to check: 0.02 in lost to each cut, a 0.05 in safety margin, and a 0.30 in minimum bar width. It also said it hadn't visually checked the last page of the template PDF.

### 3. The dramatic narration (about 3 minutes)

> "Can you read through the data findings in butter_code_storage.ipynb, and do some additional research, and prepare an overly-dramatic narration describing the trends in butter storage over the course of a year? Read butter_sculpture_patterns.pdf to see how we are making this into a butter sculpture for a competition. Please be sure to use numerous butter and Thanksgiving puns as possible."

**What Claude did:**

- Read both notebooks and the pattern PDF.
- Ran three web searches:
  - why stocks build in spring and drop before the holidays,
  - the 1992 government butter surplus,
  - Thanksgiving butter sales.
- Wrote `butter_sculpture_story.md`. It takes the months in clockwise order around the sculpture and quotes each bar's real numbers. It ends with an "Unsalted Facts" appendix that has the data table and source links.
- Flagged one discrepancy. The 112-year mean (which the sculpture uses) peaks in July, but over the last 30 years the peak comes in May. The story uses the sculpture's numbers and mentions the modern pattern.

### 4. This README

I asked Claude to read the folder and its own session history and write this file.

## What was mine and what was the AI's

**Mine:**

- The idea of a butter-storage chart for a butter sculpting contest.
- The detailed spec for the data notebook: data source, validation targets, chart types, and print rules.
- Each follow-up direction: the typical-year chart, the sculpture feasibility study, and the narration.
- Reviewing the outputs.
- The actual carving.

**The AI's:**

- All the Python code.
- Data exploration and cleaning decisions (each one explained in the notebooks).
- Chart design details.
- The geometry and optimization for the sculpture.
- The printable patterns.
- The web research and all of the narration's prose and puns.

## Caveats

- **The data is real.** It comes straight from USDA NASS. The checks against published figures are built into the notebook and pass.
- **I haven't verified every outside claim in the story.** The background facts (the spring flush, the 1992 government butter mountain, Thanksgiving sales jumps) came from Claude's web searches, and the sources are linked in the story's appendix. Treat them with the skepticism you'd give any AI-gathered research.
- **The sculpture design is a model.** It rests on assumed stick sizes and cutting losses, which are editable parameters in the first cell of `butter_sculpture.ipynb`. Real butter may disagree.

## Reproducing it

```bash
pip install -r requirements.txt   # or use your own environment
echo "NASS_API_KEY=your_key_here" > .env   # free key from quickstats.nass.usda.gov
jupyter nbconvert --to notebook --execute butter_cold_storage.ipynb
jupyter nbconvert --to notebook --execute butter_sculpture.ipynb
```

The raw data is cached in `data/raw/`, so the notebooks run offline unless you set the refresh flag at the top of `butter_cold_storage.ipynb`. `.env` is git-ignored. The sculpture notebook needs only numpy, pandas and matplotlib. During the session Claude also installed `pymupdf` to render the PDF pages to images for checking, but neither notebook uses it.
