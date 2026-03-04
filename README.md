# Digital Image Processing

A collection of three Python-based assignments developed as part of a **Digital Image Processing** course. Each assignment explores a different set of core techniques in image analysis, from histogram manipulation to spectral clustering.

---

## Assignment 1: Histogram Equalization & Matching

This assignment focuses on the design and implementation of algorithms for histogram balancing and matching, with the goal of improving image contrast and transferring tonal distributions between images.

Three distinct approaches are implemented and compared:

- **Greedy Approach** — Iteratively maps intensity levels by greedily minimizing the cumulative distribution function (CDF) difference between source and target histograms.
- **Non-Greedy Approach** — Constructs a global optimal mapping by considering the full histogram distributions simultaneously, avoiding locally suboptimal decisions made by the greedy strategy.
- **Post-Disturbance Approach** — Applies a corrective step after an initial mapping to resolve ambiguities and refine the intensity transfer.

The assignment includes:
- Core function implementations for each of the three approaches.
- Presentation of visual results for each method.
- Comparative analysis and commentary on the trade-offs in quality, computational cost, and accuracy.

---

## Assignment 2: Edge Detection & Circle Detection (Hough Transform)

This assignment covers two stages: detecting edges in grayscale images using classical gradient-based methods, and then applying the Hough Transform to extract circular features from the resulting edge maps.

> **Note:** Images were downscaled to $(267 \times 360)$ pixels prior to processing to improve performance and reduce memory consumption.

### Edge Detection

Two methods are implemented and compared:

- **Sobel Operator** — A first-derivative based approach that computes image gradients in the horizontal and vertical directions, highlighting regions of rapid intensity change.
- **Laplacian of Gaussian (LoG)** — A second-derivative based approach where Gaussian smoothing is applied first to suppress noise, followed by the Laplacian operator. Edges are identified at the **zero-crossings** of the resulting response.

### Circle Detection via Hough Transform

The Hough Transform is applied to the binary edge maps produced by each detection method to locate circles defined by their centre coordinates and radius.

Key aspects of the analysis:
- Parametric voting in the $(x_0, y_0, r)$ accumulator space.
- Evaluation of detection **stability and accuracy** as a function of the voting threshold $V_{min}$, which controls the minimum number of votes required for a candidate circle to be accepted.

---

## Assignment 3: Spectral Clustering & Image Segmentation

This assignment implements graph-based image segmentation using **Spectral Clustering**, building on the affinity matrix $A$ and the graph Laplacian $L = D - A$, where $D$ is the diagonal degree matrix.

### Core Implementations

- **`spectral_clustering`** — Computes the $k$ smallest non-trivial eigenvectors of $L$ (excluding the eigenvector corresponding to the zero eigenvalue), applies row-wise $\ell_2$ normalization to the resulting embedding, and clusters the rows using k-means to produce the final segmentation.

- **`n_cuts`** — Implements the **Normalized Cuts** algorithm based on the relaxation of the combinatorial Ncut problem. Solves the generalised eigenvalue problem $L\mathbf{y} = \lambda D\mathbf{y}$ and uses the second smallest eigenvector to bipartition the graph.

- **`calculate_n_cut_value`** — Evaluates the quality of a binary partition by computing the **Ncut metric**:
$$\text{Ncut}(A, B) = \frac{\text{cut}(A, B)}{\text{assoc}(A, V)} + \frac{\text{cut}(A, B)}{\text{assoc}(B, V)}$$
  Lower values indicate a higher-quality, more balanced cut.

- **`n_cuts_recursive`** — A recursive extension of `n_cuts` that dynamically splits clusters based on two stopping thresholds:
  - $T_1$: minimum cluster size — prevents over-segmentation of very small regions.
  - $T_2$: maximum Ncut value — halts splitting when the resulting partition is of insufficient quality.

### Experimentation

The methods are tested and visualised for varying numbers of clusters $k \in \{2, 3, 4\}$, allowing a qualitative and quantitative comparison of segmentation granularity and coherence.