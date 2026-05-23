# Morphological Featurization of Grayscale Images

## Summary

This repository provides a Python workflow for extracting morphological, intensity, and texture-based features from grayscale images. It is designed for image-based inspection workflows where visual structure needs to be converted into tabular data for downstream analysis, comparison, clustering, anomaly detection, quality control, or machine learning.

The workflow includes both **global image-level features** and **localized blob-level features**. Global features describe the overall intensity and texture structure of each image. Blob-level features describe discrete detected regions of interest, including their size, area, and intensity distributions.

An optional masked variant excludes bright regions such as holes, tears, glare, or missing material, allowing features to be calculated only over valid material regions.

This workflow is intended for applied image analysis in contexts such as materials science, biomaterials, visual inspection, morphology screening, process development, and quality-control analytics.

---

## Novelty and Scope

This repository presents an applied image-featurization workflow for grayscale image datasets. It does **not** claim to introduce a new computer-vision algorithm, texture descriptor, or segmentation method.

Related concepts already exist across:

- classical image processing
- texture analysis
- histogram-based feature extraction
- blob detection
- connected-component analysis
- Haralick-style texture features
- materials image analysis
- computer-vision feature engineering
- unsupervised anomaly detection workflows

The contribution of this repository is an **applied, reproducible workflow** that combines global image texture descriptors, intensity-distribution statistics, blob-level morphology, and optional defect masking into a simple tabular featurization pipeline.

The practical goal is to help convert image sets into structured datasets that can support questions such as:

- How do images differ in overall intensity, contrast, or texture?
- Are there distinct morphological regions or blobs within each image?
- Do blob size, area, or intensity statistics vary across samples?
- Can image-derived features support clustering, screening, anomaly detection, or quality control?
- Can image-based morphology be summarized in a form suitable for statistical learning?

This repository should be interpreted as:

- a practical image-featurization workflow
- a technical prototype
- a feature-engineering tool for grayscale image datasets
- a foundation for downstream statistical or machine-learning analysis

It should not currently be interpreted as:

- a validated production inspection system
- a replacement for domain-specific segmentation
- a causal or mechanistic morphology model
- a universal image-analysis framework
- a claim of novelty over existing computer-vision or texture-analysis literature

---

## Workflow Overview

The repository includes two main featurization modes.

### 1. Full-Image Featurization

The unmasked workflow computes features across the entire grayscale image. This is useful when all regions of the image are considered valid material or when global image structure is the main object of analysis.

### 2. Masked Featurization

The masked workflow applies a brightness threshold to exclude bright regions, such as holes, tears, glare, or missing material. Features are then computed only over the remaining valid material region.

In the current workflow, bright areas with mean gray value greater than 245 are treated as excluded regions in the masked variant. This threshold can be adjusted depending on image conditions and material type.

Each workflow saves a CSV file summarizing extracted features for every image in the dataset.

---

## Feature Types

The workflow extracts two broad classes of features:

1. global image-level features
2. blob-level features

Together, these allow each image to be represented as a row of tabular data suitable for downstream analysis.

---

## Global Image-Level Features

Global features are computed across the entire image or masked valid region. These features describe overall intensity distribution, texture, and image complexity.

| Feature | Description |
|---|---|
| `mean_grey_value_image` | Average grayscale intensity of the image or valid region |
| `contrast` | Standard deviation of pixel intensity; captures visual variation |
| `energy` | Sum of squared histogram probabilities; measures uniformity |
| `homogeneity` | Similarity among pixel intensities |
| `entropy` | Image randomness or complexity |
| `variance` | Dispersion of pixel intensities |
| `skewness` | Asymmetry of the intensity distribution |
| `kurtosis` | Tailedness of the intensity distribution |
| `sum_of_squares` | Histogram variance; another measure of brightness variability |
| `sum_average` | Weighted average of histogram intensity values |
| `sum_entropy` | Entropy from summed intensity distribution |
| `difference_entropy` | Entropy from differences in adjacent intensities |
| `autocorrelation` | Regularity or repeating structure in intensity patterns |

These features are useful for distinguishing images that differ in brightness, contrast, texture complexity, uniformity, or spatial regularity.

---

## Blob-Level Features

Blob-level features are derived from distinct detected regions of interest. These may correspond to visible morphological structures, particles, domains, defects, colonies, patches, or other localized image features depending on the application.

| Feature Group | Description |
|---|---|
| `num_blobs` | Total number of detected blobs |
| Blob diameter statistics | Average, standard deviation, minimum, maximum, and median blob diameter |
| Blob area statistics | Average, standard deviation, minimum, maximum, and median blob area |
| Blob mean gray value statistics | Average, standard deviation, minimum, maximum, and median intensity within detected blobs |

