# cRab Package 

**cRab** (Clustering Rivals and Buddies) is an R package for evaluating clustering solutions using resampling-based stability. Rather than relying solely on cluster shape or separation, as many traditional clustering evaluation methods do, cRab implements the **CRAB** (Clustering Rivals and Buddies) algorithm to measure how consistently observations are grouped together across repeated clustering runs. 

The CRAB algorithm introduces the CRAB score, a stability metric based on **Buddies** (observations assigned to the same cluster) and **Rivals** (observations assigned to different clusters). The resulting score provides a quantitative way to compare clustering algorithms and evaluate tuning parameter choices based on the stability of their clustering solutions. 

# Installation 

Install via GitHub:

```{r}
install.packages("remotes")
remotes::install_github("jasminecabrera/cRab-Package")
```

# Main Functions 
| Function | Description | 
|----------|-------------| 
| `buddies_matrix()` | Construct the buddies matrix from repeated clustering results. | 
| `mean_matrix()` | Compute the average similarity matrix across clustering runs. | 
| `similarity_score()` | Calculate the CRAB score. | 
| `crab_prep()` | Prepare clustering output for analysis. | 
| `crab_cols()` | Select clustering result columns. | 
| `crab_sim_params()` | Generate combinations of simulation parameters. | 
| `crab_bake()` | Execute the full CRAB workflow from prepared inputs and return CRAB scores across tested k values. | 
| `simulate_mvn()` | Simulate multivariate normal datasets with a specified number of clusters. | 
| `repeat_simulations()` | Run the CRAB algorithm repeatedly across multiple simulated datasets, allowing evaluation of clustering stability across different variance settings and repeated experiments. | 

# Getting Started

Load the package:

```{r}
library(cRab)
```

## Real Data Example

```{r}
palmerpenguins::penguins |>
crab_prep() |>
crab_cols(c("flipper_length_mm", "bill_length_mm")) |>
crab_params(min_k = 2, max_k = 5, num_subsamples = 30, cluster_method = "kmeans") |>
crab_bake()
```

## Simulated Data 

cRab includes functionality for running simulation experiments to evaluate how clustering stability behaves under different data conditions. The `repeat_simulations()` function: 

- Generates synthetic datasets with varying variance
- Applies the CRAB algorithm across multiple runs
- Returns performance results for comparison across conditions

```{r}
repeat_simulations(n = 100, num_runs = 5, variance = c(0.1, 0.2),
                   min_k = 2, max_k = 4, num_subsamples = 10,
                   cluster_method = "kmeans")
```

# Output 

```crab_bake()``` returns a data frame containing: 
- Tested values of k
- CRAB stability score for each k
- Runtime per model
- Best clustering solution marked as `"BEST"`

# Thesis

This package is based on the methods developed in my Master’s thesis.

Full thesis: [Thesis](https://digitalcommons.calpoly.edu/theses/3329/)

The thesis provides a detailed explanation of the CRAB algorithm, its mathematical formulation, and simulation studies evaluating clustering stability.

# Authors

The CRAB algorithm was developed jointly by Jasmine Cabrera and [Allen Choi](https://github.com/TheAllenChoi) during the development of this package and associated thesis work.
