# EcoLens

EcoLens is a client-side Smart Sustainability Recommendation System for institutions. It converts monthly energy, waste, and water readings into hotspots, anomaly alerts, ranked improvements, savings scenarios, and a printable decision brief.

## Setup

```bash
pnpm install
pnpm dev
```

Production verification:

```bash
pnpm test
pnpm build
```

The app uses React 19, Vite, TypeScript, Tailwind CSS v4, React Router, Zustand, Recharts, Framer Motion, PapaParse, jsPDF, and html2canvas. Data and settings persist in local storage; no backend is required.

## CSV format

Use UTF-8 CSV with this exact header:

```csv
month,block,energy_kwh,waste_kg,water_kl
2026-09,Admin Block,4300,380,120
```

- `month` uses `YYYY-MM`.
- Resource values must be non-negative numbers.
- Upload replaces the current dataset after the entire file passes validation.

## Scoring and assumptions

- **Attention Index** = `100 × (0.5 × energy/maxEnergy + 0.3 × waste/maxWaste + 0.2 × water/maxWater)` for selected-period block totals.
- **Block score** = `max(0, 100 − 0.6 × Attention Index − anomaly penalty)`.
- An anomaly occurs when the latest month is above the configured multiplier (default `1.2×`) of that block’s previous six-month energy average.
- Recommendation rules live in `src/lib/recommendations.ts`; actions are ranked by monthly rupee saving.
- Rates and carbon factors are editable in Settings and recalculate the entire app.

## Tests

Vitest covers attention indexing, anomaly detection, rate-sensitive savings, and recommendation ranking.

## Deploy to Vercel

Import the repository into Vercel. The Vite framework preset is detected automatically; no environment variables or backend configuration are needed. The production command is `pnpm build` and output directory is `dist`.

The optional AI summary is template-based by default and never needs an API key. A future Claude/Gemini adapter can replace the local summary without changing recommendation calculations.
