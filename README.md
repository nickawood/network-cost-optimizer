# Network Cost Optimizer Skill for Antigravity & Gemini

**Network Cost Optimizer** converts a customer's raw circuit inventory (leased lines, physical cross-connects, WAN links, exchange drops) into a defensible Google Cloud networking modernization and cost-savings case.

Built as an agent skill for Google Antigravity and Gemini Enterprise for SPARK Days.

## Overview

When presented with a network inventory spreadsheet (XLSX or CSV), this skill enables AI agents to:
1. **Parse & Classify Circuits**: Categorize physical cross-connects (XC), local loops, long-haul GWAN links, and M&A duplicates.
2. **Build Savings Models**: Apply industry baseline Monthly Recurring Costs (MRC) and estimate potential percentage reductions.
3. **Map to Google Cloud Networking**: Map legacy connectivity to modern GCP equivalents (**Private Service Connect**, **Dedicated Interconnect**, **Network Connectivity Center**, **Cross-Cloud Interconnect**).
4. **Cover All Client Postures**: Ensure recommendations accommodate GCP-native, multi-cloud (AWS/Azure), and on-premise clients.
5. **Differentiate & Defend**: Provide sharp competitive positioning vs AWS Direct Connect and Azure ExpressRoute.
6. **Generate Phased Migration Plans**: Provide a 24-week phased migration roadmap.
7. **Flag Critical Pitfalls**: Avoid common traps like double-run telecom costs, egress/NAT fees, and assumption of 100% cloud readiness.

## Structure

```
network-cost-optimizer/
├── README.md
├── plugin.json
└── skills/
    └── network-cost-optimizer/
        └── SKILL.md
```

## Installation

### In Antigravity

Clone this repository into your Antigravity plugins directory:

```bash
git clone https://github.com/nickawood/network-cost-optimizer.git ~/.gemini/config/plugins/network-cost-optimizer
```

Restart Antigravity. The `/network-cost-optimizer` skill will be available automatically.

### In Gemini Enterprise

1. Copy the contents of [`skills/network-cost-optimizer/SKILL.md`](skills/network-cost-optimizer/SKILL.md).
2. Navigate to **Gemini Enterprise > Custom Skills / System Instructions**.
3. Create a new skill named `network-cost-optimizer` and paste the contents.

## Usage

Upload or reference a network circuit inventory spreadsheet (XLSX/CSV) in conversation with your agent and prompt:
> *"Analyze this circuit inventory and build a Google Cloud cost-savings and networking modernization case."*

## License

Apache-2.0
