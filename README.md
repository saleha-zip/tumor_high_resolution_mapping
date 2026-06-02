# High Resolution Mapping of the Tumor Microenvironment
### Reproduction of Janesick et al. (2023), Nature Communications

This repository contains the code, notebooks and reproduced figures for the paper **"High resolution mapping of the tumor microenvironment using integrated single-cell, spatial and in situ analysis"** published in Nature Communications (2023). The goal of this project is to understand the paper's methodology and verify its key findings by reproducing selected figures from the published datasets.

---

## Table of Contents

- [Paper Overview](#paper-overview)
- [Background](#background)
- [Methodology](#methodology)
- [Results](#results)
- [Repository Structure](#repository-structure)

---

## Paper Overview

Breast cancer is not a single uniform disease. Even within one tumor different regions behave differently at the molecular level and understanding those differences is critical for diagnosis and treatment. The core challenge has always been that existing technologies force a tradeoff. You can either profile gene expression in individual cells with high precision but lose all information about where those cells sit in the tissue, or you can study the tissue with spatial context but sacrifice single-cell resolution.

This paper resolves that tradeoff by combining three complementary technologies on serial sections of the same formalin-fixed paraffin-embedded (FFPE) breast cancer tissue block. These are Chromium Single Cell Gene Expression Flex (scFFPE-seq) for single-cell whole transcriptome profiling, Visium CytAssist for spatially resolved whole transcriptome data and Xenium In Situ for high-resolution targeted gene expression at subcellular spatial resolution. The central argument of the paper is that none of these technologies alone is sufficient. It is their integration that reveals biological insights that would otherwise remain hidden.

---

## Background

Before this study researchers working on tumor heterogeneity had to choose between depth and spatial context. Single-cell RNA sequencing could tell you exactly what genes a cell was expressing and separate millions of cells into distinct types. However once tissue is dissociated into individual cells for sequencing all spatial information is permanently lost. You no longer know whether two cell types were sitting next to each other or separated by millimeters of stromal tissue.

Spatial transcriptomics addressed this by keeping the tissue intact and attaching spatial coordinates to gene expression measurements. Technologies like Visium allowed researchers to map gene expression across a tissue section and overlay it with histology images. However each spatial spot captures a region of tissue containing multiple cell types mixed together making it difficult to assign expression patterns to specific cell types without additional reference data.

On the in situ side technologies like CosMx, MERSCOPE, Molecular Cartography and Xenium In Situ had recently become commercially available offering subcellular spatial resolution. The limitation is that they are targeted panels requiring prior knowledge to design a good gene panel which is where single-cell data becomes essential as a guide.

The gap this paper addresses is the integration of all three data types in a coherent mutually reinforcing pipeline applied specifically to FFPE tissue which is the preservation format used in the vast majority of clinical biobanks worldwide.

---

## Methodology

The study used a single FFPE breast cancer tissue block sectioned into serial 5 µm sections. Each section was run through one of the three platforms. This serial sectioning approach ensured all three technologies were measuring the same biological sample from nearly identical tissue slices.

**scFFPE-seq** used RNA templated ligation technology designed specifically to overcome formalin-induced RNA degradation. This produced whole transcriptome single-cell data from 27,472 cells organized into 17 distinct clusters with a median of 1,480 genes identified per cell.

**Visium CytAssist** used a probe set targeting 18,536 genes across 54,018 probes applied to spatially barcoded spots on a capture array. The CytAssist instrument transferred analytes from a standard glass slide to the Visium array allowing standard H&E staining to be performed first. This produced spatially resolved whole transcriptome data from 4,992 spots with a median of 5,712 genes per spot.

**Xenium In Situ** used a targeted panel of 313 genes selected based on single-cell atlas data for human breast tissue. Rolling circle amplification generated strong signal-to-noise ratios for each transcript and the Xenium Analyzer decoded gene identity across 167,885 cells with a median of 166 transcripts per cell.

Integration across the three platforms was achieved through image registration using RANSAC homography and a spot interpolation method that binned Xenium transcript coordinates into the nearest Visium spots by spatial proximity.

---

## Results

The integration of the three technologies produced several findings that no single platform could have achieved alone.

The scFFPE-seq data established a cellular reference of 17 annotated cell types including two molecularly distinct forms of ductal carcinoma in situ (DCIS #1 and DCIS #2) as well as invasive tumor cells, two myoepithelial subtypes, immune cells, stromal cells and endothelial cells.

The Visium data spatially located these tumor subtypes on the tissue and revealed their spatial organization relative to the H&E morphology. Ten of the 17 Visium clusters were unequivocally assigned to a single cell type while the remaining seven reflected spots with mixed cell type compositions in regions where multiple populations coexist in close proximity.

The Xenium data provided subcellular resolution that revealed a small triple-positive DCIS region expressing ERBB2, ESR1 and PGR simultaneously. This region was so sparse it was invisible in the Visium data without Xenium guidance. Using spot interpolation the corresponding Visium spots were identified and whole transcriptome differential expression analysis revealed 48 and 44 differentially expressed genes compared to the two PGR-negative DCIS subtypes respectively.

In a second tissue sample Xenium identified rare boundary cells co-expressing both tumor markers (ERBB2 and ABCC11) and myoepithelial markers (MYLK and DST) at the DCIS-to-invasive transition zone. These cells were then located in the scFFPE-seq data and whole transcriptome profiling identified CX3CL1, CCL28, PROM1 and KLK5 as uniquely expressed in this population.

---

## Repository Structure

```
tumor_high_resolution_mapping/
│
├── README.md
├── LICENSE
│
├── figures/
│   ├── figure2b_spatial_clusters.png
│   ├── figure2c_combined_panel.png
│   ├── figure2c_SCGB2A2.png
│   ├── figure2c_CPB1.png
│   ├── figure2c_KRT17.png
│   ├── figure2c_FABP4.png
│   ├── figure2c_IL2RG.png
│   ├── figure2c_SFRP2.png
│   ├── figure2c_CDH2.png
│   ├── figure2c_MT-ND1.png
│   ├── marker_genes_all_clusters.png
│   ├── pca_variance_ratio.png
│   ├── qc_after_filtering.png
│   ├── qc_before_filtering.png
│   └── tsne_17clusters_annotated.png
│
└── notebooks/
    └── Visium_Analysis.ipynb
```
---

## Member 1 — scRNA-seq Single Cell Foundation

> **Placeholder** — to be completed by Member 1.
>
> This section will cover the scFFPE-seq pipeline, quality control, dimensionality reduction and reproduction of Figure 2a showing the 17 annotated cell type clusters.

---

## Visium CytAssist Spatial Analysis

### What This Section Covers

This section covers the Visium CytAssist spatial transcriptomics pipeline. The goal was to load and process the published Visium dataset for the FFPE breast cancer sample, apply the official cell type annotations provided by the authors and reproduce the spatial cluster map and spatial gene expression plots from Figures 2b and 2c of the paper.

### Why Spatial Context Matters

The scFFPE-seq data produced by single-cell sequencing tells you a great deal about what cell types are present in a tumor and what those cells are expressing but it cannot tell you anything about where those cells are located relative to each other. That spatial relationship is not a minor detail. In cancer biology the neighborhood a cell lives in directly influences its behavior. A tumor cell sitting at the edge of a duct surrounded by myoepithelial cells is in a fundamentally different microenvironment than one that has already broken through into the surrounding stroma and you cannot observe that distinction from dissociated single-cell data alone.

Visium CytAssist fills this gap by preserving the tissue architecture and attaching spatial coordinates to gene expression measurements. Every sequenced spot on the Visium slide corresponds to a physical location on the tissue section and that location can be overlaid directly on the H&E image of the same tissue. This makes it possible to see exactly which duct a cluster of spots expressing a DCIS marker corresponds to and to map molecular heterogeneity onto recognizable histological structures.

### Data and Files Used

The following files were downloaded from the 10x Genomics dataset page for this paper:

| File | Purpose |
|------|---------|
| `CytAssist_FFPE_Human_Breast_Cancer_filtered_feature_bc_matrix.h5` | Gene expression matrix |
| `CytAssist_FFPE_Human_Breast_Cancer_spatial.tar.gz` | Tissue image and spot coordinates |
| `CytAssist_FFPE_Human_Breast_Cancer_analysis.tar.gz` | Pre-computed clustering |
| `Cell_Barcode_Type_Matrices.xlsx` | Official cell type annotations from the authors |

The annotation Excel file contains a Visium sheet with three columns: barcode, cluster number and cell type annotation. These are the exact annotations used in the paper so no manual cluster labeling was needed.

### Pipeline Summary

The data was loaded using `scanpy.read_visium` which reads the expression matrix and spatial folder together registering the H&E image and spot coordinates in a single AnnData object. After verifying the tissue image loaded correctly standard preprocessing was applied including gene filtering to remove genes detected in fewer than 3 spots, normalization to 10,000 counts per spot and log transformation.

Cell type annotations were loaded from the Excel file and mapped onto each spot barcode. The Visium dataset contains 4,992 spots under tissue covering 11 annotated cell type categories. These include DCIS #1, DCIS #2, invasive tumor, adipocytes, immune cells, stromal cells, stromal/endothelial cells and mixed composition spots representing regions where multiple cell types coexist within the 55 µm capture diameter of a single spot.

### Understanding the Mixed vs Unequivocal Clusters

The distinction between the unequivocally annotated spots and the mixed spots is not a limitation of the data processing. It is an honest reflection of the tissue biology. In a tumor section many cell types are physically intermingled. A spot sitting at the edge of a duct simultaneously captures transcripts from the ductal epithelium inside, the myoepithelial layer wrapping around it and the stromal cells just outside. No spatial technology at this resolution can cleanly separate those signals without additional single-cell reference data which is exactly why the scFFPE-seq annotations were essential for interpreting the Visium output.

### Reproduced Figure 2b — Spatial Cluster Map

The figure below reproduces the spatial cluster map from Figure 2b of the paper. Each circle represents one Visium spot and the color indicates the cell type annotation assigned by the authors. The invasive tumor region (red) is clearly visible in the upper right portion of the tissue. The large yellow cluster on the left corresponds to adipocytes. DCIS #1 (blue) sits at the bottom center and the stromal and endothelial populations spread across large portions of the tissue. Mixed spots (grey) appear where multiple cell types coexist in close spatial proximity.

![Figure 2b Reproduction — Visium Spatial Clusters](figures/figure2b_spatial_clusters.png)

This pattern is consistent with Figure 2b in the paper. The spatial organization of the tumor subtypes relative to each other and to the surrounding stromal and immune compartments matches the published result.

### Reproduced Figure 2c — Spatial Marker Gene Expression

Figure 2c from the paper shows spatial expression maps for eight marker genes each representing a different cell type. The combined panel below reproduces all eight maps.

![Figure 2c Reproduction — Combined Panel](figures/figure2c_combined_panel.png)

Each gene is also shown individually below with a brief interpretation of what the spatial pattern confirms.

**SCGB2A2 — DCIS #1 marker**

![SCGB2A2](figures/figure2c_SCGB2A2.png)

SCGB2A2 expression is concentrated in the lower portion of the tissue in a spatially coherent cluster that overlaps with the DCIS #1 annotation in Figure 2b. This confirms that this ductal region has a molecularly distinct identity from the rest of the tumor.

**CPB1 — DCIS #2 marker**

![CPB1](figures/figure2c_CPB1.png)

CPB1 shows a more dispersed pattern across the tissue with hotspots in the upper corners and scattered foci throughout. This is consistent with the paper's description of DCIS #2 as a region with invasive carcinoma lesions scattered throughout the stromal connective tissue.

**KRT17 — Myoepithelial marker**

![KRT17](figures/figure2c_KRT17.png)

KRT17 expression forms thin elongated clusters that wrap around ductal structures visible in the H&E image. This ring-like spatial pattern is exactly what would be expected for the myoepithelial layer which physically surrounds the ductal epithelium as a boundary cell population.

**FABP4 — Adipocyte marker**

![FABP4](figures/figure2c_FABP4.png)

FABP4 is strongly expressed in the left portion of the tissue in a large contiguous region that corresponds to the yellow adipocyte cluster in Figure 2b. This confirms that Visium successfully captured adipocyte transcripts despite the fact that adipocytes are notoriously difficult to recover with dissociation-based single-cell methods because they rupture during tissue processing.

**IL2RG — Immune cell marker**

![IL2RG](figures/figure2c_IL2RG.png)

IL2RG expression is scattered as small foci across the tissue rather than forming large contiguous regions. This dispersed pattern is consistent with the biology of immune infiltration in breast tumors where lymphocytes and macrophages are distributed throughout the tumor microenvironment rather than forming solid masses.

**SFRP2 — Stromal marker**

![SFRP2](figures/figure2c_SFRP2.png)

SFRP2 is broadly expressed across a large central region of the tissue with areas of high expression forming an interconnected stromal network. This broad distribution reflects the stromal compartment that surrounds and separates the ductal and invasive tumor regions.

**CDH2 — Invasive tumor marker**

![CDH2](figures/figure2c_CDH2.png)

CDH2 shows strong expression concentrated in the upper right quadrant of the tissue with a clear boundary separating high-expressing spots from low-expressing spots. This high-expressing region spatially overlaps with the invasive annotation in Figure 2b confirming that CDH2 marks the invasive front of the tumor.

**MT-ND1 — Mitochondrial / Invasive**

![MT-ND1](figures/figure2c_MT-ND1.png)

MT-ND1 is broadly expressed across the tissue but with the highest intensity in the invasive tumor region. The paper noted that mitochondrial gene expression correlates with the invasive region which is consistent with the elevated metabolic activity expected in actively invading cancer cells. This pattern is reproduced here.

### Key Statistics Verified

| Metric | Paper Reports | Reproduced |
|--------|--------------|------------|
| Spots under tissue | 4,992 | 4,992 |
| Median genes per spot | 5,712 | 5,712 |
| Mean reads per spot | 40,003 | 40,003 |
| Cell type categories | 11 | 11 |
| Genes in probe set | 18,536 | 18,085 (after filtering) |

### Notebook

The full analysis notebook is available at:
`notebooks/Visium_Analysis.ipynb`

---

## Member 3 — Differential Gene Expression

> **Placeholder** — to be completed by Member 3.
>
> This section will cover the differential gene expression pipeline, triple-positive DCIS region identification and reproduction of Figure 4d showing the dot plot of canonical marker genes across tumor subtypes.

---

## Member 4 — Xenium In Situ

> **Placeholder** — to be completed by Member 4.
>
> This section will cover the Xenium in situ pipeline, cell segmentation, subcellular spatial resolution analysis and reproduction of Figure 3b showing spatial gene expression maps at single-cell resolution.

---

## Data Availability

All datasets used in this reproduction are publicly available:

- Processed Visium and Xenium datasets: [10x Genomics Human Breast Preview Dataset](https://www.10xgenomics.com/products/xenium-in-situ/preview-dataset-human-breast)
- Raw sequencing data: [GEO accession GSE243280](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE243280)
- Author companion code: [GitHub — 10XGenomics/janesick_nature_comms_2023_companion](https://github.com/10XGenomics/janesick_nature_comms_2023_companion)

---

## Citation

Janesick A, Shelansky R, Gottscho AD, et al. High resolution mapping of the tumor microenvironment using integrated single-cell, spatial and in situ analysis. *Nature Communications*. 2023;14:8353. https://doi.org/10.1038/s41467-023-43458-x
