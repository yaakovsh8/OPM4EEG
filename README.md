

# 🧠 Order Pattern Matching for ADHD analysis using EEG waves

# Accessing External Project Files

Due to GitHub’s 25 MB file size limit, large project assets are hosted on Mega.nz. Follow the steps below to download, extract, and integrate them into this repository.

---

## 1. Download from Mega.nz

1. Open your web browser and navigate to:

   > [https://mega.nz/folder/nQxinQZA#YTR33\_RAgW5PNK3yTuYVEQ](https://mega.nz/folder/nQxinQZA#YTR33_RAgW5PNK3yTuYVEQ)

2. Click **Download** (or **Download as ZIP**) to download the entire folder as a ZIP archive.

3. Wait for the download to complete.

## 2. Extract and Place Files

1. Locate the downloaded ZIP file (usually in your **Downloads** folder).
2. Right-click the ZIP file and choose **Extract All...** (or use your preferred extraction tool).
3. When prompted for the destination, select a folder named **OPM4EEG\_Project\_files** at the root of this repository:

   ```
   <path-to-your-repo>/OPM4EEG_Project_files/
   ```

   After extraction, the folder structure should resemble:

   ```plaintext
   OPM4EEG_Project_files/
   ├── OPM4EEG.zip
   └── (other files and folders)
   ```

## 3. Integrate into the Project

* Ensure the **OPM4EEG\_Project\_files** folder is located in the root of the cloned repository before running any scripts or the installer.
* The installer (`OPM4EEG_Approach_1_Installer.exe`) will automatically detect and include files from this directory.

## 4. External Files Contents

Inside **OPM4EEG\_Project\_files**, you will find:

* **OPM4EEG.zip** – the complete codebase, resources, and data.
* Additional documentation (PDFs, user guides), video tutorials, and installer executable.

## 5. Important Notes

* Do **not** rename or reorganize any files or folders, as scripts and the installer rely on fixed paths.
* If you need to update or replace files, extract them again into the same directory structure.

---

You are now ready to install and run the project using the provided installer or scripts. Good luck with your analysis and development!

## Overview 

This capstone project explores whether recurring **EEG waveform patterns** can distinguish between children diagnosed with **ADHD** and a control group of neurotypical children.

We developed a modular and efficient **EEG signal processing pipeline** that supports two complementary research approaches using **Order Preserving Matching (OPM)**:

* **Approach 1**: Pattern discovery based on **waveform structure** (morphology of individual cycles).
* **Approach 2**: Pattern discovery based on **sequences of frequency bands** over time.

> 🎯 The ultimate goal is to support early ADHD diagnosis through interpretable EEG pattern recognition tools.

---

## 🧪 Dataset

* **Subjects**: 121 children (61 ADHD, 60 control)
* **Sampling rate**: 128 Hz
* **Channels**: 19 (10–20 system)
* **Task**: Visual attention task (character counting in cartoon frames)
* **Diagnosis**: DSM-IV, confirmed by psychiatrists, all ADHD subjects were treated with Ritalin

---

## 🧩 Research Approaches

### 🔹 Approach 1: Morphological Pattern Detection (Waveform Shape)

* **Preprocessing**:

  * High-pass, low-pass, notch filter (50 Hz), CAR
  * Decomposition via **AMICA**
  * Theta band isolation (4–8 Hz)
  * Hilbert transform to extract envelope and phase
* **Pattern Extraction**:

  * Detect bursts with ≥3 cycles using phase criteria (≥6π)
  * Encode each cycle using **OPM**
  * Store and compare patterns using a **hash-table-based engine**
* **Goal**: Identify cycle shapes unique to ADHD subjects (≥90% appearance rate in ADHD, rare in control)

### 🔹 Approach 2: Symbolic Band Sequence Detection (Macro EEG Trends)

* **Preprocessing**:

  * FastICA with automatic artifact rejection (based on kurtosis)
  * Frequency normalization and segmentation into 1–2 second windows
  * Band labeling (δ, θ, α, β, γ) via Welch’s method
* **Pattern Matching**:

  * Dual filtering: OPM structure + symbolic band sequence
  * Pattern is valid if it:

    * Appears in ≥80% of one group
    * Is consistent in OPM order and label sequence
    * Appears in the same channel across subjects

---

## 🧠 Key Technologies

| Component           | Description                                |
| ------------------- | ------------------------------------------ |
| `AMICA`             | ICA algorithm for source separation        |
| `OPM`               | Order Preserving Matching algorithm        |
| `Python`            | Main programming language                  |
| `NumPy`, `SciPy`    | Signal processing and numerical operations |
| `Pandas`, `Parquet` | Efficient motif data storage & retrieval   |
| `Tkinter`           | Interactive GUI for EEG motif browsing     |

---

## 📊 Results

### Approach 1:

* Detected highly specific **morphological cycles** recurring in ADHD group only
* Best results from **IC-based Hilbert-transformed patterns**
* PPV (Positive Predictive Value) scores calculated for each of 8 configurations

### Approach 2:

* Found **symbolic sequences** (like \['θ','β','β']) recurring in ADHD but rare in control
* Repeated tests across time windows (1s, 2s) revealed time-scale-sensitive markers
* Discovered both **ADHD-distinctive** and **Control-distinctive** sequences

---

## 🛠 How to Run

```bash
# Step 1: Run ICA using AMICA
python approach_1/code/artifact_cleaning/ultimate_cleaner.py

# Step 2: Run motif discovery on raw/IC/Hilbert/etc.
python approach_1/code/pattern_matching/run_pattern_matcher.py

# Step 3: View patterns with GUI
python approach_1/code/gui_app/main.py
```

✔ Output patterns are saved in `.parquet` and `.json` formats
✔ GUI lets you explore matches, frequencies, and channel distributions

---

## 🧩 Folder Structure

```
project_root/
│
├── approach_1/
│   ├── code/
│   │   ├── artifact_cleaning/   # AMICA + filters
│   │   ├── gui_app/             # Main Tkinter viewer
│   │   ├── pattern_matching/    # OPM encoding + matching
│   ├── data/
│   │   ├── raw/                 # .mat EEG files
│   │   ├── clean/               # Cleaned .npy EEG data
│   │   ├── results/             # OPM pattern outputs
│
├── approach_2/
│   ├── code/                    # Frequency band analysis
│   ├── data/                    # .npy EEG segments
│   ├── results/                 # Symbolic pattern outputs
│
├── docs/                        # Project documentation
└── README.md
```

---

## 📈 Contributions & Learnings

✔ Discovered waveform and sequence patterns that differentiate ADHD from controls
✔ Developed a dual-mode symbolic pattern matching pipeline
✔ Optimized runtime from days to minutes using **hashing** and **memory write-back** techniques
✔ Identified limitations of longer symbolic sequences (e.g., 6-band) in ADHD group
✔ Proposed future work to merge both approaches into multichannel temporal pattern mining

---

## 📚 Future Work

* Analyze cross-channel transitions after key motifs
* Build classifiers using high-confidence motifs
* Extend analysis to additional EEG datasets or age groups
* Explore clinical application for real-time ADHD screening

---

## 👥 Authors

* Yaakov Shitrit
* Tamir Nahari
  👩‍🏫 Guided by: Dr. Anat Dahan, Dr. Samah Idrees Ghazawi

---
