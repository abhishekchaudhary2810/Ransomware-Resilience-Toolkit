# Ransomware Resilience and Recovery Assessment Toolkit

A Python and Streamlit prototype that assesses ransomware readiness against the
six NIST Cybersecurity Framework (CSF) 2.0 functions.

## What the prototype does

- Collects organisation and assessment details.
- Scores 24 ransomware controls across Govern, Identify, Protect, Detect,
  Respond, and Recover.
- Adjusts claimed maturity using the strength of supporting evidence.
- Highlights critical gaps and produces prioritised recommendations.
- Calculates overall resilience, recovery readiness, and evidence confidence.
- Exports a self-contained HTML report and the raw assessment as JSON.
- Saves assessment snapshots in a local SQLite database.
- Imports and exports control-level assessment data as CSV.
- Includes low, medium and high maturity case studies for repeatable evaluation.
- Maps every toolkit control to current NIST CSF 2.0 outcomes selected by the
  final NIST IR 8374r1 ransomware profile.
- Displays official UK 2025/2026 adoption benchmarks by organisation size.
- Reads the CISA KEV catalog and isolates vulnerabilities associated with known
  ransomware campaigns.

## Run locally

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

## Run tests

```bash
python3 -m unittest discover -s tests -v
```

Run the case-study evaluation:

```bash
python3 evaluate_samples.py
```

Refresh the official datasets:

```bash
python3 refresh_official_data.py
```

## Data files

- `data/sample_case_studies.csv` contains 72 records: 24 controls for each of
  three fictional organisations. The cases are synthetic and contain no
  personal or confidential information.
- `data/assessments.db` is created locally when the application starts. It
  stores organisation details, answers and calculated results as assessment
  snapshots.
- `data/evaluation_results.csv` records the expected-range test results for the
  three sample organisations. All three cases pass their acceptance ranges.
- CSV exports use one row per control and can be imported back into the app.

## Official evidence

The `data_sources` directory records raw source snapshots, derived datasets and
SHA-256 provenance. Official external data provides control definitions, threat
context and contextual benchmarks. It does not replace the organisation's own
evidence and it is not treated as ground-truth maturity data.

## Scoring model

Each control has a maturity answer from 0 to 4 and an evidence rating from 0
to 3. The maturity percentage is multiplied by an evidence factor. This limits
the score when a control is claimed but not supported by reliable evidence.
Control criticality is used as a weight when function and overall scores are
calculated. The scoring rules are transparent in `toolkit/scoring.py`.

This is an assessment aid, not a security certification or a replacement for
professional judgement.
