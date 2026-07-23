# Thin Section Image Processing

This repository contains Jupyter notebooks for stitching multiple ice-core thin-section RAW images into a single high-fidelity composite. The main workflow in [stitch.ipynb](stitch.ipynb) is tuned for scan-style image sets captured on a fixed rig: it uses OpenCV `SCANS` mode, a low-resolution alignment pass, a high-resolution final pass, memory tracking, and a retry system that progressively relaxes confidence thresholds when a sample is difficult to stitch.

The pipeline is intentionally conservative about memory. It loads RAW `.NEF` files, builds a downsampled geometry pass first, then runs the full-resolution stitch only after the low-res pass clears an inclusion check. Large intermediates are deleted as soon as they are no longer needed, and the notebook logs resident memory so you can see where working set growth happens during a batch.

*This is still a work-in-progress notebook set. Double-check output quality before using it as a final scientific record.*

## Repository Contents
- `environment.yml`: Conda environment definition for the notebook runtime.
- `stitch.ipynb`: Main workflow. Supports external-drive or home-directory data roots, `SCANS` mode stitching, low-res gating, high-res retries, memory logging, and `_FLAG` output suffixes when retries were needed.
- `stitch_simple.ipynb`: Earlier simplified version of the scan-stitch pipeline with similar memory tracking.
- `stitch_lenscorrect.ipynb`: Experimental variant that adds optional lens-correction support before stitching. It is behind a toggle and depends on external metadata/lens-profile tooling.

## Current Directory Layout
Set these up before running the notebooks:

- `raw_data/`: Local sample root. Each subfolder should be a sample ID and contain that sample’s `.NEF` files.
- `image_outputs/`: Output folder for stitched composites.
- Optional external drive root: `stitch.ipynb` can point `raw_data_dir` at an external volume instead of `raw_data/` via a simple toggle in the parameter cell.

## What `stitch.ipynb` Does

The current main notebook is built for repeated batch stitching of scan-style image sets. For each sample it:

1. Loads all `.NEF` files in the sample folder.
2. Runs a low-resolution scan-mode stitch to check whether the geometry is good enough to proceed.
3. If the low-res pass succeeds, runs a high-resolution stitch with a separate retry schedule.
4. Keeps the best high-res result it sees, rather than rerunning the low-res pass on every retry.
5. Writes the final stitched image to `image_outputs/`, and adds `_FLAG` to the filename if the final result required more than one attempt.

The notebook is configured to be forgiving on difficult samples by using a gradual threshold schedule instead of a single hard cutoff. It is also designed to avoid stale-memory buildup by explicitly deleting large arrays and stitcher objects and by logging memory at major stages of the loop.

## How to Use

### 1. Set Up the Environment

This project uses a Conda environment to manage dependencies such as OpenCV and rawpy.

Clone the repository and enter it:

```bash
git clone https://github.com/liamkirkpatrick/thin-section-image-proccessing.git
cd thin-section-image-proccessing
```

Create and activate the environment:

```bash
conda env create -f environment.yml
conda activate thin-section-stitch
```

### 2. Prepare Your Data

Place sample folders under `raw_data/`, or point `stitch.ipynb` at an external drive by changing the `use_external_drive` setting in the parameter cell.

### 3. Run the Notebook

Launch Jupyter Lab or Jupyter Notebook from the activated environment:

```bash
jupyter lab
```

Then run the cells sequentially. You can either let the notebook discover all sample folders automatically or manually set `sampleID` to a specific list of section names.

### 4. Notes on the Lens-Correction Notebook

`stitch_lenscorrect.ipynb` is experimental. It includes optional lens-correction hooks, but it is not the default path and should be treated as an advanced variant rather than the primary workflow.

  
