# VCF QC Workflow

**Author:** Jeongmin Kim

This repository showcases genotype quality control (QC) and principal component
analysis (PCA) outputs from an R and PLINK 2 workflow. The demonstration uses
a synthetic multisample VCF with 100 samples and 1,000 SNPs across four
simulated groups.

## Review the demo

- [QC report](examples/four_groups/05_report/qc_report.md)
- [Sample inventory](examples/four_groups/03_tables/sample_inventory.csv)
- [PCA scree plot](examples/four_groups/04_figures/pca_scree.png)
- [PC1 vs PC2](examples/four_groups/04_figures/pca_pc1_pc2.png)
- [PC2 vs PC3](examples/four_groups/04_figures/pca_pc2_pc3.png)

## Quality control

| Step | Demo setting | Action |
| --- | --- | --- |
| VCF import | Autosomal biallelic A/C/G/T SNPs | Convert eligible variants to PLINK 2 binary format |
| Baseline missingness | Before filtering | Record per-sample and per-variant missingness |
| Sample missingness | Maximum 10% | Exclude samples above the threshold before variant QC |
| Variant missingness | Maximum 10% | Exclude variants above the threshold after sample QC |
| Minor allele frequency (MAF) | Minimum 5% | Exclude variants below the threshold |
| Linkage disequilibrium (LD) pruning | 650 kb window, r² threshold of 0.20 | Select the SNP subset used for PCA |
| PCA | 10 components with mean imputation | Mean-impute remaining missing genotype calls during PCA |

The workflow was verified with PLINK v2.0.0-b.1.4 M1 (7 Nov 2026).

## Demo results

- **Samples.** 99 of 100 passed QC. `DEMO100` was excluded because its genotype
missingness was 20%.
- **SNPs.** 998 of 1,000 passed variant QC. One SNP exceeded the missingness
threshold, and one monomorphic SNP fell below the MAF threshold.
- **PCA.** Ten PCs were computed. The largest adjacent eigenvalue drop was
between PC3 and PC4, consistent with the three group contrasts in the simulation.
- **Sample inventory.** Each input sample has one row with its simulated group,
pre-QC missingness, QC status, and PC scores where available.

## Planned QC extensions

- **Laboratory metadata.** Flag DNA quantity below 200 ng and lab-reported
call rate below 97% when those measurements are available.
- **Sample QC.** Add sex concordance, relatedness, and heterozygosity checks,
and evaluate a 5% sample missingness threshold.
- **Variant QC.** Evaluate a 5% variant missingness threshold, a minimum MAF
of 1%, and Hardy–Weinberg equilibrium (HWE) filtering at p ≤ 1 × 10⁻⁶
in a defined sample set.
- **Post-imputation QC.** For reference-panel imputation outputs, evaluate
filters for Rsq below 0.80, MAF below 1%, and minor allele count below 5.

## Research lab services

I help research teams apply their genotype QC criteria to multisample VCFs
and deliver a sample inventory, PCA diagnostics, and a concise QC report.
A pilot can start with one dataset and a review of your lab's existing
QC criteria. If your team runs the workflow internally, I can provide
scripts and help set them up in your lab's computing environment.

For project inquiries, email [Jeongmin Kim](mailto:jeongmin.kim.work@gmail.com).