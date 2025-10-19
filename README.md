# EEG-Signal-Analysis-KarolinaStocka
EEG-Signal-Analysis-KarolinaStocka is a Python toolkit for analyzing horse EEG signals. It provides signal preprocessing, artifact removal, spectral feature extraction (PSD, SEF95, band power), and statistical comparisons between anesthetic agents. Developed from a Master's thesis, it supports reproducible workflows for veterinary research.

Overview
EquineNeuroAnalytics is a comprehensive Python toolkit designed for analyzing electroencephalographic (EEG) signals in horses, with focus on:

Sleep depth assessment under various pharmacological protocols
Signal preprocessing and artifact removal
Spectral feature extraction (PSD, SEF95, band power)
Statistical comparison between anesthetic agents
Automated analysis pipelines for veterinary research

This project emerged from Master's thesis research on "The Impact of Filtering on EEG Signal Features in Horses: The Role of Post-Processing in Sleep Depth Assessment" at the intersection of biomedical engineering, neuroscience, and veterinary medicine.

Signal Processing

EDF file support - Direct reading of European Data Format files
Digital filtering - Butterworth bandpass (0.5-30 Hz) with configurable parameters
Multi-channel processing - Simultaneous analysis of Fp1, Fp2, F7, F8 channels
Artifact handling - Statistical outlier detection and removal

Feature Extraction

Spectral features: Power Spectral Density (Welch method), band power (delta, theta, alpha, beta)
Temporal features: RMS (Root Mean Square), signal amplitude
Advanced metrics: SEF95 (Spectral Edge Frequency 95%)

Analysis Capabilities

Comparative pharmacology - Analyze effects of different anesthetic agents
Temporal segmentation - Automated wake/sleep state classification
Statistical testing - ANOVA, t-tests with effect size calculations
Rich visualizations - Publication-ready plots with seaborn/matplotlib


Installation
Prerequisites

Python 3.12 
pip package manager

Quick Install
bash# 
Clone the repository
git clone https://github.com/KarolinaStocka/EEG-Signal-Analysis-KarolinaStocka.git
cd EEG-Signal-Analysis-KarolinaStocka
## Dependencies / Libraries

This project uses the following Python libraries:

- **MNE-Python (v1.9.0)** – EEG/MEG data loading, preprocessing, analysis, and visualization. [Link](https://mne.tools/stable/index.html)
- **NumPy (v1.26.4)** – Numerical computations and multi-dimensional arrays. [Link](https://numpy.org/)
- **SciPy (v1.13.1)** – Signal processing, statistics, and Fourier transforms. [Link](https://scipy.org/)
- **Pandas (v2.2.2)** – Data manipulation and management of EEG/anesthesiology results. [Link](https://pandas.pydata.org/)
- **Matplotlib (v3.8.4)** – Static, interactive, and animated visualizations. [Link](https://matplotlib.org/)
- **Seaborn (v0.13.2)** – Statistical visualization based on Matplotlib. [Link](https://seaborn.pydata.org/)

# Install package in development mode
pip install -e .


## Basic Usage

```python
from equine_neuro.preprocessing import BandpassFilter
from equine_neuro.features import SpectralAnalyzer
from equine_neuro.io import EDFReader

# Load EEG data
reader = EDFReader("path/to/eeg_file.edf")
data, metadata = reader.load()

# Apply bandpass filtering
filter = BandpassFilter(lowcut=0.5, highcut=30.0, order=4)
filtered_data = filter.apply(data, fs=metadata['sfreq'])

# Extract spectral features
analyzer = SpectralAnalyzer()
features = analyzer.compute_band_power(
    filtered_data, 
    fs=metadata['sfreq'],
    bands={'delta': (0.5, 4), 'theta': (4, 8), 'alpha': (8, 12)}
)

print(features)


## Full Analysis Pipeline

```python
from equine_neuro.pipeline import EEGPipeline

# Initialize pipeline
pipeline = EEGPipeline(
    filter_params={'lowcut': 0.5, 'highcut': 30.0},
    bands={'delta': (0.5, 4), 'theta': (4, 8), 'alpha': (8, 12)}
)

# Run analysis
results = pipeline.run(
    edf_file="path/to/file.edf",
    drug_name="Ketamine_Midazolam",
    wake_window=(-15, 0),   # 15 minutes before drug administration
    sleep_window=(0, 15)    # 15 minutes after drug administration
)

# Save results to CSV
results.to_csv("analysis_results.csv")


## Example Results

Below are sample results from EEG spectral analysis for different drugs. For each frequency band (delta, theta, alpha), mean power during wakefulness and sleep is shown, along with the p-value and statistical significance.

### Drug: ACP
- **Delta:** Wake=600.10, Sleep=296.32, p=0.0089 → significant
- **Theta:** Wake=48.63, Sleep=28.39, p=0.0323 → significant
- **Alpha:** Wake=11.67, Sleep=7.80, p=0.0453 → significant

### Drug: Xylazine_Butopranol
- **Delta:** Wake=100.73, Sleep=74.44, p=0.0000 → significant
- **Theta:** Wake=25.46, Sleep=20.54, p=0.0000 → significant
- **Alpha:** Wake=8.26, Sleep=6.85, p=0.0001 → significant

### Drug: Ketamine_Midazolam
- **Delta:** Wake=104.83, Sleep=85.82, p=0.0000 → significant
- **Theta:** Wake=24.64, Sleep=20.71, p=0.0000 → significant
- **Alpha:** Wake=7.67, Sleep=6.81, p=0.0098 → significant

### Drug: Mepivacaine
- **Delta:** Wake=96.24, Sleep=208.49, p=0.0634 → not significant
- **Theta:** Wake=20.80, Sleep=19.79, p=0.6550 → not significant
- **Alpha:** Wake=6.95, Sleep=6.70, p=0.7045 → not significant

### Drug: Morphine_Xylazine
- **Delta:** Wake=110.64, Sleep=107.08, p=0.6601 → not significant
- **Theta:** Wake=21.58, Sleep=21.09, p=0.5193 → not significant
- **Alpha:** Wake=5.66, Sleep=5.33, p=0.1191 → not significant

### Drug: Midazolam_Ketamine
- **Delta:** Wake=53.47, Sleep=50.24, p=0.4288 → not significant
- **Theta:** Wake=15.77, Sleep=14.27, p=0.1639 → not significant
- **Alpha:** Wake=4.87, Sleep=4.63, p=0.5194 → not significant


Author
Karolina Stocka

🎓 MSc Biomedical Engineering (Medical Informatics)
🎓 BSc Biotechnology
💼 LinkedIn:
📧 email: karolina.stocka7@gmail.com


Acknowledgments

Data collected at University Veterinary Hospital, Vienna
Research approved by Bioethics Committee
Built with Python scientific computing stack (NumPy, SciPy, Pandas)


