# Morphological Featurization of Grayscale Images

This repository provides a Python workflow for extracting comprehensive morphological and texture-based features from grayscale images. It includes both global (image-level) and localized (blob-level) feature extraction, with optional masking of bright regions (e.g., holes, tears). This tool is suitable for use in visual inspection, materials science, quality control, and image-based screening workflows.

## Features

### Global Image-Level Features

These features are computed across the entire image (or masked region), describing overall texture and intensity characteristics:

- **mean_grey_value_image** – Average brightness of the image.
- **contrast** – Standard deviation of pixel intensity; highlights visual variation.
- **energy** – Sum of squared histogram probabilities; measures uniformity.
- **homogeneity** – Similarity between pixel intensities.
- **entropy** – Image randomness or complexity.
- **variance** – Dispersion of pixel intensities.
- **skewness** – Asymmetry in intensity distribution (dark vs. light bias).
- **kurtosis** – Tailedness of intensity distribution (e.g., sharp transitions).
- **sum_of_squares** – Histogram variance; a measure of brightness variability.
- **sum_average** – Weighted average of histogram intensity values.
- **sum_entropy** – Entropy from summed intensity distribution.
- **difference_entropy** – Entropy from differences in adjacent intensities.
- **autocorrelation** – Detects regular patterns in intensity (e.g., repeating textures).

### Blob-Level Features

Features derived from distinct blobs (regions of interest) detected in the image:

- **num_blobs** – Total number of detected features.
- **Blob Diameter Statistics** – avg, std, min, max, median blob diameter.
- **Blob Area Statistics** – avg, std, min, max, median blob area.
- **Blob Mean Grey Value Statistics** – avg, std, min, max, median intensity within blobs.

## Workflow Options

Two versions of the analysis are included:

1. **Unmasked Featurization** – Computes features over the entire image, useful when all regions are of interest.
2. **Masked Featurization (Holes Excluded)** – Applies a brightness threshold to exclude holes/tears, computing features only within valid material regions.

Each version saves a CSV file summarizing the features for each image in the dataset.

## Requirements

- Python 3.x
- Required libraries:
  - `opencv-python`
  - `numpy`
  - `pandas`
  - `scipy`
  - `matplotlib`
  - `scikit-learn`
  - `tensorflow` (included for potential downstream use, not required for core featurization)

Install dependencies using pip:

```bash
pip install opencv-python numpy pandas scipy matplotlib scikit-learn tensorflow
```

## Usage

1. Place your images in a directory (supported formats: `.jpg`, `.jpeg`, `.png`, `.bmp`, `.tiff`).
2. Update the `image_dir` variable in the script to point to your image folder.
3. Run the desired featurization script:
   - `full_image_featurization.py`
   - `masked_image_featurization.py`
4. Output CSVs (`image_analysis_results.csv` or `image_analysis_results_holes_excluded.csv`) will be saved in the image directory.

## Output Example

| image file | mean_grey_value_image | contrast | entropy | num_blobs | avg_diameter | avg_area | avg_mean_grey_blob |
|------------|------------------------|----------|---------|-----------|---------------|----------|---------------------|
| img1.png   | 127.3                  | 42.1     | 6.82    | 12        | 115.4         | 9990.2   | 134.5               |
| img2.png   | 119.6                  | 30.5     | 5.71    | 3         | 240.0         | 17850.7  | 121.1               |

## Notes

- The blob detector is tuned to ignore small noise and identify biologically or materially relevant features.
- Bright areas (MGV > 245) are considered holes or tears in the masked variant.
- This workflow is designed to be modular and extensible for feature engineering and unsupervised anomaly detection.

---

For any questions, feel free to open an issue or contribute.
