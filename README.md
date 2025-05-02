# TIFF to Video for OVP Ingestion

[![DOI](https://zenodo.org/badge/976627153.svg)](https://doi.org/10.5281/zenodo.15324063)

A pragmatic workaround for ingesting static TIFF images into Online Video Platforms (OVPs) by converting them into short, static MP4 video files.

## Problem

OVPs often lack direct support for still image ingestion, requiring a workaround to centralize all visual assets.

## Solution

Convert TIFF files to short (e.g., 5-second) MP4 videos to leverage the OVP's video ingestion pipeline.

## Benefits

* Centralized management of all visual assets within the OVP.
* Utilization of OVP infrastructure for processing, organization, and delivery.
* Potential streamlining of workflows involving both still and moving images.

## Technical Implementation

This repository provides a shell script that uses FFmpeg to batch convert TIFF files to MP4 videos.

### Prerequisites

* [FFmpeg](https://ffmpeg.org/) installed on your system.

### Script (`scripts/tiff_to_mp4.sh`)

~~~```bash
#!/bin/bash

find . -maxdepth 1 -name "*.tif" -print0 | \
xargs -0 -I % bash -c ' \
  filepath="%"; \
  filename=$(basename "$filepath" .tif); \
  ffmpeg -framerate 1 -i "$filepath" \
         -c:v libx265 -preset veryslow -crf 16 -vf "scale=1920:1080,format=yuv444p" \
         -r 1 -t 5 -movflags +faststart \
         "$filename.mp4" \
'

echo "Conversion process complete. MP4 files created in the same directory as the TIFFs."
~~~

### Considerations
* Potential loss of TIFF-specific metadata.
* Trade-offs between video quality and file size.
* Processing time for large TIFF collections.
* Compatibility with the target OVP's supported video formats.
