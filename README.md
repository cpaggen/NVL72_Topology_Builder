# NVL72 Rail Planner

An interactive, browser-based cabling planner for NVL72 GPU rails. It models GPU-to-ToR links, ToR-to-spine uplinks, switch port counts, link speeds, bandwidth oversubscription, and a downloadable CSV cable schedule.

## Topology model and NVIDIA RA alignment

The planner follows the NVIDIA Enterprise Reference Architecture (RA) compute-fabric principles for a dual-plane, rail-optimized design:

- An NVL72 scalable unit is modeled as one rack with 18 compute trays and 4 GPUs per tray (72 GPUs total).
- GPU positions 0–3 form four rails. Each rail connects the same GPU position across the compute trays.
- The compute fabric has two independent planes. Each GPU has a connection to the matching rail in both planes; the default link speed is 400 Gb/s per plane, corresponding to the RA's 800 Gb/s ConnectX-8 interface split into two 400 Gb/s links.
- The diagram shows each rack once and draws the two plane connections from every GPU. Cable color identifies the GPU-position rail; solid and dashed lines distinguish Plane A and Plane B.

Leaf and spine counts are calculated from the selected interface counts and link speeds, including the non-blocking or oversubscription setting. These are generic capacity estimates that follow the RA's dual-plane rail design principles; they are not an exact reproduction of an NVIDIA RA switch bill of materials or its topology-specific rack-pair layouts.

## Cisco ToR models

Choose one of the Cisco Nexus ToR models in the app. The model selection sets the switch's front-panel data-port count and maximum per-port rate; the downlink and uplink rate inputs represent the configured native or breakout link speeds used in the estimate.

| Cisco Nexus model | Data ports | Maximum rate per port |
| --- | ---: | ---: |
| 9332D-H2R | 32 | 400 Gb/s |
| 9364D-GX2A | 64 | 400 Gb/s |
| 9364E-SG2 | 64 | 800 Gb/s |
| 9164E-NS4-O | 64 | 800 Gb/s |
| N9364F-SG3-L | 64 | 1.6 Tb/s |
| N9364F-SG3-A | 64 | 1.6 Tb/s |

Port counts and maximum speeds are based on Cisco product documentation: [9332D-H2R datasheet](https://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/nexus-9332d-h2r-switch-ds.pdf), [9364D-GX2A datasheet](https://www.cisco.com/c/dam/en/us/products/collateral/networking/switches/nexus-9000-series-switches/nexus-9300-gx2-series-fixed-switches-ds.pdf), [9364E-SG2 datasheet](https://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/nexus-9364e-sg2-switch-ds.pdf), [9164E-NS4-O datasheet](https://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/n9164e-ns4-o-switch-ds.html), and Cisco's [N9364F-SG3 announcement](https://blogs.cisco.com/datacenter/networking-for-the-agentic-era-cisco-unveils-new-innovations-in-scale-and-simplicity).

For the source design guidance, see NVIDIA's [Network Logical Architecture](https://docs.nvidia.com/enterprise-reference-architectures/nvl72-ai-factory/latest/network-logical-architecture.html), [Networking Physical Topologies](https://docs.nvidia.com/enterprise-reference-architectures/nvl72-ai-factory/latest/networking-physical-topologies.html), and [System Hardware & Components](https://docs.nvidia.com/enterprise-reference-architectures/nvl72-ai-factory/latest/components.html).

## Run with uv

Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and Python 3.10 or newer. From this folder, run:

```powershell
uv sync
uv run python serve.py
```

Then open <http://127.0.0.1:8000>. Stop the server with `Ctrl+C`.

The project has no third-party Python dependencies. `requirements.txt` is included for environments that use pip instead of uv.

## Run with Python and pip

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python serve.py
```

Open <http://127.0.0.1:8000>.

## Options

The server binds to localhost by default. To choose a different local port:

```powershell
uv run python serve.py --port 8080
```

The app is static and keeps planner inputs in the browser; the server only serves the files and does not store user data.
