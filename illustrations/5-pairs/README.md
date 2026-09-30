# Scout behavior: 5 paired seeds

Both policies run on the same fixed 75 × 75 medium-density, random-placement instance, with 25 objects, five agents, one scout, radio radius 4 and horizon 715. Policy seeds are 23001–23005, paired between HSR-1 and Soft-HSR-1.

![Scout exploration and carrying](../../figures/paper/reviewer12_heatmaps.png)

[Vector PDF](../../figures/paper/reviewer12_heatmaps.pdf) · [Map instance](../../study/maps/test/75/medium/mapd_75x75_medium_random_objects25_topo3001_obj13001.json)

| File | Contents |
|---|---|
| [Run metrics](results/run_metrics.csv) | Ten individual policy runs |
| [Descriptive means](results/descriptive_means.csv) | Fixed-instance policy means |
| [Paired differences](results/paired_differences.csv) | Soft-HSR-1 minus HSR-1 within each seed |
| [Occupancy](results/occupancy.csv) | Mean ticks per cell, with x/y coordinates |
| [Scout activity](results/scout_per_run.csv) | Scout mode-time counters |
| [Scout services](results/scout_services.csv) | Pickup/delivery events and carrying intervals |

`delivery_auc` uses a 0–1 scale; `auc_percent` uses 0–100; `auc_difference_pp` is in percentage points. Occupancy and mode-times are in mission ticks. Only activity with `0 <= tick < 715` contributes to the occupancy figure.

Panels A and B use the same exploration scale; panel C uses a separate carrying scale. HSR-1 scout carrying is structurally zero. The visualization is illustrative; quantitative effect estimates are those of the full factorial study.
