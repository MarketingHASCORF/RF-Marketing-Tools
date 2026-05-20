# HASCO Bulk Datasheet Processor

**Live tool:** [https://yourusername.github.io/hasco-tools](https://yourusername.github.io/hasco-tools)
*(Replace with your actual GitHub Pages URL after setup)*

A browser-based internal tool for HyTech Associates / HASCO Components that extracts structured product data from PDF datasheets and exports it as a standardized CSV — ready for import into Acumatica, BigCommerce, or Excel.

---

## What It Does

1. **Upload** up to 500 HASCO product PDF datasheets at once
2. **AI reads each PDF** — electrical specs, material specs, connector details, certifications
3. **Extracts all fields** into a consistent 48-column structure, one row per part number
4. **Export to CSV** — download anytime, even mid-batch

Supports all HASCO product types: cable assemblies (HLL series, VNA, phase stable, ruggedized, low loss, armored), adapters, attenuators, terminations, and connectors.

---

## How to Use

### First time
1. Open the tool URL in Chrome, Edge, or Firefox
2. Bookmark it — the URL never changes

### Running a batch
1. **Drag and drop** up to 500 PDF datasheets onto the upload zone, or click to browse
2. Click **Process All PDFs** — the tool processes files one at a time
3. Watch the live log as each file is extracted and parsed
4. Click **Export CSV** to download the results — you can do this at any point, even mid-batch

### Pause & Resume
- Click **Pause** at any time — the current file finishes, then processing holds
- Click **Resume** to continue from where you left off
- Your progress is auto-saved every 5 files

### If the browser closes unexpectedly
- Reopen the tool — a green **"Saved session found"** banner will appear
- Click **Restore** to reload all previously extracted rows back into the table
- Re-upload only the files that hadn't been processed yet and continue

### Retrying errors
- If some files fail (network issue, unusual PDF format), a **Retry Errors** button appears after the batch finishes
- It resets only the failed files and re-runs just those — no need to reprocess the whole batch

---

## Output CSV — Column Reference

The exported CSV has **48 columns**. Every row is one part number.

### Identity
| Column | Description | Example |
|--------|-------------|---------|
| `part_number` | HASCO part/model number | `HLL185R-VP-VJ-L` |
| `product_name` | Full descriptive name from datasheet title | `1.85mm Male to 1.85mm Female Low Loss Ruggedized Test Cable - DC to 67 GHz` |
| `subcategory` | Product category (inferred) | `Ruggedized Cables - Low Loss Phase Stable` |
| `series` | Product family prefix | `HLL185R` |

### Connectors
| Column | Description | Example |
|--------|-------------|---------|
| `connector_a` | Port 1 interface type | `1.85mm` |
| `connector_b` | Port 2 interface type | `1.85mm` |
| `connector_a_gender` | Port 1 gender | `Male` |
| `connector_b_gender` | Port 2 gender | `Female` |

### Electrical Specifications
| Column | Description | Example |
|--------|-------------|---------|
| `frequency_range_ghz` | Operating frequency range | `DC - 67` |
| `max_freq_ghz` | Maximum frequency (numeric) | `67` |
| `vswr_max` | Maximum VSWR | `1.50:1` |
| `insertion_loss_formula` | Insertion loss specification | `≤4.0 x f(GHz) dB` |
| `bending_phase_stability` | Phase stability under bending | `≤±6.5° @ 67 GHz` |
| `phase_stability` | Amplitude/phase stability | `±0.15 dB @ 67 GHz` |
| `insulation_resistance` | Insulation resistance | `≥5000 MΩ` |
| `capacitance` | Capacitance per unit length | `27pf/ft = 88pf/m` |
| `rf_leakage_db` | RF leakage in dB | `90` |
| `temp_range` | Operating temperature range | `-55°C to 125°C` |
| `bend_radius_mm` | Minimum bend radius | `30` |
| `impedance_ohms` | Characteristic impedance | `50` |
| `durability_cycles` | Flex cycle rating | `>5000` |
| `armor_stress_tolerance` | Armor mechanical tolerance | `90N / 25mm` |
| `velocity_pct` | Velocity of propagation % | `76` |
| `screening_effectiveness_db` | Screening effectiveness in dB | `90` |
| `armor_outer_diameter_mm` | Armor outer diameter | `4.7` |
| `weight_kg_per_m` | Cable weight per meter | `0.038` |

### Materials
| Column | Description | Example |
|--------|-------------|---------|
| `connector_body_material` | Connector body | `Passivated Stainless Steel` |
| `connector_dielectric` | Connector dielectric | `PEI` |
| `connector_center_contact` | Center contact material | `Gold plated BeCu` |
| `cable_center_conductor` | Cable center conductor | `Silver plated Copper` |
| `cable_dielectric` | Cable dielectric | `LDPTFE` |
| `inner_shield` | Inner shield construction | `Silver plated flat Copper wire braid` |
| `middle_shield` | Middle shield (if present) | `LDPTFE` |
| `outer_shield` | Outer shield construction | `Silver plated Copper wire braid` |
| `jacket` | Jacket material | `FEP` |
| `inner_armor` | Inner armor (ruggedized only) | `Silver plated Copper` |
| `strengthened_shield` | Strengthened shield layer | `Silver plated Copper wire` |
| `anti_twist_layer` | Anti-twist layer | `Polyimide` |
| `outer_armor` | Outer armor | `PTFE Braid` |

### Compliance & Admin
| Column | Description | Example |
|--------|-------------|---------|
| `rohs_compliant` | RoHS compliance | `Yes` |
| `cage_code` | CAGE code | `0T8L4` |
| `drawing_rev` | Drawing revision | `B` |

### To Be Filled (after BigCommerce / Acumatica export)
| Column | Description |
|--------|-------------|
| `product_url` | hascoRF.com product page URL |
| `datasheet_url` | Direct link to hosted PDF |
| `image_url` | Product image URL |
| `price_usd` | List price |
| `inventory_tier` | T1 (In Stock) / T2 (Fast Build) / T3 (CTO) |
| `notes` | Revision history, special notes |

---

## Tips for Best Results

- **One datasheet per PDF** — multi-product PDFs will extract only the first product's data
- **Text-based PDFs work best** — scanned image-only PDFs may extract fewer fields
- **Batch by product family** — processing all HLL series together makes it easy to spot gaps in the CSV
- **Export frequently** — click Export CSV after every 50–100 files so you have incremental backups
- **Part number in filename** — if a PDF's part number can't be extracted, the tool falls back to the filename, so naming files like `HLL185R-VP-VJ-L.pdf` gives you a clean fallback

---

## Technical Notes

- Runs entirely in the browser — no server, no database
- PDFs are sent to the [Anthropic API](https://www.anthropic.com) for text extraction and structured parsing
- No PDF content is stored anywhere — it is processed in memory and discarded after each extraction
- Auto-save uses browser `localStorage` — clearing browser data will clear the saved session
- Tested in Chrome 120+, Edge 120+, Firefox 121+

---

## Updating the Tool

1. Download the latest `index.html` from whoever manages the tool (HyTech IT / marketing)
2. In the GitHub repo, click `index.html` → **Edit** (pencil icon) → paste new content → **Commit changes**
3. The live URL updates within ~60 seconds

---

## Repository Structure

```
hasco-tools/
├── index.html     ← The bulk datasheet processor (the tool itself)
└── README.md      ← This file
```

---

## Contact

**HyTech Associates, Inc.**
5214 Bonsai Avenue, Moorpark, CA 93021
sales@hasco-inc.com · 888-498-3242 · [hasco-inc.com](https://www.hasco-inc.com)
