# TreeWatch Vienna

**Street-Level Tree Canopy and Vitality Monitoring with Deep Learning**

Akhil Pillai (Matr. No. 12402165) · Applied Deep Learning, TU Wien, WS2026
Assignment 1: Initiate

---

## Idea

Take an ordinary phone photo of a street or park tree in Vienna and have a model fill in its "passport": genus/species, size (height, trunk circumference, crown diameter), approximate age, and (stretch goal) vitality. Inspectors then only visit the trees the model flags.

The core trick: **labels come almost for free.** Vienna publishes an open tree register, so a GPS-tagged photo can be matched to its register entry and inherit that tree's attributes as labels, with no manual annotation.

**Motivation:** the EU Nature Restoration Law (Reg. (EU) 2024/1991, Art. 8) requires no net loss of urban tree canopy by 2030 vs. 2024, then an increasing trend. Cities need a cheap, reliable way to monitor their trees.

## Project type

**Bring your own data.** A new dataset of geo-tagged Vienna tree photos, linked to the open register, with a trained baseline for others to compare against. The anonymised dataset will be published at the end of the course. Secondary goal: a "beat the classics" comparison against non-deep-learning baselines.

## Dataset

- **Baumkataster Wien** (open data, 233,102 trees, export checked 26.10.2026): GPS + species for every tree; trunk circumference in cm; height and crown diameter in 8 size classes each; planting year (missing for ~28%).
- **Own phone photos**: 1,500–3,000 images of 800–1,500 trees, covering the 20 most common genera (90% of the register).
- **Mapillary** (CC-BY-SA): summer, in-leaf views of the same trees for the vitality subset.

Labels are generated automatically by GPS + compass matching; only vitality is labelled by hand. Split by tree and by area (one district held out).

## Approach

Pretrained backbone (ResNet-50 / DINOv2) fine-tuned with multi-task heads:

- **Genus** classification (20 genera + other), species as a finer head
- **Trunk circumference** regression (cm, L1)
- **Height** and **crown diameter** as ordinal classification (8 classes)
- **Age** regression, masked where planting year is missing

*Stretch goals (after the core works):* vitality via U-Net foliage/wood segmentation (following Fatima et al., 2026), and multi-photo feature fusion.

**Baselines:** register-only (predict size from species+age, no image), classical vision (color/texture features + Random Forest), Pl@ntNet as an external species reference.

**Evaluation:** top-1/top-5 accuracy (genus, species); MAE (circumference, age); exact and ±1-class accuracy (height, crown); IoU and R² for the vitality stretch goal. Grad-CAM for interpretability.

## Work-breakdown structure

| Task | Hours |
|---|---|
| Literature research and planning | 8 |
| Dataset collection (register, photo walks, Mapillary) | 25 |
| Data pipeline (GPS matching, cleaning, anonymisation, splits) | 15 |
| Network(s) and baselines | 15 |
| Training, fine-tuning, experiments | 25 |
| Web application | 15 |
| Final report | 10 |
| Presentation | 6 |
| **Core total** | **119** |
| Stretch: vitality labelling and segmentation | 10 |
| **Total incl. stretch** | **129** |

## Deadlines

- Assignment 1 (Initiate): 27.10.2026
- Assignment 2 (Hacking): 15.12.2026
- Assignment 3 (Deliver): 12.01.2027

## References

1. Regulation (EU) 2024/1991 on nature restoration, OJEU, 2024.
2. Branson et al., "From Google Maps to a fine-grained catalog of street trees," *ISPRS J. Photogramm. Remote Sens.*, 135, 2018.
3. Beery et al., "The Auto Arborist dataset," *CVPR*, 2022.
4. Fatima et al., "Integrated deep learning framework for automating tree health assessment using ground-based images," *Urban Forestry & Urban Greening*, 118, 2026, doi:10.1016/j.ufug.2026.129345.

---

*See the Report (PDF) in this repository for details.*
