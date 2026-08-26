# UK &amp; Ireland Port Congestion Dataset

Open data on congestion at **17 UK and Ireland ports** — health scores, vessel
queues, berth utilisation, waiting times, and week-over-week movement.

Published by [Carrgo Freight Solutions](https://www.carrgo.co.uk/), a UK freight
forwarder. **Free to use with attribution (CC BY 4.0).**

Most freight sites publish a single live congestion number and overwrite it, which
makes historical comparison impossible. This dataset keeps dated snapshots so you
can see how conditions actually moved.

---

## Quick start

| File | What it is |
|------|-----------|
| [`data/latest.csv`](data/latest.csv) | Most recent snapshot, CSV |
| [`data/latest.json`](data/latest.json) | Most recent snapshot, JSON (with methodology) |
| `data/uk-port-congestion-YYYY-MM-DD.csv` | Dated permanent snapshot |
| `data/uk-port-congestion-YYYY-MM-DD.json` | Dated permanent snapshot |

```bash
# grab the latest as CSV
curl -O https://raw.githubusercontent.com/rbuilder80-sudo/carrgo-uk-port-congestion-data/main/data/latest.csv
```

```python
import pandas as pd
url = ("https://raw.githubusercontent.com/rbuilder80-sudo/"
       "carrgo-uk-port-congestion-data/main/data/latest.csv")
df = pd.read_csv(url)

# ports getting worse this week
print(df[df.score_change_7d < 0][["port", "health_score", "score_change_7d"]])
```

---

## Fields

| Field | Meaning |
|-------|---------|
| `port` | Port name |
| `slug` | URL-safe identifier |
| `region` / `country` | England, Scotland, Wales, Northern Ireland, Ireland (ROI) |
| `status` | Normal / Moderate / Congested |
| `health_score` | **0-100 composite.** Above 75 normal, 50-74 moderate, below 50 congested |
| `score_yesterday` | Same measure 1 day earlier |
| `score_week_ago` | Same measure 7 days earlier |
| `score_month_ago` | Same measure 30 days earlier |
| `score_change_7d` | Movement over 7 days. **Negative = deteriorating** |
| `score_change_30d` | Movement over 30 days |
| `wait_time` | Typical waiting time before a vessel is worked |
| `vessels_waiting` | Vessels queued for a berth |
| `vessels_at_berth` | Vessels currently being worked |
| `berth_utilisation_pct` | Berth occupancy, percent |
| `trend` | Improving / Stable / Worsening |
| `best_score` / `worst_score` | Observed range for that port |

## Methodology

`health_score` is a 0-100 composite of three measured inputs: **vessels waiting for
a berth**, **berth utilisation**, and **average waiting time** before a vessel is
worked.

- **75-100 — Normal.** Operating within expected parameters.
- **50-74 — Moderate.** Delay risk; build slack into inland delivery.
- **0-49 — Congested.** Expect knock-on delays and demurrage exposure.

Interpretation note: the **trend matters more than the absolute score** when you are
booking. A port at 68 falling 3 points a week is a worse bet than a port at 62
holding steady, because the trend tells you what conditions will look like when your
vessel actually arrives.

## Why this exists

Carrgo publishes this because our own operations depend on it, and because port
congestion data in the UK is otherwise fragmented, paywalled, or overwritten. If it
saves you a demurrage bill or a bad routing decision, it has done its job.

## Licence &amp; attribution

Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
You may copy, republish, adapt and build on this data, including commercially,
provided you attribute the source.

**Suggested citation:**

> Carrgo Freight Solutions, *UK &amp; Ireland Port Congestion Dataset*,
> https://www.carrgo.co.uk/resources/uk-port-congestion-report/

Journalists, analysts, researchers and other freight businesses are welcome to use
it. If you publish something built on this data we would like to see it.

## Related, from the same data

- 🔴 [**Live port congestion tracker**](https://www.carrgo.co.uk/resources/port-congestion-tracker/) — continuously updated
- 📄 [**Weekly UK Port Congestion Report**](https://www.carrgo.co.uk/resources/uk-port-congestion-report/) — readable analysis, dated editions
- 🗂 [**Report archive**](https://www.carrgo.co.uk/resources/uk-port-congestion-report/archive/) — every past edition
- ⚖️ [**Port comparison tool**](https://www.carrgo.co.uk/tools/port-comparison/) — compare two ports side by side
- 🧮 [**Freight cost calculator**](https://www.carrgo.co.uk/tools/cost-calculator/) — estimate landed cost

## Disclaimer

Provided for information and planning. Port conditions change continuously and
figures are indicative rather than contractual. Verify against your carrier or
terminal before making a commercial commitment.

---

*Maintained by [Carrgo Freight Solutions](https://www.carrgo.co.uk/) — UK freight
forwarding, customs clearance, sea, air, road and rail freight.*
