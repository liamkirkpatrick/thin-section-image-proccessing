# Thin Section Image Proccessing

This repository contains a Jupyter notebook for stitching multiple ice core thin-section images into a single, high-fidelity composite at full resolution. 

The pipeline is explicitly designed for memory efficiency on hardware with limited RAM (e.g., 16 GB) when handling large numbers of high-resolution RAW camera files (.NEF). It first computes a fast geometric alignment blueprint using heavily downsampled versions of the images in a flat 2D grid scan configuration. It then applies this spatial blueprint to the full-resolution data to generate the final high-magnification canvas, using explicit memory cleanup to prevent system bottlenecks during batch processing.

* this is vibe-coded (thanks Gemni and Claude!) - use caution and double check output is what you expect *

## Repository Contents
- environment.yml: *contains environment variables for conda environment*
- stich.ipynb: *Jupyter notebook for combining thin section images*

## Folder Structure
- raw_data: *folder contains a folder for each thin section. Sub-folders shoudl be titled with the core-section-sample*
- image_outputs: *folder includes final image outputs*
