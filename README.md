# Thin Section Image Proccessing

This repository contains a Jupyter notebook for stitching multiple ice core thin-section images into a single, high-fidelity composite at full resolution. 

The pipeline is explicitly designed for memory efficiency on hardware with limited RAM (e.g., 16 GB) when handling large numbers of high-resolution RAW camera files (.NEF). It first computes a fast geometric alignment blueprint using heavily downsampled versions of the images in a flat 2D grid scan configuration. It then applies this spatial blueprint to the full-resolution data to generate the final high-magnification canvas, using explicit memory cleanup to prevent system bottlenecks during batch processing.

* this is vibe-coded (thanks Gemni and Claude!) - use caution and double check output is what you expect *

## Repository Contents
- environment.yml: *contains environment variables for conda environment*
- stich_simple.ipynb: *Jupyter notebook for combining thin section images - simple version*
- stitch.ipynb *My best version currently. Builds on stitch with the abilityt o pull data from external files, run larger batches with better memory management, and automatically adjust confidence thresholds*
- stitch_lenscorrect.ipynb *In-proccess script to apply a lens correction (based on lens data from .NEF metadata). Not currently working - do not use yet.*

## Folder Structure - set these up in the respository before use!
- raw_data: *folder contains a sub-folder for each thin section. Sub-folders should be titled with the sample ID, and contain a list of .NEF files*
- image_outputs: *folder includes final image outputs*
- (optional) extenal drive: *script allows you to point to an external drive, although you may have to give permissions for your jupyter workspace to access this path.*

## How to use

### 1. Set up the Environment

This project uses a Conda environment to manage dependencies (like OpenCV and rawpy) and prevent version conflicts. 

Open your terminal (or Anaconda Prompt on Windows) and run the following commands:

__Clone the repository and navigate into it__
- git clone [https://github.com/liamkirkpatrick/thin-section-image-proccessing.git](https://github.com/liamkirkpatrick/thin-section-image-proccessing.git)
- cd thin-section-image-proccessing

__Create the environment from the environment.yml file__
- conda env create -f environment.yml

__Activate the new environment__
- conda activate thin-section-stitch

### 2. Prepare Your data

Follow instructions above to set up the file structure.

### 3. Run the notebook

- Once your data is in place, launch Jupyter Lab or Jupyter Notebook from your activated environment by typing 'jupyter lab'
- Either mannually enter the samples you wish to run as a list of strings (sampleID) or use script to automatically read in all samples in raw_data folder
- Run the cells sequentially.

  
