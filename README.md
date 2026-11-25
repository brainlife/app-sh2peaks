<h1 align="center">Compute peaks from spherical harmonic data</h1>

<p align="center">
  A fibre-orientation peaks image from the spherical-harmonic FOD of a CSD dataset, with MRtrix3 sh2peaks.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.623"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.623-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

## 🧠 At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Extracts fibre orientation peaks from the spherical harmonic FOD at a chosen lmax of a CSD dataset with MRtrix3 sh2peaks, and outputs a peaks image.**

Selects the FOD image for the requested lmax from the CSD input and runs sh2peaks with optional number of peaks, amplitude threshold and fast mode, writing peaks.nii.gz. Use it when a downstream tool (for example TractSeg or FACT tracking) needs peak directions rather than FODs. Needs a CSD dataset containing the FOD at the requested lmax.

| | |
|---|---|
| 📥 **Inputs** | `neuro/csd` |
| 📤 **Outputs** | `neuro/peaks` |
| 🐳 **Container** | `brainlife/mrtrix3:3.0_RC3` |
| ⚙️ **Resources** | 8 CPUs · 40 GB memory · 0.5 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.623](https://doi.org/10.25663/brainlife.app.623) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `csd` | `neuro/csd` | yes | CSD dataset with the response function and FOD images per lmax. Must contain the FOD for the selected lmax. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `peaks` | `neuro/peaks` | peaks.nii.gz: 4D image of peak directions (three values per peak, scaled by amplitude). |

<details>
<summary><b>⚙️ Parameters</b> (4)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `lmax` | number | `8` | Spherical harmonic order of the FOD image to extract peaks from. |
| `num_peaks` | number | `3` | Number of peaks to extract per voxel (sh2peaks -num). |
| `threshold` | number | `0` | Minimum FOD amplitude for a peak to be kept (sh2peaks -threshold); 0 means no threshold. |
| `fast` | boolean | `false` | If true, use sh2peaks -fast (precomputed direction set, faster but less precise). |

</details>

<!-- brainlife-app:end -->

## 🚀 Running the App

### ☁️ On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.623](https://doi.org/10.25663/brainlife.app.623), choose a `neuro/csd` dataset from your project, set the options and press **Execute**. The App writes a `neuro/peaks` dataset to your project. Use it when a downstream tool (for example TractSeg or FACT tracking) needs peak directions rather than FODs.

- **`lmax`** (default 8): the spherical-harmonic order of the FOD image to extract peaks from. The CSD dataset must contain the FOD at this order.
- **`num_peaks`** (default 3): the number of peaks to extract per voxel (`sh2peaks -num`).
- **`threshold`** (default 0): the minimum FOD amplitude for a peak to be kept (`sh2peaks -threshold`); 0 means no threshold.
- **`fast`** (default `false`): use `sh2peaks -fast`, which uses a precomputed direction set and is faster but less precise.

### 💻 Locally

You need [Singularity/Apptainer](https://apptainer.org/) and [`jq`](https://jqlang.github.io/jq/). The App runs in the `brainlife/mrtrix3:3.0_RC3` container.

1. Clone this repository and check out the released version (`git checkout v1.0`).
2. Create `config.json` with the paths to the FOD images and the options you want:

   ```json
   {
       "lmax2": "/input/csd/lmax2.nii.gz",
       "lmax4": "/input/csd/lmax4.nii.gz",
       "lmax6": "/input/csd/lmax6.nii.gz",
       "lmax8": "/input/csd/lmax8.nii.gz",
       "lmax": 8,
       "num_peaks": 3,
       "threshold": 0,
       "fast": false
   }
   ```

   Add `lmax10`, `lmax12` or `lmax14` if you want to use a higher order.

3. Run the App:

   ```bash
   ./main
   ```

## 📦 Outputs

```text
peaks/
└── peaks.nii.gz   4D image of peak directions: three values (x, y, z) per peak, scaled by the peak amplitude
```

## 🩺 Troubleshooting

- **`something went wrong. check derivatives`**: `peaks.nii.gz` was not produced. Check that the CSD input contains the FOD at the requested `lmax`.
- **`threshold` has no effect**: this version compares `threshold` as an integer, so values that are not whole numbers are ignored.

## 📚 Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Tournier J-D, Smith RE, Raffelt D, Tabbara R, Dhollander T, Pietsch M, Christiaens D, Jeurissen B, Yeh C-H, Connelly A. MRtrix3: A fast, flexible and open software framework for medical image processing and visualisation. *NeuroImage* 202:116137 (2019).
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)
- Compute Peaks from Spherical Harmonic (SH) data. brainlife.io App. [doi:10.25663/brainlife.app.623](https://doi.org/10.25663/brainlife.app.623)
- Tournier et al. MRtrix3: A fast, flexible and open software framework for medical image processing and visualisation. *NeuroImage* 202:116137 (2019). [doi:10.1016/j.neuroimage.2019.116137](https://doi.org/10.1016/j.neuroimage.2019.116137)
- Wasserthal et al. TractSeg - Fast and accurate white matter tract segmentation. *NeuroImage* 183:239-253 (2018). [doi:10.1016/j.neuroimage.2018.07.070](https://doi.org/10.1016/j.neuroimage.2018.07.070)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)

## 👥 Authors

- Brad Caron

**Contributors:** Soichi Hayashi

## 💰 Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## ⚖️ License

MIT. MRtrix3 is distributed under the [Mozilla Public License 2.0](https://www.mrtrix.org/).
