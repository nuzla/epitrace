# epiinfer-contact-tracing

Python implementation of **epiinfer-contact-tracing** (*Forecasting Infection Rates in Epidemics
Given Contact Tracing Information*, Kascelan, Yang and Shasha, *Algorithms*, 2025). Given who met
whom each day, it rolls an epidemic forward on the contact graph and forecasts the number of
symptomatic people. It is packaged so that anyone can **forecast with their own contact-tracing
data** and **reproduce the paper's tables**, from a Python install or a Docker image. The
package, the command and the image are called `epitrace`.

The paper calls the method *EpiInfer-CONTACT*. Here it is *epiinfer-contact-tracing*, to tell it
apart from *EpiInfer*, the authors' earlier system, which uses no contact tracing.

## Documentation

| Page | What it covers |
|---|---|
| [Tutorial](TUTORIAL.md) | **Start here.** A forecast in Docker, step by step: simulate an epidemic, forecast, back-test, and compare with the paper's other methods. Every command's exact output and plot is shown, and CI checks that you get the same numbers. |
| [Dataset format](DATA_FORMAT.md) | The CSV files each of the three engines needs, and how to turn a case list and a contact list into them. |
| [Reproducing the paper](REPRODUCING.md) | The paper's datasets, the command for every table, and how closely each one reproduces. |
| [Paper vs code](PAPER_VS_CODE.md) | Where the published numbers came from, and where the paper and the research code disagree. |
| [Releasing](RELEASING.md) | How the Docker image is published (for maintainers). |

## Three engines

| Engine | What it is |
|---|---|
| `final` (default) | The October 2026 method. It needs only who is symptomatic or recovered each day, plus the contacts. |
| `contact` | The 2025 code behind most published tables. It also needs who is pre-symptomatic, which real data rarely record. |
| `compartmental` | The earlier EpiInfer: deterministic, no contact list, only daily counts per age group. |

## Source, image and citation

- Code, install instructions and issues: [github.com/nuzla/epitrace](https://github.com/nuzla/epitrace)
- Release notes: [CHANGELOG](https://github.com/nuzla/epitrace/blob/main/CHANGELOG.md)
- Container image for Intel/AMD and ARM: [ghcr.io/nuzla/epitrace](https://github.com/nuzla/epitrace/pkgs/container/epitrace)
- MIT licence. Please cite the paper; see [CITATION.cff](https://github.com/nuzla/epitrace/blob/main/CITATION.cff).
