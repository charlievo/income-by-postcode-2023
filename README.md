# Australian Income by Postcode (2022–23)

A geographic income analysis of Australia using ATO taxation statistics, joined with ABS socio-economic data and postcode geography. Built as part of a data analytics portfolio targeting roles in tax/data analysis.

**Live dashboard:** [Tableau Public Story](#) — interactive map, key findings, state breakdowns, and tax/debt analysis across four pages.

## What this is

Most income-by-postcode visualisations stop at a single colour-coded map. This project goes further: it joins three separate datasets, runs exploratory analysis to surface non-obvious patterns, segments postcodes using k-means clustering, and builds a four-part interactive Tableau Story to communicate the findings.

## Data sources

- **ATO Taxation Statistics 2022–23**, Table 6B — individual taxpayers by postcode ([data.gov.au](https://data.gov.au/data/dataset/taxation-statistics-postcode-data))
- **ABS SEIFA 2021** — Index of Relative Socio-Economic Advantage and Disadvantage, by postal area ([abs.gov.au](https://www.abs.gov.au/statistics/people/people-and-communities/socio-economic-indexes-areas-seifa-australia/2021))
- **Postcode geography lookup** — suburb names, state, and remoteness classification ([github.com/matthewproctor/australianpostcodes](https://github.com/matthewproctor/australianpostcodes))

Note: SEIFA is Census-based and only updated every five years, so 2021 is the most recent version available — there's a small mismatch with the 2022–23 ATO data that's worth keeping in mind when interpreting the SEIFA/income relationship.

## Process

1. **Clean** — loaded the raw ATO Excel file, dropped subtotal rows, renamed 160+ columns down to the ones that mattered
2. **Join** — merged in SEIFA deciles and suburb/remoteness data using postcode as the key, checked match rates
3. **Derive** — calculated per-lodger averages (raw dollar totals are meaningless without dividing by population), plus an effective tax rate and a "theoretical" rate based on 2022–23 tax brackets for comparison
4. **Explore** — distribution analysis, outlier detection (needed before touching Tableau's colour scale, otherwise a handful of extreme postcodes wash out the whole map), correlation matrix, income composition by quartile
5. **Segment** — k-means clustering (9 standardised variables) to group postcodes into profiles that go beyond a simple income ranking
6. **Visualise** — exported a clean CSV and built the interactive dashboard in Tableau

## Key findings

- **69x** difference between the highest and lowest average-income postcode in the country (Darling Point, NSW vs Bungunya, QLD)
- SEIFA score and income are correlated but not tightly — r ≈ 0.46 across all postcodes, with plenty of exceptions in both directions
- Every income quintile pays a higher effective tax rate than the theoretical bracket rate would suggest, and the gap is largest at the bottom — likely an artefact of how taxable income is already reported post-deduction
- Clustering surfaced four distinct postcode profiles that income alone doesn't capture — e.g. a "Wealthy Investor Class" segment defined as much by capital gains and dividend income as by salary
- State-level patterns vary a lot more than a single national average suggests — ACT has zero postcodes in the bottom two income quintiles, while Tasmania has almost a third of its postcodes there
- HELP debt is heavily concentrated in inner-city graduate hubs, with Melbourne Inner leading nationally

## Tools

Python (pandas, numpy, scikit-learn, matplotlib, seaborn) for cleaning, joining, EDA and clustering. Tableau Public for the interactive dashboard.

## Files

- `notebook/postcode.ipynb` — full analysis notebook, cleaning through to CSV export
- `data/` — raw source files
- `output/postcodes_cleaned.csv` — final cleaned dataset used in Tableau

## Limitations

- SEIFA 2021 vs ATO 2022–23 creates a two-year lag between the two datasets
- A small number of postcodes (around 15) don't have a geographic match in Tableau and are excluded from the map
- The "theoretical tax rate" calculation is a simplified bracket-only model and doesn't account for every offset or deduction interaction
