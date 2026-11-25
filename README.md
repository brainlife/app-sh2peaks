[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-brainlife.app.616-blue.svg)](https://doi.org/10.25663/brainlife.app.616)

# Compute Peaks from Spherical Harmonic (SH) data

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

**Extracts fibre orientation peaks from the spherical harmonic FOD at a chosen lmax of a CSD dataset with MRtrix3 sh2peaks, and outputs a peaks image.**

Selects the FOD image for the requested lmax from the CSD input and runs sh2peaks with optional number of peaks, amplitude threshold and fast mode, writing peaks.nii.gz. Use it when a downstream tool (for example TractSeg or FACT tracking) needs peak directions rather than FODs. Needs a CSD dataset containing the FOD at the requested lmax.

Run it on brainlife.io: [https://doi.org/10.25663/brainlife.app.623](https://doi.org/10.25663/brainlife.app.623)

| Input | Datatype | Required | Description |
|---|---|---|---|
| `csd` | `neuro/csd` | yes | CSD dataset with the response function and FOD images per lmax. Must contain the FOD for the selected lmax. |

| Output | Datatype | Description |
|---|---|---|
| `peaks` | `neuro/peaks` | peaks.nii.gz: 4D image of peak directions (three values per peak, scaled by amplitude). |

| Parameter | Type | Default | Description |
|---|---|---|---|
| `lmax` | number | `8` | Spherical harmonic order of the FOD image to extract peaks from. |
| `num_peaks` | number | `3` | Number of peaks to extract per voxel (sh2peaks -num). |
| `threshold` | number | `0` | Minimum FOD amplitude for a peak to be kept (sh2peaks -threshold); 0 means no threshold. |
| `fast` | boolean | `false` | If true, use sh2peaks -fast (precomputed direction set, faster but less precise). |

Runtime: container `brainlife/mrtrix3:3.0_RC3`; 8 CPUs; 40 GB memory; 0.5 h walltime.

Please cite:
- Avesani P. et al. The open diffusion data derivatives. Sci Data 6, 69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

<!-- brainlife-app:end -->

This app will compute the peaks from spherical harmonic (SH) data, including FOD images found within the csd dataytpe, using MRTrix3.

### Authors

- Brad Caron (bacaron@utexas.edu)

### Contributors

- Soichi Hayashi (shayashi@iu.edu)

### Funding Acknowledgement

brainlife.io is publicly funded and for the sustainability of the project it is helpful to Acknowledge the use of the platform. We kindly ask that you acknowledge the funding below in your publications and code reusing this code.

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)

### Citations

We kindly ask that you cite the following articles when publishing papers and code using this code.

1. Avesani, P., McPherson, B., Hayashi, S. et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6, 69 (2019). https://doi.org/10.1038/s41597-019-0073-y

2. Tournier, J.-D.; Smith, R. E.; Raffelt, D.; Tabbara, R.; Dhollander, T.; Pietsch, M.; Christiaens, D.; Jeurissen, B.; Yeh, C.-H. & Connelly, A. MRtrix3: A fast, flexible and open software framework for medical image processing and visualisation. NeuroImage, 2019, 202, 116137

#### MIT Copyright (c) 2020 brainlife.io The University of Texas at Austin and Indiana University

## Running the App

### On Brainlife.io

You can submit this App online at [https://doi.org/10.25663/brainlife.app.616](https://doi.org/10.25663/brainlife.app.616) via the 'Execute' tab.

### Running Locally (on your machine)

1. git clone this repo

2. Inside the cloned directory, create `config.json` with something like the following content with paths to your input files.

```json
{
    "track": "/input/track/track.tck",
    "lmax2": "/input/csd/lmax2.nii.gz",
    "lmax4": "/input/csd/lmax4.nii.gz",
    "lmax6": "/input/csd/lmax6.nii.gz",
    "lmax8": "/input/csd/lmax8.nii.gz",
    "lmax10": "/input/csd/lmax10.nii.gz",
    "lmax12": "/input/csd/lmax12.nii.gz",
    "lmax14": "/input/csd/lmax14.nii.gz",
    "mask": "/input/mask/mask.nii.gz",
    "lmax": 8,
    "num_peaks": 3,
    "threshold":    0,
    "fast": false
}
```

### Sample Datasets

You can download sample datasets from Brainlife using [Brainlife CLI](https://github.com/brain-life/cli).

```
npm install -g brainlife
bl login
mkdir input
bl dataset download
```

3. Launch the App by executing 'main'

```bash
./main
```

## Output

The main output of this App is a peaks datatype.

#### Product.json

The secondary output of this app is `product.json`. This file allows web interfaces, DB and API calls on the results of the processing.

### Dependencies

This App only requires [singularity](https://www.sylabs.io/singularity/) to run. If you don't have singularity, you will need to install following dependencies.   

- MRTrix3: https://www.mrtrix.org/

#### MIT Copyright (c) 2020 brainlife.io The University of Texas at Austin and Indiana University
