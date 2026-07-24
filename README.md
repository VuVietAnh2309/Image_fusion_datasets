# CoRe-Fusion — Multimodal Medical Image Fusion Datasets

This repository hosts the three co-registered functional–anatomical image sets used to develop and
evaluate **CoRe-Fusion: Controllable Recombination for Training-Free Multimodal Medical Image Fusion**.
All images are 8-bit PNG, spatially registered, and resized to **256×256**. Within each split, a
functional image and its anatomical counterpart share the **same file name** (e.g.
`PET/25015.png` ↔ `MRI/25015.png` form one pair).

> These images are **derived** (registered, intensity-normalized, resized) from publicly available
> sources for research use. If you use them, please **cite the original data sources** listed below and
> comply with their terms of use.

## Datasets

| Dataset | Modality A | Modality B | Train | Val | Test | Source | In paper |
|---|---|---|---|---|---|---|---|
| **PET–MRI**   | PET (pseudo-colour, functional)   | MRI (grayscale) | 215 | 30 | 24 | Harvard Whole Brain Atlas | ✓ |
| **SPECT–MRI** | SPECT (pseudo-colour, functional) | MRI (grayscale) | 303 | 30 | 24 | Harvard Whole Brain Atlas | ✓ |
| **PET–CT**    | PET (grayscale, functional)       | CT (grayscale)  | —   | —  | 56 | TCIA Soft-Tissue-Sarcoma  | ✓ (external) |
| **CT–MRI**    | CT (grayscale)                    | MRI (grayscale) | 160 | —  | 24 | Harvard Whole Brain Atlas | — |

- **PET–MRI** and **SPECT–MRI** are the *primary* brain datasets, drawn from the Harvard Whole Brain
  Atlas (co-registered brain scans spanning normal ageing and neurological conditions such as
  Alzheimer's disease, Huntington's disease, cerebral metastases and glioma).
- **PET–CT** is a *held-out external cohort* from a different repository and body region (pelvis/thigh
  soft-tissue sarcoma), used only as a frozen-configuration generalization test. Both of its modalities
  are grayscale (raw SUV / Hounsfield intensities). It has **no** train/val split.
- **CT–MRI** is an *anatomical–anatomical* pair (both grayscale) from the Harvard atlas. It is **not used**
  in the CoRe-Fusion paper — the method targets functional–anatomical fusion — and is included here only
  for dataset-management convenience. It has no validation split.

## Splits (PET–MRI, SPECT–MRI)

- **`train/`** — training images (used to build/verify the method; CoRe-Fusion itself is training-free).
- **`val/`** — a **30-pair** validation subset drawn from the training split by uniform stride, used
  solely to select the single output-control hyperparameter (the amplification bound `g_max`).
- **`test/`** — the held-out evaluation split (24 pairs) reported in the paper.
- `train` and `val` are **disjoint**: `train` = original training set **minus** the 30 validation pairs
  (245 − 30 = 215 for PET–MRI, 333 − 30 = 303 for SPECT–MRI).

## Folder structure

```
CoRe-Fusion_datasets/
├── PET-MRI/
│   ├── train/{PET, MRI}     215 pairs
│   ├── val/{PET, MRI}        30 pairs
│   └── test/{PET, MRI}       24 pairs
├── SPECT-MRI/
│   ├── train/{SPECT, MRI}   303 pairs
│   ├── val/{SPECT, MRI}      30 pairs
│   └── test/{SPECT, MRI}     24 pairs
├── PET-CT/                   (external test only)
│   ├── PET/  56
│   └── CT/   56
└── CT-MRI/                   (not used in the paper; provided for completeness)
    ├── train/{CT, MRI}      160 pairs
    └── test/{CT, MRI}        24 pairs
```

## Notes on preprocessing

- **PET–MRI / SPECT–MRI:** functional scans are pseudo-colour (metabolic/perfusion activity); the MRI is
  grayscale soft-tissue anatomy. All pairs are already co-registered in the source atlas.
- **PET–CT:** exported from the TCIA Soft-Tissue-Sarcoma FDG-PET/CT volumes — tumour-containing axial
  slices only, intensity-normalized (1–99 percentile) to 8-bit and resampled to 256×256. The included
  56-slice test set is a patient-balanced subsample (≈8 slices from each of 7 patients).

## Sources and citations

**Harvard Whole Brain Atlas** (PET–MRI, SPECT–MRI) — Keith A. Johnson and J. Alex Becker, Harvard
Medical School. <http://www.med.harvard.edu/AANLIB/>

**TCIA Soft-Tissue-Sarcoma (STS) FDG-PET/CT collection** (PET–CT) — hosted on The Cancer Imaging
Archive; please cite both the collection paper and the TCIA infrastructure paper.

```bibtex
@misc{harvardatlas,
  title  = {The Whole Brain Atlas},
  author = {Johnson, Keith A. and Becker, J. Alex},
  howpublished = {\url{http://www.med.harvard.edu/AANLIB/}},
  note   = {Harvard Medical School},
  year   = {1999}
}

@article{vallieres2015sts,
  title   = {A radiomics model from joint FDG-PET and MRI texture features for the prediction of
             lung metastases in soft-tissue sarcomas of the extremities},
  author  = {Valli{\`e}res, Martin and Freeman, Carolyn R. and Skamene, Sonia R. and El Naqa, Issam},
  journal = {Physics in Medicine and Biology},
  volume  = {60}, number = {14}, pages = {5471--5496}, year = {2015},
  doi     = {10.1088/0031-9155/60/14/5471}
}

@article{clark2013tcia,
  title   = {The Cancer Imaging Archive (TCIA): Maintaining and Operating a Public Information Repository},
  author  = {Clark, Kenneth and Vendt, Bruce and Smith, Kirk and Freymann, John and Kirby, Justin and
             Koppel, Paul and Moore, Stephen and Phillips, Stanley and Maffitt, David and Pringle, Michael
             and Tarbox, Lawrence and Prior, Fred},
  journal = {Journal of Digital Imaging},
  volume  = {26}, number = {6}, pages = {1045--1057}, year = {2013},
  doi     = {10.1007/s10278-013-9622-7}
}
```

## Terms of use

The original data are subject to the terms of their respective providers (the Harvard Whole Brain Atlas
and the TCIA Data Usage Policy / the STS collection's license). These derived images are shared for
non-commercial academic research only; downstream users must observe the original licenses.
