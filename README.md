TFBS Analyzer

Overview

This Analyzer is an end-to-end in silico bioinformatics pipeline designed to elucidate the transcriptional regulatory network of any user-defined target gene.
By integrating automated genomic sequence retrieval, dual predictive motif scanning, Machine Learning (Random Forest) filtering, and systems biology annotation via MaayanLab Enrichr, this custom script identifies clinically relevant Transcription Factor Binding Sites (TFBS). It includes a specialized focus in isolating regulatory targets with confirmed clinical associations.

Pipeline Architecture

The workflow is divided into modular phases to ensure high reproducibility and biological viability:
Automated Genomic Extraction: Interacts directly with the Ensembl REST API to retrieve precise proximal and distal promoter sequences (e.g., -2000 to +500 bp relative to the TSS) based on the provided Ensembl ID.
Dual Motif Scanning: Dynamically fetches Position Weight Matrices (PWMs) from the complete JASPAR CORE human catalog and validates them against HOCOMOCO v11, applying strict conservation affinity thresholds (>82%).
ML Filter: Mitigates the high false-positive rate of traditional PWM scanning using a Random Forest classifier. It evaluates the 3D chromatin accessibility and epigenetic viability of the local genomic context based on CpG dinucleotide density and GC transitions in the flanking regions.
Systems Biology Integration: Submits the validated transcription factors to the MaayanLab Enrichr API in real-time, mapping hijacked signaling pathways (KEGG 2021 Human) and clinical disease associations (DisGeNET).

Prerequisites

The pipeline is built entirely in Python (v3.x). Ensure you have the following open-source libraries installed:
pip install pandas numpy scikit-learn biopython requests urllib3


Configuration & Usage

The script is designed as a standalone, zero-configuration executable. To analyze your gene of interest, modify the USER CONFIGURATION at the top of the script:
# Target Gene Ensembl ID
TARGET_GENE_ID =

# Promoter window coordinates (base pairs)
UPSTREAM_BP =    
DOWNSTREAM_BP =    

# Minimum affinity threshold for the primary PWM scan
JASPAR_THRESHOLD = 

Once configured, run the script from your terminal


Output Files

The pipeline generates comprehensive CSV reports in your working directory:
Base_Results_[Gene].csv: Consolidated catalog of all mathematically viable TFBS, maintaining sequence data and ML confidence scores.
Oncological_Filter_[Gene].csv: Subset containing TFs with direct clinical associations according to DisGeNET.
Metabolic_Pathways_[Gene].csv: Subset of TFs associated with general metabolic and signaling pathways mapped via KEGG.

Scientific Justification
This pipeline architecture is built upon validated computational frameworks. The integration of PWMs with Random Forest classifiers utilizing flanking sequence features (GC content) significantly outperforms classical motif scanning by capturing sequence-dependent DNA structural characteristics, chromatin accessibility, and nucleosome occupancy.

References:
Arvey, A., Agius, P., Noble, W. S., & Leslie, C. (2012). Sequence and chromatin determinants of cell-type–specific transcription factor binding. Genome Research, 22(9), 1723-1734. https://doi.org/10.1101/gr.127712.111
Hooghe, B., Broos, S., Van Roy, F., & De Bleser, P. (2012). A flexible integrative approach based on random forest improves prediction of transcription factor binding sites. Nucleic Acids Research, 40(14), e106. https://doi.org/10.1093/nar/gks283

License
This project is licensed under the MIT License.