Blob-level summaries allow localized morphology to be represented in a compact form without requiring every individual region to be modeled separately.

---

## Practical Use Cases

This workflow is most useful when:

- images are grayscale or can be converted meaningfully to grayscale
- image sets need to be converted into structured tabular features
- morphology, texture, intensity, or visible defects are analytically important
- downstream analysis will use clustering, classification, regression, anomaly detection, or statistical comparison
- visual inspection needs to be made more quantitative and reproducible

Potential applications include:

- materials characterization
- biomaterials analysis
- mycelium morphology screening
- process development
- surface inspection
- defect detection
- image-based quality control
- microscopy-derived morphology screening
- texture comparison across experimental treatments
- preprocessing for unsupervised anomaly detection

---

## Usage

### 1. Prepare Image Directory

Place images in a directory using one of the supported formats:

```text
.jpg
.jpeg
.png
.bmp
.tiff
```

### 2. Update Image Path

Update the `image_dir` variable in the script to point to the image folder.

### 3. Run the Desired Script

Use the full-image workflow when all regions should be analyzed:

```text
full_image_featurization.py
```

Use the masked workflow when bright holes, tears, glare, or invalid regions should be excluded:

```text
masked_image_featurization.py
```

### 4. Review Output CSV

The workflow generates CSV summaries such as:

```text
image_analysis_results.csv
image_analysis_results_holes_excluded.csv
```

These files can be used for visualization, statistical analysis, clustering, anomaly detection, or machine-learning workflows.

---

## Output Example

| image file | mean_grey_value_image | contrast | entropy | num_blobs | avg_diameter | avg_area | avg_mean_grey_blob |
|---|---:|---:|---:|---:|---:|---:|---:|
| `img1.png` | 127.3 | 42.1 | 6.82 | 12 | 115.4 | 9990.2 | 134.5 |
| `img2.png` | 119.6 | 30.5 | 5.71 | 3 | 240.0 | 17850.7 | 121.1 |

---

## Requirements

The workflow uses Python 3.x.

Required libraries:

- `opencv-python`
- `numpy`
- `pandas`
- `scipy`
- `matplotlib`
- `scikit-learn`

Optional downstream library:

- `tensorflow`

TensorFlow is included for potential downstream use but is not required for the core featurization workflow.

Install dependencies using pip:

```bash
pip install opencv-python numpy pandas scipy matplotlib scikit-learn tensorflow
```

If TensorFlow is not needed, the core dependencies can be installed with:

```bash
pip install opencv-python numpy pandas scipy matplotlib scikit-learn
```

---

## Current Limitations

Several limitations should be considered.

1. **The workflow is based on classical image features.**  
   It does not use deep learning or learned feature representations in its current form.

2. **Blob detection is threshold- and parameter-dependent.**  
   The detected blobs depend on image quality, scale, preprocessing, contrast, and detector settings.

3. **The masked workflow assumes bright invalid regions.**  
   The current masking logic treats very bright regions as holes, tears, glare, or missing material. Other applications may require different masking rules.

4. **Feature values are sensitive to image acquisition conditions.**  
   Lighting, exposure, magnification, resolution, compression, and background can strongly influence extracted features.

5. **The workflow summarizes morphology but does not explain it mechanistically.**  
   Image-derived features may be useful for screening or comparison, but domain knowledge is required to interpret their biological, material, or process meaning.

6. **Segmentation is intentionally simple.**  
   For complex images, domain-specific segmentation or preprocessing may be required before this workflow is appropriate.

---

## Future Development

Possible next steps include:

- adding configurable preprocessing options
- exposing blob-detection parameters in a configuration file
- adding batch visualization of masks and detected blobs
- adding support for multiple channels or color-derived features
- adding scale calibration for converting pixels to physical units
- adding grouped analysis by experimental condition
- adding clustering and anomaly-detection examples
- adding feature correlation and redundancy diagnostics
- packaging the workflow into reusable functions or modules
- adding example datasets with synthetic or public images

---

## Bottom Line

Morphological Featurization of Grayscale Images provides a practical way to convert grayscale image datasets into structured, interpretable feature tables.

The workflow is intended for applied researchers, scientists, and engineers who need to quantify visual morphology, texture, and localized image structure in a reproducible form. The emphasis is not on new computer-vision theory, but on making image-derived morphology accessible for statistical learning, screening, quality control, and experimental interpretation.

---

## License

This project is released under the **Apache License 2.0**.

The intent is to support open exploration, reproducibility, and extension of image-featurization workflows while preserving a permissive license structure suitable for research, education, and applied development.
