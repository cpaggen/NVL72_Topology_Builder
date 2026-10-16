# NVL72 Rail Planner

An interactive, browser-based cabling planner for NVL72 GPU rails. It models GPU-to-ToR links, ToR-to-spine uplinks, switch port counts, link speeds, bandwidth oversubscription, and a downloadable CSV cable schedule.

## Topology model and NVIDIA RA alignment

Select **GB300 NVL72** or **Vera Rubin NVL72** at the top of the input panel. The planner follows NVIDIA Enterprise Reference Architecture (RA) compute-fabric principles while keeping its switch-count estimates configurable:

- Both profiles model one NVL72 rack as 18 compute trays with 4 GPU packages per tray (72 total). GPU positions 0–3 form four rails.
- **GB300** uses two independent compute planes with a default 400 Gb/s GPU-to-plane link. Each GPU is drawn once and its two plane links are shown with different line styles. Cisco Nexus models or a freeform ToR can be selected.
- **Vera Rubin** lets users select 1–8 active compute planes, defaulting to eight, at a default 200 Gb/s per GPU-plane link. Line patterns distinguish planes and rail colors identify the four GPU positions.
- The Vera Rubin profile includes NVIDIA Spectrum-X SN6600-LD, SN6810-LD, and SN6800-LD leaf options. SN6600-LD and SN6810-LD are modeled as one leaf group per plane; SN6800-LD groups four planes per leaf switch. The diagram shows a single rack set and routes the plane lanes to the corresponding leaves.

Leaf and spine counts are calculated from the selected interface counts and link speeds, including the non-blocking or oversubscription setting. Counts are capacity estimates, not an exact reproduction of an RA bill of materials, scalable-unit boundary, shuffle layer, or topology-specific rack layout.

## Cisco ToR models

Choose one of the Cisco Nexus ToR models in the app. The model selection sets the switch's front-panel data-port count and maximum per-port rate; the downlink and uplink rate inputs represent the configured native or breakout link speeds used in the estimate.

Select **Freeform** to enter a custom ToR port count directly. Freeform switch model and per-port speeds are user assumptions and are not validated against a hardware datasheet.

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

The Vera Rubin switch groupings and plane options are based on the supplied *NVIDIA Cloud Partner Reference Design – Vera Rubin NVL72 Systems with Spectrum-X, version 4* and are simplified for interactive capacity planning. In that reference, an NCP scalable unit has 14 racks; SN6600-LD/SN6810-LD use per-plane leaves and an external shuffle layer, while SN6800-LD leaf switches span four planes.

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
