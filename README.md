# F1 Regulation Era Dataset — Collection Scripts

Python scripts to collect Formula 1 data for each technical regulation era using [FastF1](https://github.com/theOehrly/Fast-F1).

## Datasets

| Era | Years | Kaggle |
|-----|-------|--------|
| Hybrid Turbo V6 | 2018–2021 | *(coming soon)* |
| Ground Effect Hybrid | 2022–2025 | *(coming soon)* |

## Data Collected

For every race weekend:

- **race_results.csv** — driver, team, grid, position, points, status, race time
- **sprint_results.csv** — same schema, only present for sprint weekends
- **lap_times.csv** — 28 columns including:
  - Lap & sector times (s1, s2, s3)
  - All 4 speed traps (speed_i1, speed_i2, speed_fl, speed_st)
  - Tyre data (compound, tyre_life, fresh_tyre, stint)
  - Pit in/out times, track position, personal best flag
  - Track status (green/SC/VSC), deleted laps
  - Car telemetry aggregates (full_throttle_pct, brake_pct, gear_changes, max_speed, min_speed, drs_activated)

## Folder Structure

```
f1-<era>/
├── 2022/
│   ├── 01_Bahrain_Grand_Prix/
│   │   ├── race_results.csv
│   │   └── lap_times.csv
│   ├── 05_Miami_Grand_Prix/
│   │   ├── race_results.csv
│   │   ├── sprint_results.csv   ← sprint weekends only
│   │   └── lap_times.csv
│   ...
├── 2023/
...
```

## Usage

```bash
pip install fastf1 pandas
```

Edit the config at the top of `collect_season.py`:

```python
YEAR    = 2024   # ← only change this
ERA_DIR = 'f1-ground-effect-hybrid-era'
```

Then run:

```bash
python collect_season.py
```

The script will:
- Auto-fetch the full race schedule for that year
- Detect sprint weekends automatically
- Skip future rounds (safe to run mid-season)
- Handle per-round errors gracefully without stopping
- Delete the FastF1 cache on completion

## Data Source

All data sourced from [FastF1](https://github.com/theOehrly/Fast-F1), which pulls from the official F1 live timing API. Reliable coverage from **2018 onwards**.

## Notes

- Telemetry extraction (car data per lap) is the slowest step — expect **2–4 hours** for a full season depending on internet speed
- FastF1 caches data locally during the run; cache is auto-deleted on completion
- `speed_fl` (finish line speed trap) may have some nulls depending on the circuit
- `s1` nulls on lap 1 are expected (out-lap, sector 1 not cleanly recorded)
