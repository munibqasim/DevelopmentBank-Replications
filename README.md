Project Similarity Detector
This repository contains tools and methodologies for detecting similar projects across development banks, with a focus on identifying replications using a combination of KDE (Kernel Density Estimation) and Z-score methods.
Overview
The Project Similarity Detector identifies similar projects using two complementary methods:

Sector-Adjusted Z-Score of Minimum Distance: Detects anomalously similar project pairs within the same sector
Kernel Density Estimation (KDE): Identifies regions of high project density in the embedding space

Core Components
1. Project Similarity Detector
The main class that implements the similarity detection algorithms:

Precomputes sector statistics at project level
Trains KDE models for dimensionality reduction
Computes pairwise similarities between projects
Creates visualizations for analysis

pythondetector = ProjectSimilarityDetector(
    umap_dim=15, 
    z_score_threshold=2.0,
    kde_percentile=70,
    use_project_level=True
)
2. Data Loader Module
Provides functions for loading embeddings from FAISS indices and metadata files for various development banks:

load_embeddings_from_faiss: Loads embeddings for a specific bank type (WorldBank, IDB, AFDB, AIIB, ADB)

3. Analysis Modules
Intrabank Analysis

run_intrabank_analysis: Analyzes similarity between projects within the same development bank
run_worldbank_replication_analysis: Specifically analyzes World Bank projects for replications

Interbank Analysis

analyze_cross_bank_similarity: Compares projects across different development banks

Methodology
Step 1: Embedding Preparation

Projects are represented by their median embedding vectors
Embeddings are normalized to unit vectors for cosine distance calculations

Step 2: Sector Statistics

For each sector, compute the mean and standard deviation of pairwise distances
These statistics are used to calculate sector-adjusted Z-scores

Step 3: KDE Modeling

Apply UMAP for dimensionality reduction to 15D
Train KDE models for each sector
Calculate a density threshold at 70th percentile

Step 4: Similarity Scoring

For each project pair, compute:

Cosine distance
Z-score = (sector_mean_dist - pair_dist) / sector_std_dist
KDE density values


Flag pairs with Z-score ≥ 2.0 and high density values

Project Replication Classification
Projects are classified into two main categories:

Original/Replicated: Projects that others have copied from
Replicating: Projects that copied from others

Additional analyses include:

Temporal distribution of replications
Statistics on replication time differences
Distribution of original projects by year

Running the Analysis
python# Example: Run World Bank replication analysis
run_worldbank_replication_analysis()

# Example: Analyze cross-bank similarity
analyze_cross_bank_similarity(metadata_file, faiss_index_file, 
                             "WorldBank", ["IDB", "AFDB", "AIIB", "ADB"], 
                             output_dir)
Visualization
The tool generates several visualizations:

Z-score distributions
Temporal distribution of replications
Yearly breakdown of original projects
