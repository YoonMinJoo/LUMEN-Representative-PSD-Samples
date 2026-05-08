# LUMEN Representative PSD Samples

Representative Power Spectral Density (PSD) samples related to the paper:

> **LUMEN: A Lightweight UAV Multi-Enhanced Network for PSD-Based RF Fingerprinting on Edge Devices**

This repository provides representative PSD examples and preprocessing references for UAV RF fingerprinting research. The released files are intended to illustrate spectral characteristics across UAV classes, flight states, and acquisition conditions used in the proposed LUMEN framework.

---

## Dataset Overview

The released representative samples consist of:

- 2 RF link categories
  - `fly_control_status_data`
  - `fly_telemetry_data`
- 14 UAV classes
  - `uav01` ~ `uav14`
- 3 indoor acquisition conditions
  - `indoor01`
  - `indoor02`
  - `indoor03`
- 5 flight states
  - `arming`
  - `takeoff`
  - `hovering`
  - `moving`
  - `landing`

The sample scale is organized as follows:

```text
2 RF links × 14 UAV classes × 3 indoor conditions × 5 flight states = 420 representative condition groups
```

---

## Directory Structure

```text
data/
├── fly_control_status_data/
│   ├── uav01/
│   │   ├── indoor01/
│   │   │   ├── arming/
│   │   │   ├── takeoff/
│   │   │   ├── hovering/
│   │   │   ├── moving/
│   │   │   └── landing/
│   │   ├── indoor02/
│   │   └── indoor03/
│   ├── uav02/
│   └── ...
│
├── fly_telemetry_data/
│   ├── uav01/
│   ├── uav02/
│   └── ...
```

Each `.npy` file contains a representative PSD sample generated from RF IQ recordings.

---

## PSD Preprocessing

PSD representations were generated from complex IQ samples using Welch spectral estimation.

Main preprocessing configuration:

| Parameter | Value |
|---|---|
| Sampling Rate | 20 MS/s |
| Segment Length | 100 ms |
| PSD Method | Welch |
| FFT Size | 8192 |
| Input Format | Complex IQ samples |
| Output Format | PSD stored as `.npy` |

The PSD computation follows the procedure below:

```python
from scipy.signal import welch

freqs, psd = welch(seg, fs=20_000_000, nperseg=8192)
```

The resulting PSD values can be further transformed using logarithmic scaling:

```python
psd_log = 10 * np.log10(psd + 1e-10)
```

where `1e-10` is used to avoid numerical instability when the spectral power is close to zero.

---

## Example `.npy` Format

Each `.npy` file stores a Python dictionary containing frequency bins and PSD values:

```python
{
    "freqs": freqs,
    "psd": psd
}
```

Example loading code:

```python
import numpy as np

data = np.load("data/fly_control_status_data/uav01/indoor01/arming/arming_0000.npy", allow_pickle=True).item()

psd = data["psd"]

print(psd.shape)
```

---

## Purpose of Release

The released files are intended to:

- illustrate spectral differences across UAV classes
- demonstrate flight-state-dependent spectral variations
- support understanding of the preprocessing pipeline
- provide representative examples related to the LUMEN framework
- improve transparency and reproducibility support for the paper

These files **do not constitute the complete benchmark dataset** used for training and evaluation in the paper.

---

## Notes

- All representative samples were collected in a controlled electromagnetic anechoic chamber environment.
- The repository contains preprocessed PSD examples only.
- Raw IQ recordings and the complete benchmark dataset are not fully distributed in this repository.
- The released samples are provided for academic reference and reproducibility support.

---

## Citation

If you use these representative samples in academic work, please cite:

```bibtex
@article{lumen2026,
  title={LUMEN: A Lightweight UAV Multi-Enhanced Network for PSD-Based RF Fingerprinting on Edge Devices},
  author={Yoon, Min-Joo and Park, Ki-Woong},
  journal={Sensors},
  year={2026}
}
```

---

## Contact

For questions regarding the representative samples or additional research use, please contact the corresponding author of the paper.
