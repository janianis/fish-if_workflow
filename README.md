## Origin and acknowledgements

This FISH–IF workflow was adapted from the napari-flofish project:
https://github.com/SanderSMFISH/napari-flofish

That repository is a fork of the original project:
https://github.com/LaToyaVanderB/napari-flofish

FloFish source used:
https://github.com/SanderSMFISH/flofish

FloFish commit:
fc185bb8173c01bc6e4781d7ccade5a463ac5b60

This adaptation adds manual mask-alignment review, nuclear-overlap
filtering, per-cell mRNA and protein-associated fluorescence
quantification, and separate QC plots for each image field.


# FISH–IF notebook workflow

A notebook-based workflow for image processing and per-cell quantification using FloFish and Big-FISH, with segmentation using Omnipose and visual mask review in napari.

Channels are assigned by their analysis roles in the configuration rather than by fixed fluorophore names.

## Setup

1. Open the workflow notebook in Jupyter.
2. Select the Python environment containing its dependencies.
3. Set the input directory, JSON configuration path and output directory.
4. Confirm that input files are accessible.
5. Use an empty output directory for Stage 1.

Record the Python and package versions used for the analysis.

## Configuration

| Variable | Purpose |
|---|---|
| `CONFIG_FILE` | Input JSON configuration path. |
| `CONFIG` | Configuration loaded into Python. |
| `OUTPUT_ROOT` | Stage 1 output directory. |
| `runtime_config` | Working configuration used for processing. |

Use the configuration schema expected by the notebook. Check:

- Input-channel mapping and alignment.
- Voxel dimensions and units.
- Spot-radius and filtering parameters.
- Detection thresholds for each spot-analysis channel.
- Segmentation and mask-retention settings.
- Intensity projection and background correction.

### Channel roles

| Role | Purpose |
|---|---|
| Cell-segmentation image | Generate cell masks. |
| Nuclear-reference channel | Generate nuclear masks and support overlap-based retention where used. |
| Spot-analysis channel | Detect and optionally decompose fluorescent spots for per-cell counting. |
| Intensity-analysis channel | Measure fluorescence intensity within cell masks. |

Assign the dataset’s channels to the roles supported by the notebook. Channel names, indices and fluorophores are dataset-specific.

## Workflow overview

1. Read and align the configured channels; export input layers.
2. Generate cell masks and, where applicable, nuclear masks.
3. Inspect mask alignment in napari and save corrections.
4. Process configured spot-analysis channels for focus selection, filtering, detection and decomposition where enabled.
5. Apply the configured mask-retention criteria.
6. Assign spot coordinates to retained masks and quantify configured intensity-analysis channels.
7. Export per-cell measurements, image-level QC tables and plots.

Check whether final counts use detected or decomposed coordinates and which background statistic is used for intensity correction.

## Running Stage 1

Run imports, configuration and processing cells in order.

Use a new, empty output folder for a fresh run. Inspect partial outputs before restarting an interrupted run.

## Using different channels

Verify that the configured channel mapping is used consistently in image reading, alignment, detection, intensity quantification, output filenames and plotting. These steps must not rely on hard-coded channel names from a previous dataset.
