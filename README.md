🧬 GWAS Pipeline for Quantitative Trait Analysis (GEMMA-based)

📌 Overview

This repository provides a fully reproducible Genome-Wide Association Study (GWAS) pipeline for analyzing quantitative traits using:

Whole-genome SNP data (VCF)
Phenotypic measurements (Excel)
Linear Mixed Models (LMM) via GEMMA

The workflow integrates:

Variant processing (PLINK)
Phenotype cleaning & transformation
Kinship estimation
Trait-wise GWAS
Statistical summaries
Publication-quality visualization

⚠️ Note: This study uses a small sample size (n = 5) and should be interpreted as an exploratory/pilot GWAS.

🧭 Workflow
RAW DATA (Assemblies)
        │
        ▼
Reference Genome Selection
        │
        ▼
Alignment (minimap2)
        │
        ▼
BAM Processing (samtools)
        │
        ▼
Variant Calling (bcftools)
        │
        ▼
SNP Filtering
        │
        ▼
Final GWAS-ready VCF
        │
        ▼
PLINK Conversion
        │
        ▼
Genotype QC
        │
        ▼
Phenotype Cleaning (Excel → matrix)
        │
        ▼
Kinship Matrix (GEMMA)
        │
        ▼
Trait-wise GWAS (LMM)
        │
        ▼
Summary Tables + Visualization
        │
        ▼
Manuscript-ready Figures
📁 Repository Structure
.
├── album_final_noF.vcf.gz        # Final filtered VCF
├── quantitative.xlsx            # Quantitative phenotype data
├── script.ipynb                 # Main analysis notebook
│
├── geno.*                       # PLINK files
├── geno_filtered.*              # Filtered PLINK files
│
├── output/                      # GEMMA outputs
│   ├── kinship.cXX.txt
│   ├── <trait>.assoc.txt
│   ├── <trait>.log.txt
│
├── trait_results/               # Processed GWAS results
├── trait_plots/                 # Basic plots
├── manuscript_figures/          # High-quality figures
└── trait_pheno_files/           # Per-trait phenotype records
⚙️ Requirements
Software
Python ≥ 3.8
PLINK v1.9
GEMMA v0.98.5
samtools, bcftools (upstream steps)
Python packages
pip install pandas numpy matplotlib openpyxl
📊 Input Data
1. Genotype Data
album_final_noF.vcf.gz
~702,016 SNPs
5 samples (Album_A–E)
2. Phenotype Data

File: quantitative.xlsx

Trait	Sample A	Sample B	Sample C	Sample D	Sample E
Plant Height (m)	0.6 ± 0.05	...	...	...	...

Traits:

Plant Height
Stem Breadth
Leaf Length
Leaf Breadth
Petiole Length
Petiole Breadth
Panicle Length
🧪 Analysis Pipeline
1. Convert VCF → PLINK
plink1.9 --vcf album_final_noF.vcf.gz \
          --double-id \
          --allow-extra-chr \
          --make-bed \
          --out geno
2. Genotype Filtering
plink1.9 --bfile geno \
          --geno 0.2 \
          --make-bed \
          --allow-extra-chr \
          --out geno_filtered
3. Phenotype Processing
Extract mean from values like 0.6 ± 0.05
Transpose matrix (samples × traits)
Match order with .fam
4. Kinship Matrix
./gemma -bfile geno_filtered \
         -gk 1 \
         -o kinship
5. Trait-wise GWAS
./gemma -bfile geno_filtered \
         -k output/kinship.cXX.txt \
         -lmm 4 \
         -o <trait_name>
6. Post-processing
Top SNP extraction
Summary tables
Cross-trait comparison
7. Visualization
Manhattan plots
QQ plots
Multi-panel figures
Vector graphics (PDF, SVG)
📈 Outputs
📊 Tables
File	Description
gwas_trait_summary.csv	GWAS summary
best_hit_per_trait.csv	Top SNP per trait
trait_significance_summary.csv	Significance stats
*_top20.csv	Top SNPs per trait
📊 Figures
Individual Manhattan plots
Individual QQ plots
Combined supplementary panels
Publication-ready (PNG, PDF, SVG)
⚠️ Limitations
Sample size: n = 5
No genome-wide significant SNPs
Results are:
exploratory
hypothesis-generating
not confirmatory
🧠 Interpretation

This pipeline identifies:

candidate loci
trait-specific SNP ranking
potential pleiotropy

⚠️ Validation in larger populations is required.

📝 Suggested Manuscript Statement

Genome-wide association analysis was performed using GEMMA v0.98.5 under a linear mixed model with a kinship matrix derived from filtered SNPs. Seven quantitative traits were analyzed independently. Due to the limited sample size (n = 5), results are considered exploratory and candidate loci were prioritized based on relative statistical significance.

🚀 How to Run
Upload files to Google Colab
Run script.ipynb sequentially
Download outputs
📦 Export Results
from google.colab import files
files.download("manuscript_high_quality_figures.zip")
🔮 Future Work
Increase sample size (≥100)
SNP → gene annotation
Functional enrichment
Multi-trait GWAS (mvLMM)
Experimental validation
👨‍🔬 Author Notes

This pipeline integrates:

population genomics
statistical genetics
reproducible computational workflows

Suitable for:

pilot GWAS studies
teaching
method development
📜 License

This project is licensed under the GNU GPL v3.
