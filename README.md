# RouteReview

Deep learning model that flags PCB routing issues that design rule checks (DRC) miss.

APS360 (Applied Fundamentals of Deep Learning) course project, University of Toronto.

## Overview

DRC catches hard violations like clearance and trace width errors, but many real routing problems pass DRC and only get caught in manual review. RouteReview takes an image of a PCB layout and highlights these issues pixel by pixel so a designer can fix them before fabrication.

## Issues detected

| Class | Why it matters |
|---|---|
| Acute angle | Traps etchant during fabrication, can cause opens |
| Dangling stub | Acts as an antenna, causes reflections |
| Redundant vias | Extra inductance, cost, and failure points |
| Long parallel run | Crosstalk between nets |
| Neck-down | Impedance discontinuity and current bottleneck |
| Plane split crossing | Signal routed over a gap in the reference plane breaks its return path |

## Pipeline

1. **Collect** open-source KiCad boards from GitHub (permissive licenses only)
2. **Inject** known routing defects into clean boards with the KiCad Python API, logging each location as a label
3. **Render** copper, via, pad, and reference plane layers into a 5-channel image and tile it
4. **Train** a U-Net (convolutional encoder-decoder) to segment each issue class
5. **Evaluate** against a classical computer vision baseline on synthetic boards and a hand-labeled set of real boards

## Repository structure (planned)

```
route-review/
├── data/            # board list, injection logs (raw boards not committed)
├── scripts/
│   ├── collect/     # board scraping and filtering
│   ├── inject/      # defect injection
│   └── render/      # layer export and tiling
├── baseline/        # classical CV baseline
├── model/           # U-Net and training code
├── notebooks/       # Colab training and evaluation notebooks
└── results/         # metrics and example outputs
```

## Status

Proposal stage. Code coming soon.

## Requirements (planned)

- KiCad 8 (with `kicad-cli` and the `pcbnew` Python API)
- Python 3.10+
- PyTorch, OpenCV, NumPy

## Data and licensing

Source boards are used under their original licenses and credited in `data/sources.csv`.

## Author

Raky Aissata Kane
