# Schneider Prize — Advanced FTIR Analysis Platform

**MRG Labs Hackathon 2025 Project**

A professional-grade, AI-powered desktop application for analyzing FTIR (Fourier Transform Infrared Spectroscopy) data of grease samples. This comprehensive platform combines traditional spectroscopy with cutting-edge machine learning to provide laboratory-quality analysis, automated reporting, and predictive maintenance insights.

![Version](https://img.shields.io/badge/version-3.0--AI-blue)
![Platform](https://img.shields.io/badge/platform-Windows-blue)
![AI](https://img.shields.io/badge/AI-LLM%20Enabled-purple)
![ML](https://img.shields.io/badge/ML-Machine%20Learning-orange)
![License](https://img.shields.io/badge/license-MIT-orange)

---

## 📥 Download & Get Started

**Ready to use - No installation required!**

### Installation Steps:

1. **Download** the entire [dist folder](dist/) from this repository
   - ⚠️ **Important**: Download BOTH the `.exe` file AND the `_internal` folder
   - Keep them in the same directory together
   
2. **Extract** to any location on your computer (e.g., Desktop, Documents, C:\FTIR_Analyzer)

3. **Double-click** the `.exe` file to launch the application

4. **Start analyzing** your FTIR data immediately!

### ⚠️ Important Notes:
- The `.exe` file and `_internal` folder **must be in the same directory**
- Don't move or delete the `_internal` folder
- Don't separate the `.exe` from its `_internal` folder
- You can create a shortcut to the `.exe` file for convenience

**System Requirements:**
- Windows 10 or newer
- No Python or other software needed
- Works on any modern Windows PC
- ~100 MB disk space for the application

---

## 🌟 What's New in Version 3.0

**Advanced AI & Machine Learning Capabilities:**
- 🧠 **AI Report Generator** - Cloud LLM + local fallback for narrative reports
- 📊 **Batch Analysis** - One-click CSV export of metrics for all samples
- 🤖 **Supervised Learning** - Train regressors to predict TAN, hours-in-service, etc.
- 🔬 **Peak Deconvolution** - Gaussian fitting for overlapping peaks
- 📈 **Signal Preprocessing** - Professional smoothing and baseline correction
- 🔄 **DTW Alignment** - Dynamic Time Warping for spectral drift correction
- 💡 **Template Alerts** - Rule-based AI for instant chemistry insights
- 🚦 **QC Thresholds** - Configurable pass/fail criteria with color-coded status
- 📝 **Provenance Tracking** - Complete audit trails for regulatory compliance
- 🖱️ **Interactive Tooltips** - Hover over plots for instant data readout

---

## 🚀 Advanced AI & ML Features (NEW in v3.0)

This section documents the cutting-edge analysis capabilities that transform this tool into a professional-grade laboratory platform.

### 1. 🧠 AI Report Generator

**Cloud LLM + Local Fallback System**

Generate professional FTIR analysis reports automatically using AI.

#### Features
- **Dual-mode operation**: 
  - Cloud: Uses OpenAI API (gpt-4o-mini) for sophisticated narrative reports
  - Local: Rule-based markdown generator (works offline, no API key needed)
- **Privacy-first**: Only sends metrics and peaks to cloud (NOT raw spectra) by default
- **Configurable**: Choose what to include in reports
- **Markdown output**: Saves `.md` files for easy viewing/sharing
- **PDF integration**: Optionally embeds reports into PDF exports

#### Setup for Cloud LLM (Optional)

Set environment variables before running:

```bash
# Windows PowerShell
$env:OPENAI_API_KEY="sk-your-key-here"
$env:OPENAI_MODEL="gpt-4o-mini"
$env:OPENAI_API_BASE="https://api.openai.com/v1"

# Linux/Mac
export OPENAI_API_KEY="sk-your-key-here"
export OPENAI_MODEL="gpt-4o-mini"
export OPENAI_API_BASE="https://api.openai.com/v1"
```

Without these variables, the tool automatically uses the local fallback (still very useful!).

#### Usage
1. Load baseline and sample
2. Click **"Generate AI Report"** button
3. Configure options:
   - ✅ **Privacy mode** (default: ON) - Sends only metrics, not raw data
   - ✅ **Include recommendations** - Adds maintenance suggestions
   - ⬜ **Append to PDF** - Adds narrative to PDF reports
4. Click **"Generate"**
5. Report saves as markdown file in output directory

#### Report Contents
- **Summary**: RMSE, Pearson r, ΔA, oxidation index, closest match, anomaly score
- **Notable Peaks**: Top 6 peaks with wavenumbers and functional group labels
- **Interpretation**: Chemistry-grounded analysis of findings
- **Recommendations**: Actionable maintenance suggestions (optional)

**Example Report Output**:
```markdown
# FTIR Report — 103925.csv vs 19231.csv

**Summary**
- **RMSE:** 0.0234
- **Pearson r:** 0.9823
- **ΔA @ 1715 cm⁻¹:** 0.0145
- **Oxidation Index:** 0.234

**Notable Peaks**
- 2920 cm⁻¹ (A=0.456) — C-H Stretch (Aliphatic)
- 2851 cm⁻¹ (A=0.423) — C-H Stretch (Aliphatic)
- 1720 cm⁻¹ (A=0.089) — C=O Stretch (Carbonyl)

**Interpretation**
- Sample closely matches baseline overall. Oxidation is moderate.

**Recommendations**
- Track carbonyl band over time; oxidation mitigation may be warranted.
```

---

### 2. 📊 Batch Analysis → CSV

**One-Click Multi-Sample Metrics Export**

Analyze all loaded samples simultaneously and export comprehensive metrics to CSV.

#### Features
- Processes all samples against baseline in one operation
- Exports: RMSE, Pearson r, ΔA, Oxidation Index, Water Index
- Timestamp-stamped filenames for version control
- Perfect for trend analysis and quality control

#### Usage
1. Load baseline
2. Load multiple samples (10, 50, 100+)
3. Click **"Batch Analyze → CSV"**
4. CSV saves automatically to output directory

#### Output Format
```csv
sample,rmse,pearson_r,delta_at_1715,oxidation_index,water_index
103925.csv,0.0234,0.9823,0.0145,0.234,0.089
103926.csv,0.0456,0.9654,0.0234,0.312,0.102
103927.csv,0.0789,0.9234,0.0456,0.445,0.156
```

#### Use Cases
- **Trend Analysis**: Import to Excel, plot oxidation over time
- **QC Dashboard**: Identify outliers visually
- **Predictive Maintenance**: Correlate metrics with failure events
- **Research**: Statistical analysis of large sample sets

---

### 3. 🤖 Supervised Machine Learning - Calibration Regressor

**Train ML Models to Predict Real-World Values**

Train Ridge regression models on your calibration data to predict properties like Total Acid Number (TAN), hours-in-service, viscosity, etc.

#### How It Works
1. Create calibration CSV with known values
2. App vectorizes each spectrum into 1030-dimensional feature space
3. Ridge regression learns the mapping from spectral features to target values
4. Predict target for new unknown samples

#### Calibration CSV Format
```csv
sample,target
Mobilgrease_28.csv,150.5
Aeroshell_7_25492.csv,243.2
Mobil_SHC_460_91684.csv,89.7
```

**Columns**:
- `sample`: Filename (must match loaded samples exactly)
- `target`: Numeric value to predict (hours, TAN, viscosity, etc.)

#### Usage
1. Load baseline and sample spectra
2. Create calibration CSV with at least 5 known samples
3. Click **"Load Calibration"**
4. Select your calibration CSV
5. Model trains automatically
6. Select any sample and click **"Predict Target"**
7. Get instant prediction based on spectral fingerprint

#### Example Applications

**Predict Hours-in-Service**:
```csv
sample,target
bearing_A_new.csv,0
bearing_A_100h.csv,100
bearing_A_500h.csv,500
bearing_A_1000h.csv,1000
bearing_A_2000h.csv,2000
```
Train model → Predict hours for unknown bearing grease

**Predict Total Acid Number (TAN)**:
```csv
sample,target
fresh_oil.csv,0.12
used_6mo.csv,0.89
used_12mo.csv,1.45
used_18mo.csv,2.34
```
Train model → Predict TAN without wet chemistry

#### Technical Details
- **Algorithm**: Ridge Regression (L2 regularization, α=1.0)
- **Feature Space**: 1030 dimensions
  - 1024 resampled spectrum points (600-3700 cm⁻¹)
  - 6 domain-specific features (oxidation index, band areas, peak stats)
- **Minimum Samples**: 5 (more is better, 20+ recommended)
- **Preprocessing**: Automatic - applies current smoothing/baseline settings

---

### 4. 📈 Signal Preprocessing

**Professional-Grade Signal Processing**

Clean and enhance spectra before analysis using laboratory-standard techniques.

#### Smoothing Options

**Savitzky-Golay Filter** (when scipy installed):
- **Window size**: Odd integer (default: 11 points)
- **Polynomial order**: 2-5 (default: 2)
- **Benefits**: Preserves peak shape while reducing noise
- **Best for**: High-resolution data with minor noise

**Moving Average** (fallback):
- **Window size**: Odd integer (default: 11 points)
- **Benefits**: Simple, fast, effective
- **Best for**: Quick noise reduction

#### Baseline Correction Options

**Linear Detrend** (default):
- Removes linear baseline drift
- Fast computation
- Good for minor baseline issues

**Rolling Minimum**:
- **Window size**: Odd integer (default: 101 points)
- Subtracts rolling minimum from signal
- Excellent for complex baseline curvature
- Handles sloping backgrounds

#### UI Controls
Located in **"Pre-process"** panel:
- ☑ **Smooth**: Enable/disable smoothing
- **win**: Window size for smoothing
- **poly**: Polynomial order (Savitzky-Golay only)
- ☑ **Baseline**: Enable/disable baseline correction
- **Dropdown**: Select "linear" or "rolling" mode
- **roll**: Window size for rolling minimum

#### Real-time Preview
- Changes apply instantly to the plot
- See effects before saving
- All exports use preprocessed data

---

### 5. 🔬 Advanced Band Metrics

**User-Editable Integration Windows**

Customize the wavenumber ranges used for quantitative analysis.

#### Configurable Bands

Located in **"Band windows (cm⁻¹)"** panel:
- **C=O**: Carbonyl region (default: 1710-1750)
- **C-H**: Aliphatic hydrocarbon (default: 2850-2960)
- **O-H**: Hydroxyl/water (default: 3200-3600)

#### Calculated Indices

**Oxidation Index**:
```
OxIndex = Area(C=O) / Area(C-H)
```
- Normalized measure of oxidation
- Independent of sample thickness
- Typical values: 0.05 (fresh) to 0.80 (heavily oxidized)

**Water Index** (NEW):
```
WaterIndex = Area(O-H) / Area(C-H)
```
- Quantifies moisture contamination
- Normalized against base oil
- Typical values: 0.01 (dry) to 0.30 (wet)

#### Use Cases
- **Custom formulations**: Adjust C=O range for specific esters
- **Synthetic greases**: Modify C-H range for PAO/PAG oils
- **Research**: Fine-tune integration windows for publications

---

### 6. 🔄 DTW Alignment (Dynamic Time Warping)

**Correct for Spectral Drift and X-Axis Misalignment**

Automatically aligns sample spectra to baseline even when wavenumbers don't match perfectly.

#### What It Solves
- **Instrument drift**: Different FTIR units have slight calibration differences
- **Temperature effects**: Spectral shifts from sample temperature
- **Mechanical shifts**: Small peak position changes from pressure/stress

#### How It Works
1. Downsamples both spectra to 512 points (for speed)
2. Computes optimal warping path using fastdtw algorithm
3. Maps sample intensities onto baseline x-grid
4. Returns aligned spectrum for fair comparison

#### Toggle Control
Located in **"Advanced"** panel:
- ☑ **DTW align**: Enable Dynamic Time Warping
- When enabled: Sample spectrum automatically warps to match baseline
- When disabled: Standard interpolation used

#### When to Use
- ✅ **USE DTW** when:
  - Comparing data from different instruments
  - Known x-axis calibration differences
  - Peaks are shifted but shapes match
  
- ⬜ **DON'T USE DTW** when:
  - Data from same instrument/session
  - Actual chemical differences (not just drift)
  - Comparing fundamentally different materials

#### Performance
- Aligns 4000-point spectra in <0.5 seconds
- Automatic fallback to shift-based alignment if fastdtw not installed

---

### 7. 💡 Template Alerts (Rule-Based AI)

**Instant Chemistry Insights from Spectral Patterns**

Automated detection of common degradation and contamination signatures.

#### Detected Conditions

**1. Oxidation Progression**
- Monitors ΔOxidation Index between baseline and sample
- **WARN**: ΔOx > 0.10 (moderate increase)
- **FAIL**: ΔOx > 0.20 (significant increase)

**2. Water/Moisture Contamination**
- Tracks O-H to C-H ratio changes
- **WARN**: Water index increase > 0.15

**3. Nitration/Contamination**
- Detects twin N-O bands at ~1515 and ~1345 cm⁻¹
- **WARN**: Both bands grow simultaneously
- Indicates: NOx exposure, combustion byproducts, contamination

**4. Ester Hydrolysis**
- Monitors C-O/C-O-C region (1000-1300 cm⁻¹) + carbonyl growth
- **INFO**: Suggests ester breakdown
- Common in: Synthetic esters, hydraulic fluids

**5. Hydrocarbon Depletion with Oxidation**
- Tracks C-H decrease + C=O increase
- **INFO**: Classic aging/oxidation pattern
- Indicates: Base oil consumption during degradation

#### Display
- Alerts automatically append to status bar
- Most severe alerts shown first (FAIL > WARN > INFO)
- Up to 3 alerts displayed
- Toggle on/off in **"Advanced"** panel: ☑ **Template alerts**

#### Example Status Messages
```
Preview updated.  •  WARN: Oxidation increased (ΔOx=0.14). | INFO: Hydrocarbon (C–H) decreased while carbonyl increased → aging/oxidation trend.
```

---

### 8. 🔬 Peak Deconvolution (Gaussian Fitting)

**Resolve Overlapping Peaks into Individual Components**

Fit multiple Gaussian functions to complex peak regions to extract individual component contributions.

#### Scientific Background
- Overlapping peaks are common in FTIR (e.g., multiple C=O species)
- Deconvolution separates contributions
- Quantifies each component individually
- Publication-quality analysis

#### Features
- **Multi-component fitting**: 1-10 Gaussian peaks
- **Automatic initial guess**: Uses detected peaks
- **Visual overlay**: Shows data, components, and sum
- **CSV export**: Component parameters and areas
- **Preprocessing integration**: Works with smoothed/corrected data
- **DTW-compatible**: Can deconvolve aligned spectra

#### Usage
1. Load and select a sample
2. Set **"Fit region"** (e.g., "1600-1800" for carbonyl region)
3. Set **"# Peaks"** (e.g., 2 for overlapping C=O species)
4. Click **"Deconvolve"**
5. View fitted components in new window
6. Optional: CSV auto-exports component details

#### Output

**Visual Display**:
- Original data (blue line)
- Individual Gaussian components (colored lines)
- Sum of components (thick line)
- Legend with labels

**CSV Export** (if enabled):
```csv
component,A,center_cm-1,sigma,area
1,0.234,1720.5,12.34,147.2
2,0.156,1735.2,15.67,124.8
```

**Columns**:
- `A`: Peak amplitude (height)
- `center_cm-1`: Peak position (wavenumber)
- `sigma`: Peak width parameter
- `area`: Integrated peak area (A × σ × √(2π))

#### Example Application
```
Carbonyl Region (1600-1800 cm⁻¹) in Used Grease:
- Component 1: 1715 cm⁻¹ (ketones from oxidation)
- Component 2: 1735 cm⁻¹ (esters from additives)

Area ratio = A1/A2 = degradation severity index
```

---

### 9. 📏 Advanced Band Integration

**Customizable Chemical Regions**

Define your own integration windows for domain-specific analysis.

#### Editable Bands
Located in **"Band windows (cm⁻¹)"** panel:
- **C=O**: Default "1710-1750"
- **C-H**: Default "2850-2960"
- **O-H**: Default "3200-3600"

#### Why Customize?

**Different Base Oils**:
- PAO oils: Adjust C-H to "2840-2970"
- Ester-based: Expand C=O to "1690-1750"
- Silicone greases: Add Si-O region "1000-1100"

**Different Additives**:
- Phenolic antioxidants: "3550-3700" for O-H
- Nitration monitoring: "1500-1560" for N-O

**Research Applications**:
- Match literature band definitions exactly
- Compare with published oxidation indices
- Replicate ASTM method specifications

#### Syntax
- Use hyphen or en-dash: `1710-1750` or `1710–1750`
- Order doesn't matter: `1750-1710` works too
- Must be valid numbers

---

### 10. 🚦 QC Thresholds & Color-Coded Status

**Automated Pass/Fail Assessment**

Set warning and fail limits for key metrics to enable instant quality decisions.

#### Configurable Thresholds

Click **"QC Thresholds"** button to configure:

| Metric | Default Warn | Default Fail |
|--------|--------------|--------------|
| **RMSE** | 0.03 | 0.06 |
| **Oxidation Index** | 0.35 | 0.60 |
| **Anomaly Score** | 0.55 | 0.75 |

#### Assessment Logic
```python
For each metric:
  if value >= FAIL threshold:
    Status = FAIL
  elif value >= WARN threshold:
    Status = WARN
  else:
    Status = OK

Overall Status = worst individual status
```

#### Status Display
Results automatically update in status bar:
- 🟢 **OK**: All metrics pass
- 🟡 **WARN**: At least one warning (e.g., "RMSE WARN, OX WARN")
- 🔴 **FAIL**: At least one failure (e.g., "OX FAIL")

#### Use Cases

**Receiving Inspection**:
```
Set thresholds:
- RMSE < 0.05 (warn), < 0.08 (fail)
- Oxidation < 0.20 (warn), < 0.35 (fail)

Test incoming batch:
- RMSE: 0.034 → WARN
- Oxidation: 0.18 → OK
Overall: WARN → Request COA from supplier
```

**Predictive Maintenance**:
```
Set aggressive thresholds:
- Oxidation < 0.40 (warn), < 0.60 (fail)

Monthly monitoring:
- Month 1: Oxidation 0.15 → OK
- Month 6: Oxidation 0.38 → OK
- Month 9: Oxidation 0.43 → WARN → Schedule replacement
```

---

### 11. 📝 Provenance JSON Export

**Complete Audit Trail for Regulatory Compliance**

Export every setting, parameter, and file used in your analysis for full reproducibility.

#### Exported Information
```json
{
  "timestamp": "2025-11-08T19:15:30.123456+00:00",
  "app": "Schneider CSV Plotter",
  "theme": "dark",
  "preprocess": {
    "smooth_enabled": true,
    "smooth_window": 11,
    "smooth_poly": 2,
    "baseline_enabled": true,
    "baseline_mode": "linear"
  },
  "bands": {
    "CO": "1710-1750",
    "CH": "2850-2960",
    "OH": "3200-3600"
  },
  "files": {
    "baseline": "C:/path/to/baseline.csv",
    "samples": ["sample1.csv", "sample2.csv"]
  },
  "metrics_snapshot": { ... },
  "versions": {
    "python": "3.12.0",
    "numpy": "1.26.0",
    "pandas": "2.1.0",
    "matplotlib": "3.8.0",
    "sklearn": "1.3.0"
  }
}
```

#### Use Cases
- **GLP Compliance**: FDA/EPA require method documentation
- **ISO Certification**: Audit trail for quality systems
- **Research**: Exact replication of analysis conditions
- **Troubleshooting**: Debug issues by reviewing settings
- **Archival**: Long-term storage of analysis metadata

#### Usage
1. Complete your analysis
2. Click **"Export Provenance"**
3. JSON file saves with timestamp

---

### 12. 🖱️ Interactive Peak Tooltips

**Real-Time Data Display on Mouse Hover**

Move your mouse over the plot to see exact wavenumbers and absorbances.

#### Features
- **No clicking required**: Automatic on hover
- **Nearest-point detection**: Always shows closest data point
- **Wavenumber precision**: 0.1 cm⁻¹ accuracy
- **Absorbance precision**: 3 significant figures
- **Dual implementation**: Uses mplcursors if available, custom fallback otherwise

#### Display Format
```
x=1720.5 cm⁻¹
y=0.234
```

#### Toggle
Located in **"Advanced"** panel:
- ☑ **Peak tooltips**: Enable interactive hover display

---

### 13. 🔍 DTW Alignment Details

**Technical Deep-Dive**

#### Algorithm
Uses Fast Dynamic Time Warping (fastdtw) with these optimizations:
- Downsamples to 512 points (90% faster)
- Compares intensity patterns (y-values)
- Builds optimal warping path
- Interpolates back to full resolution

#### Fallback Mode
If fastdtw not installed:
- Tries ±40 circular shifts
- Minimizes RMSE at each shift
- Returns best alignment
- Still effective for small drifts

#### Comparison

| Feature | fastdtw | Fallback |
|---------|---------|----------|
| Speed | ★★★★★ | ★★★☆☆ |
| Accuracy | ★★★★★ | ★★★☆☆ |
| Max drift | Unlimited | ±40 points |
| Dependencies | Yes | None |

---

### 14. 💡 Template Alerts - Complete List

**All Detected Conditions**

#### Alert Levels
- **INFO**: Interesting pattern, informational
- **WARN**: Potential issue, investigate
- **FAIL**: Critical issue, action required

#### Complete Alert Catalog

**Oxidation Progression**:
```
ΔOx > 0.10: WARN - "Oxidation increased (ΔOx=0.14)"
ΔOx > 0.20: FAIL - "Oxidation increased significantly (ΔOx=0.23)"
```

**Water Ingress**:
```
ΔWaterIndex > 0.15: WARN - "O–H/Water band growth (Δindex=0.18)"
```

**Nitration/Contamination**:
```
Twin N-O bands grow: WARN - "Twin N–O bands grew (~1515 & ~1345 cm⁻¹) → possible nitration/contamination"
```

**Ester Hydrolysis**:
```
C-O region + C=O increase: INFO - "Increased C–O/C–O–C region with carbonyl growth → possible ester hydrolysis"
```

**Aging Pattern**:
```
C-H decrease + C=O increase: INFO - "Hydrocarbon (C–H) decreased while carbonyl increased → aging/oxidation trend"
```

---

## 🎯 Overview

This application is designed for lubricant analysis professionals to:
- Compare grease samples against baseline references
- Detect oxidation, water contamination, and degradation
- Identify functional groups in FTIR spectra
- Generate professional branded PDF reports
- Store and retrieve data from cloud databases

---

## 🚀 Quick Start Guide

### Getting Started

1. **Load Baseline**: Click "Load Baseline CSV" and select your reference grease spectrum
2. **Load Samples**: Click "Load Sample CSV(s)" and select one or more test samples
3. **Enable FTIR Regions**: Check "FTIR Regions" to see functional group highlighting
4. **Analyze**: Click "FTIR Analysis" to get comprehensive health assessment

---

## 📊 Features

### Core Functionality

#### 1. Baseline vs Sample Comparison
- Load reference baseline FTIR spectrum
- Load multiple sample spectra for comparison
- Animated line-by-line plotting for visual appeal
- Side-by-side overlay visualization

#### 2. Peak Detection & Analysis
- **Automatic Peak Finding**: Detects local maxima in spectra
- **Adjustable Sensitivity**: Control detection threshold (0-1 scale)
- **Minimum Distance**: Set minimum separation between peaks
- **Peak Table Display**: View detected peaks in organized table
- **CSV Export**: Save peak data for external analysis

#### 3. Similarity Metrics
Quantitative comparison between baseline and sample:

- **RMSE (Root Mean Square Error)**
- **Pearson Correlation Coefficient**
- **Delta Absorbance (ΔA)** at specific wavenumbers

#### 4. FTIR-Specific Analysis 🧪

##### Functional Group Identification
Automatically detects and analyzes 5 key functional groups:

| Region | Wavenumber (cm⁻¹) | Chemical Bond | Significance |
|--------|-------------------|---------------|--------------|
| **C-H Aliphatic** | 2850-2960 | C-H Stretch | Base oil hydrocarbon chains (primary component) |
| **C=O Carbonyl** | 1700-1750 | C=O Stretch | Oxidation products, esters (degradation indicator ⚠️) |
| **C=C Unsaturated** | 1600-1680 | C=C Stretch | Unsaturated bonds (oxidation susceptibility) |
| **O-H Hydroxyl** | 3200-3600 | O-H Stretch | Water, alcohols (contamination indicator ⚠️) |
| **C-H Aromatic** | 3000-3100 | C-H Aromatic | Aromatic compounds (additives, base oil type) |

##### Contamination & Degradation Detection
Automated warning system for:

- **Oxidation Detection**: Elevated C=O peaks indicate grease breakdown
  - Medium warning: Intensity > 0.15
  - High warning: Intensity > 0.30
  
- **Water Contamination**: O-H peaks suggest moisture intrusion
  - Medium warning: Intensity > 0.10
  - High warning: Intensity > 0.25
  
- **Base Oil Depletion**: Reduced C-H intensity compared to baseline
  - Warning when C-H drops below 70% of baseline

##### Grease Health Score
- Calculated 0-100 scale
- Starts at 100 (perfect health)
- Deductions based on detected issues:
  - High oxidation: -30 points
  - Medium oxidation: -15 points
  - High water contamination: -25 points
  - Medium water contamination: -10 points
  - Base oil depletion: -20 points

**Score Interpretation**:
- 🟢 **80-100**: Excellent condition
- 🟡 **60-79**: Acceptable, monitor closely  
- 🔴 **<60**: Poor condition, action required

#### 5. Spectral Region Visualization
- Color-coded vertical bands highlight functional group regions
- Optional region labels with chemical names
- Helps quickly identify where differences occur

#### 6. PDF Report Generation
Professional two-page reports including:
- **Page 1**: Full spectral comparison plot with detected peaks
- **Page 2**: Summary with:
  - Baseline and sample identification
  - Similarity metrics (RMSE, Pearson r, ΔA)
  - Complete peak table
  - Generation timestamp

#### 7. Cloud Integration (Optional)
- Firebase storage for baselines, samples, and graphs
- Password-protected uploads
- Search and filter cloud data
- Automatic metadata tracking

#### 8. Theme Support
- Light and dark mode
- Professional MRG Labs branding
- Smooth gradient backgrounds
- Accessible color schemes

---

## 📐 Understanding the Metrics

### FTIR Data Format
- **X-axis**: Wavenumber in cm⁻¹ (inverse centimeters)
  - Represents infrared light wavelength
  - Typical range: 400-4000 cm⁻¹
  
- **Y-axis**: Absorbance (A)
  - Measures how much light is absorbed
  - Higher absorbance = stronger presence of that bond
  - Dimensionless unit

### Similarity Metrics Explained

#### 1. RMSE (Root Mean Square Error)

**What it measures**: Overall difference between two spectra

**Formula**:
```
RMSE = √(Σ(baseline_i - sample_i)² / n)
```

**Interpretation**:
- **Lower is better** (more similar spectra)
- RMSE = 0: Perfect match
- RMSE < 0.05: Excellent match
- RMSE 0.05-0.15: Good match
- RMSE > 0.15: Significant differences

**What it tells you**:
- Quantifies the "distance" between spectra
- Sensitive to both peak shifts and intensity changes
- Useful for quality control and batch consistency

**Example Use Case**:
If baseline RMSE = 0.08 but after 6 months it's 0.22, the grease has degraded significantly.

---

#### 2. Pearson Correlation Coefficient (r)

**What it measures**: Linear relationship between two spectra

**Formula**:
```
r = Cov(baseline, sample) / (σ_baseline × σ_sample)
```

**Range**: -1 to +1

**Interpretation**:
- **r = 1**: Perfect positive correlation (identical pattern)
- **r > 0.95**: Excellent match
- **r = 0.90-0.95**: Good match
- **r = 0.70-0.90**: Moderate similarity
- **r < 0.70**: Poor match, different composition

**What it tells you**:
- Measures spectral pattern similarity (shape)
- Less sensitive to absolute intensity differences
- Excellent for identifying grease type/family

**Example Use Case**:
Two greases with r = 0.98 have nearly identical chemical composition, even if one is diluted (different absolute intensities).

---

#### 3. Delta Absorbance (ΔA) at Specific Wavenumber

**What it measures**: Absorbance difference at a specific point

**Formula**:
```
ΔA = Sample_absorbance(x) - Baseline_absorbance(x)
```

where x is the wavenumber in cm⁻¹

**Interpretation**:
- **ΔA > 0**: Sample has more of that bond/compound
- **ΔA < 0**: Sample has less of that bond/compound
- **ΔA ≈ 0**: Similar amounts

**What it tells you**:
- Precise measurement of specific functional group changes
- Tracks additive depletion or contamination
- Monitors oxidation at C=O peak (~1720 cm⁻¹)

**Common Monitoring Points**:
- **1720 cm⁻¹**: C=O stretch (oxidation)
- **2920 cm⁻¹**: C-H stretch (base oil)
- **3400 cm⁻¹**: O-H stretch (water)

**Example Use Case**:
ΔA at 1720 cm⁻¹ = +0.15 indicates oxidation has created new carbonyl groups (degradation).

---

### Peak Analysis Metrics

#### Peak Intensity
- Maximum absorbance value in a region
- Indicates concentration of functional group
- Used for quantitative analysis

#### Peak Area (Integration)
- Area under the curve for a spectral region
- More robust than peak height alone
- Accounts for peak width and shape
- Better for quantification

#### Peak Position
- Wavenumber at maximum absorbance
- Identifies specific bonds/compounds
- Can shift with chemical environment
- Used for compound identification

---

## 🧪 FTIR Analysis Workflow

### Step-by-Step Usage

#### Basic Comparison
1. **Load Baseline**: Reference "new" grease
2. **Load Sample**: Test grease after use
3. **View Plot**: See spectral overlay
4. **Export**: Save comparison graph

#### Advanced Analysis
1. **Load Data**: Baseline + sample(s)
2. **Enable FTIR Regions**: Visualize functional groups
3. **Find Peaks**: Automatic detection
4. **View Metrics**: Check RMSE and correlation
5. **FTIR Analysis**: Get health score and warnings
6. **Generate Report**: Create PDF with all findings

#### Quality Control
1. Load reference baseline (certified grease)
2. Load production batches as samples
3. Use RMSE < 0.10 as acceptance criteria
4. Pearson r > 0.95 for batch consistency
5. Check for contamination warnings

---

## 🎨 User Interface Guide

### Top Control Panel

#### Baseline Card
- **Load Baseline CSV**: Select local FTIR CSV file
- **Load Baseline from DB**: Retrieve from cloud storage
- Shows currently loaded baseline name

#### Samples Card
- **Load Sample CSV(s)**: Select one or more local files
- **Load Sample(s) from DB**: Retrieve from cloud
- **Clear Samples**: Remove all loaded samples
- Shows count of loaded samples

#### Export Card
- **Choose Save Folder**: Set output directory
- **Format**: PNG, JPG, or JPEG
- **DPI**: Image resolution (default: 180)

### Middle Control Panel

#### Left Side (Display Options)
- **Preview sample**: Dropdown to select which sample to display
- **X/Y Labels**: Customize axis labels
- **Tight layout**: Optimize plot spacing
- **Animate**: Enable/disable animation
- **Duration**: Animation speed in milliseconds
- **Show Peaks**: Toggle peak markers
- **FTIR Regions**: Show functional group bands
- **Region Labels**: Display chemical group names
- **Sensitivity**: Peak detection threshold
- **MinDist**: Minimum points between peaks
- **ΔA @ x=**: Wavenumber for delta calculation

#### Right Side (Actions)
- **Find Peaks**: Detect peaks in selected sample
- **Export Peaks CSV**: Save peak table
- **Show Metrics**: Display RMSE, Pearson r, ΔA
- **FTIR Analysis**: Comprehensive chemical analysis
- **Build PDF Report**: Generate branded report
- **Batch Save All Graphs**: Export all comparisons
- **Refresh Preview**: Redraw current plot

---

## 📁 File Formats

### Input: CSV Files

Expected format:
```
Wavenumber,Absorbance
4000.0,0.0234
3999.5,0.0235
3999.0,0.0237
...
```

**Requirements**:
- At least 2 numeric columns
- First column: Wavenumber (cm⁻¹)
- Second column: Absorbance
- Comma, semicolon, or tab delimited
- Header row optional

### Output Formats

#### PNG/JPG Images
- High-resolution graphs
- Configurable DPI (72-600)
- Theme-aware colors
- Professional appearance

#### PDF Reports
- Page 1: Full spectrum plot with peaks
- Page 2: Metrics, peak table, metadata
- A4 size, print-ready
- MRG Labs branding

#### Peak CSV Export
```
x_cm^-1,A
2920.5,0.456
2851.2,0.423
1720.8,0.189
...
```

---

## 🔬 Scientific Background

### What is FTIR?

**Fourier Transform Infrared Spectroscopy** is an analytical technique that:
- Passes infrared light through a sample
- Measures which wavelengths are absorbed
- Each chemical bond absorbs specific wavelengths
- Creates a "fingerprint" unique to each substance

### Why Use FTIR for Grease Analysis?

1. **Non-destructive**: Sample can be reused
2. **Fast**: Results in minutes
3. **Comprehensive**: Detects many compounds simultaneously
4. **Quantitative**: Measure concentrations
5. **No consumables**: No solvents or reagents needed

### What FTIR Reveals About Grease

#### Base Oil Analysis
- **C-H peaks (2850-2960 cm⁻¹)**: Hydrocarbon chains
  - Mineral oil: Strong, sharp peaks
  - Synthetic esters: Additional C=O peak
  - PAO (polyalphaolefin): Very strong C-H

#### Degradation Markers
- **C=O peak (1700-1750 cm⁻¹)**: Oxidation products
  - Fresh grease: Minimal or absent
  - Used grease: Grows with oxidation
  - Severity correlates with intensity

#### Contamination Detection
- **O-H peak (3200-3600 cm⁻¹)**: Water content
  - Broad peak: Free water
  - Sharp peak: Bound water/alcohols
  - Glycol contamination: Distinctive pattern

#### Additives
- **Aromatic C-H (3000-3100 cm⁻¹)**: Phenolic antioxidants
- **P=O, S=O peaks**: EP/AW additives
- **Metal soaps**: 1550-1600 cm⁻¹

---

## 📈 Detailed Metric Explanations

### RMSE (Root Mean Square Error)

#### Mathematical Definition
RMSE quantifies the average magnitude of differences between two spectra. It's calculated by:

1. Interpolating both spectra to a common wavelength grid
2. Computing point-by-point differences
3. Squaring each difference
4. Taking the mean of squared differences
5. Taking the square root of the mean

#### Why It's Useful
- **Objective measure**: No subjective interpretation
- **Comprehensive**: Considers entire spectrum
- **Comparable**: Can compare across different samples
- **Quality control**: Set acceptance thresholds

#### Practical Application
```
Example 1: New vs Aged Grease
- Fresh grease RMSE: 0.03 (vs baseline)
- After 1 year: RMSE: 0.12
- After 2 years: RMSE: 0.28
→ Conclusion: Significant degradation, replacement needed

Example 2: Batch Consistency
- Batch A RMSE: 0.05
- Batch B RMSE: 0.06
- Batch C RMSE: 0.18
→ Conclusion: Batch C failed QC, investigate production
```

#### Factors Affecting RMSE
- Peak height changes (oxidation, dilution)
- Peak position shifts (different additives)
- Baseline drift (instrument differences)
- Noise levels in spectra

---

### Pearson Correlation Coefficient

#### Mathematical Definition
Pearson's r measures the strength and direction of linear relationship:

```
r = Σ[(x_i - x̄)(y_i - ȳ)] / √[Σ(x_i - x̄)² × Σ(y_i - ȳ)²]
```

where x̄ and ȳ are means of baseline and sample absorbances

#### Why It's Useful
- **Pattern matching**: Identifies similar compositions
- **Intensity-independent**: Works even if diluted
- **Grease type identification**: Group similar greases
- **Additive package comparison**: Same additives = high r

#### Interpretation Guide
| r value | Interpretation | Action |
|---------|----------------|--------|
| 0.98-1.00 | Nearly identical | Same grease type, good condition |
| 0.95-0.98 | Very similar | Same family, minor differences |
| 0.90-0.95 | Similar | Same base type, different additives |
| 0.80-0.90 | Moderately similar | Different grades/formulations |
| <0.80 | Different | Different grease types or severe degradation |

#### Practical Application
```
Example 1: Grease Identification
Unknown Sample vs Library:
- Mobil XHP 222: r = 0.96
- Shell Gadus: r = 0.82
- Mobilith SHC: r = 0.78
→ Conclusion: Likely Mobil XHP 222 or similar formulation

Example 2: Degradation vs Contamination
Fresh Grease vs Used:
- r = 0.92, RMSE = 0.25
→ High correlation but high error suggests contamination
  (foreign grease mixed in, maintaining similar pattern but different intensity)

- r = 0.73, RMSE = 0.28
→ Low correlation and high error suggests severe oxidation
  (chemical structure changed, different pattern)
```

#### Why r Alone Isn't Enough
- High r with high RMSE: Same pattern, different scale
- Low r with low RMSE: Rare, suggests systematic shift
- **Always use r + RMSE together** for complete picture

---

### Delta Absorbance (ΔA)

#### Mathematical Definition
```
ΔA(x) = Absorbance_sample(x) - Absorbance_baseline(x)
```

where x is a specific wavenumber (cm⁻¹)

#### Why It's Useful
- **Targeted monitoring**: Track specific compounds
- **Oxidation tracking**: Monitor C=O growth
- **Additive depletion**: Watch specific peaks decrease
- **Water ingress**: Quantify O-H increase

#### Strategic Monitoring Points

##### 1. Oxidation Tracking (1720 cm⁻¹)
```
Fresh grease: ΔA ≈ 0
After 3 months: ΔA = +0.08 (mild oxidation)
After 6 months: ΔA = +0.22 (moderate oxidation)
After 12 months: ΔA = +0.45 (severe oxidation)
```

**Action Thresholds**:
- ΔA < 0.10: Continue use
- ΔA 0.10-0.30: Increase monitoring frequency
- ΔA > 0.30: Plan replacement

##### 2. Base Oil Monitoring (2920 cm⁻¹)
```
Fresh grease: ΔA ≈ 0
After contamination: ΔA = -0.15 (oil loss/dilution)
```

Negative ΔA indicates:
- Base oil consumption
- Evaporation
- Leakage/dilution

##### 3. Water Contamination (3400 cm⁻¹)
```
Dry grease: ΔA ≈ 0
After water exposure: ΔA = +0.18
```

**Severity Scale**:
- ΔA < 0.05: Trace moisture (normal)
- ΔA 0.05-0.15: Light contamination
- ΔA 0.15-0.30: Moderate contamination
- ΔA > 0.30: Severe contamination

#### Practical Application
```
Example: Sealed Bearing Grease Monitoring

Month 0 (baseline):
- C-H @ 2920: 0.520
- C=O @ 1720: 0.042
- O-H @ 3400: 0.018

Month 6:
- C-H @ 2920: 0.498  (ΔA = -0.022)
- C=O @ 1720: 0.089  (ΔA = +0.047)
- O-H @ 3400: 0.024  (ΔA = +0.006)

Interpretation:
✓ Minimal C-H loss (normal consumption)
⚠️ C=O increase indicates mild oxidation
✓ O-H stable (seals working)

Action: Continue use, recheck in 3 months
```

---

## 🎓 Advanced Usage Tips

### Optimizing Peak Detection

#### Sensitivity Setting
- **0.05**: Detects even minor peaks (many peaks)
- **0.10**: Balanced detection (recommended)
- **0.20**: Only major peaks
- **0.30**: Very selective

**When to adjust**:
- Noisy data → Increase sensitivity
- Too many peaks → Increase sensitivity
- Missing obvious peaks → Decrease sensitivity

#### Minimum Distance
- **5 points**: Standard (recommended)
- **10 points**: Broader peaks
- **20+ points**: Very broad regions

### Best Practices

#### Data Quality
- Use consistent instrument settings
- Same resolution across samples
- Proper baseline correction
- ATR vs transmission: note collection method

#### Comparison Strategy
1. **Same grease type**: Use RMSE primarily
2. **Unknown identification**: Use Pearson r first
3. **Degradation tracking**: Monitor ΔA at key peaks
4. **Contamination detection**: Use FTIR Analysis warnings

#### When to Suspect Issues
- RMSE suddenly doubles
- Pearson r drops below 0.85
- New peaks appear
- Expected peaks disappear
- Health score < 70

---

## 🔧 Troubleshooting

### Common Issues

**Problem**: No peaks detected
- **Solution**: Decrease sensitivity (0.05-0.08)
- **Check**: Data has adequate signal-to-noise ratio

**Problem**: Too many peaks
- **Solution**: Increase sensitivity (0.15-0.25)
- **Adjust**: Increase minimum distance

**Problem**: FTIR regions not showing
- **Solution**: Check "FTIR Regions" checkbox
- **Verify**: Data covers functional group ranges

**Problem**: Metrics show "—"
- **Solution**: Ensure baseline and sample overlap in wavenumber range
- **Check**: Both files have valid numeric data

**Problem**: Firebase not connecting
- **Solution**: Check credentials in FIREBASE_CREDENTIALS section
- **Verify**: Firebase project exists and bucket is created

---

## 💡 Tips & Best Practices

### For Best Results
- Use consistent instrument settings across samples
- Ensure proper baseline correction in your FTIR software
- Compare samples from the same grease type/family
- Monitor key peaks over time for trend analysis

### Common Use Cases
- **Quality Control**: Verify batch consistency
- **Predictive Maintenance**: Track grease degradation
- **Failure Analysis**: Identify contamination sources
- **Research**: Quantify oxidation and aging

---

## 📧 Support & Feedback

**Developed for**: Schneider Prize for Technology Innovation 2025  
**Organization**: MRG Labs  

For questions or support, please contact the development team through the repository.

---

## 📚 References & Further Reading

### FTIR Spectroscopy
- ASTM E2412: Standard Practice for FTIR Condition Monitoring
- ASTM D7418: FTIR of Lubricants
- ISO 10478: FTIR Analysis Guidelines

### Grease Analysis
- NLGI Grease Production Survey
- Lubricant Analysis Handbook
- Tribology & Lubrication Technology journal

### Chemical Interpretation
- Socrates, G. "Infrared and Raman Characteristic Group Frequencies"
- Coates, J. "Interpretation of Infrared Spectra"

---

## 📄 License

MIT License - See repository for details

---

## 👥 Credits

**Developed for**: Schneider Prize for Technology Innovation 2025  
**Organization**: MRG Labs  
**Hackathon**: 2025 Innovation Challenge  

---

## 🆘 Support

For technical support:
- Check this README first
- Review sample data examples
- Verify data format matches specifications
- Ensure all dependencies are installed

---

**Last Updated**: November 2025  
**Version**: 3.0 (AI-Powered Analysis Platform)

---

## 🎓 Complete Feature Reference

### All Available Analysis Tools

| Category | Features | Button/Menu Location |
|----------|----------|---------------------|
| **Basic Analysis** | Peak finding, Metrics, FTIR regions | Analysis menu |
| **AI/ML Tools** | Suggest Match, Anomaly Check, PCA Map | Analysis menu |
| **Advanced Processing** | Preprocessing, DTW alignment, Deconvolution | Advanced panel |
| **Batch Operations** | Batch save graphs, Batch analyze CSV | Top buttons |
| **Reports** | PDF report, AI narrative, Provenance JSON | Various |
| **ML Prediction** | Load calibration, Predict target | Tool buttons |
| **Quality Control** | QC thresholds, Template alerts | Tool buttons |

### Keyboard Shortcuts & Tips

- **Click axis labels** to edit X/Y labels
- **Hover over plot** to see data values (if tooltips enabled)
- **Dark mode toggle** in top-right for comfortable viewing
- **Animation toggle** for faster refreshing with many samples

---

## 🔬 Advanced Workflows

### Workflow 1: Predictive Maintenance Program

**Goal**: Track grease degradation over time and predict replacement

**Steps**:
1. Establish baseline with fresh grease
2. Sample monthly (same bearing location)
3. Use "Batch Analyze → CSV" for all samples
4. Plot oxidation index vs time in Excel
5. Train calibration model with TAN measurements
6. Predict remaining life of new samples

**Metrics to Monitor**:
- Oxidation Index (primary)
- RMSE (overall change)
- Water Index (contamination)
- C=O peak height (carbonyl growth)

---

### Workflow 2: Quality Control in Production

**Goal**: Verify batch consistency

**Steps**:
1. Load certified reference as baseline
2. Load production batch samples
3. Set QC thresholds (e.g., RMSE < 0.08)
4. "Batch Analyze → CSV" all batches
5. Review status (OK/WARN/FAIL)
6. Flag outliers for investigation

**Decision Criteria**:
- RMSE < 0.05: Accept
- RMSE 0.05-0.10: Accept with note
- RMSE > 0.10: Reject, investigate

---

### Workflow 3: Unknown Grease Identification

**Goal**: Identify unknown sample

**Steps**:
1. Build library of known greases (load as samples)
2. Load unknown as sample to analyze
3. "Suggest Match (AI)" to find closest matches
4. Check Pearson r > 0.90 for confident ID
5. Use "FTIR Analysis" to verify functional groups
6. Compare health score to assess condition

---

### Workflow 4: Research & Publications

**Goal**: Generate publication-quality data

**Steps**:
1. Load all experimental samples
2. Configure preprocessing (document settings!)
3. Use custom band windows matching literature
4. "Peak Deconvolution" for overlapping regions
5. Export deconvolution CSVs for statistical analysis
6. "Export Provenance" for methods section
7. Generate high-DPI graphs (300+ DPI)

**Reproducibility**:
- Provenance JSON contains all settings
- Screenshots of UI show parameter values
- Deconvolution CSVs have component details
- PDF reports time-stamped and archived

---

## 💻 System Requirements

### Minimum
- **OS**: Windows 10, macOS 10.14+, Linux (Ubuntu 20.04+)
- **Python**: 3.8 or newer
- **RAM**: 4 GB
- **Disk**: 500 MB (1 GB recommended for data)
- **Display**: 1280x720 or larger

### Recommended
- **OS**: Windows 11, macOS 12+, Ubuntu 22.04+
- **Python**: 3.10 or newer
- **RAM**: 8 GB or more
- **Disk**: 2 GB (for sample storage)
- **Display**: 1920x1080 or larger (for comfortable viewing)

---

## 🆕 Version History

### v3.0 (November 2025) - AI-Powered Platform
- Added AI report generator (LLM + local fallback)
- Implemented batch CSV analysis
- Added supervised ML calibration regressor
- Implemented peak deconvolution (Gaussian fitting)
- Added signal preprocessing (Savitzky-Golay, baseline correction)
- Implemented DTW alignment for spectral drift
- Added template alerts for instant chemistry insights
- Implemented QC thresholds with pass/fail assessment
- Added provenance JSON export for compliance
- Implemented interactive hover tooltips
- Fixed sklearn lazy-loading for faster startup
- Added advanced band metrics (Water Index)
- Enhanced UI with 3 new control panels
- Added 6 new tool buttons

### v2.0 (October 2025) - FTIR Analysis Edition
- Implemented FTIR-specific functional group analysis
- Added grease health score calculation
- Created automated contamination detection
- Added spectral region visualization
- Implemented PDF report generation
- Added Firebase cloud integration
- Created dark mode theme
- Animated plotting for visual appeal

### v1.0 (September 2025) - Initial Release
- Basic CSV plotting
- Baseline vs sample comparison
- Simple metrics (RMSE, correlation)
- Export to PNG/JPG
- File management

---

## 🏆 Schneider Prize Submission Highlights

This tool demonstrates innovation in:

1. **AI Integration**: Dual-mode LLM reporting with privacy-first design
2. **Machine Learning**: Supervised learning for predictive maintenance
3. **Signal Processing**: Professional-grade preprocessing algorithms
4. **User Experience**: Intuitive UI with instant feedback
5. **Scalability**: Handles 1 to 1000+ samples efficiently
6. **Compliance**: Built-in provenance tracking for regulations
7. **Accessibility**: Works offline with graceful feature degradation

---

**Ready to revolutionize FTIR grease analysis!** 🚀
