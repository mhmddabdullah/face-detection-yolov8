# Face Detection with YOLOv8

A real-time face detection system built with YOLOv8n, trained on a Roboflow face-detection dataset and benchmarked against YOLOv5 on the same data under identical conditions.

## Overview

Face detection underpins a wide range of applications — security/CCTV monitoring, contactless device unlock, AR filters, and retail foot-traffic analytics. This project trains and evaluates **YOLOv8n** (the "nano" variant of YOLOv8, chosen for its speed/accuracy balance and ability to train on CPU) for face detection, then compares it directly against **YOLOv5** trained on the same dataset to identify which model is better suited for real-time, resource-constrained face detection.

## Dataset

Face detection dataset sourced from Roboflow ([billy-ukui4/face-fslja](https://universe.roboflow.com/billy-ukui4/face-fslja)), pre-split into train/valid/test with YOLO-format bounding box annotations.

> Note: the raw image dataset (~1,000+ images) is not included in this repository to keep it lightweight — only code, trained weights, and result artifacts are tracked. See "Reproducing" below for how to pull the dataset.

## Training Setup

- **Model:** YOLOv8n (72 layers, 3,006,233 parameters)
- **Hardware:** CPU-only — 11th Gen Intel Core i5-1135G7 @ 2.40GHz
- **Epochs:** 10 (training time: ~0.276 hours / 16.56 minutes)
- **Compute:** 8.1 GFLOPs; ~3.2ms preprocessing, ~117.7ms inference, ~6.9ms postprocessing per image

## Results

| Metric | Score |
|---|---|
| Precision | 0.934 |
| Recall | 0.815 |
| mAP@0.5 | 0.907 |
| mAP@0.5:0.95 | 0.778 |

## YOLOv8n vs. YOLOv5 Comparison

Both models were trained on the identical dataset and settings for a fair comparison:

| | YOLOv8n | YOLOv5 |
|---|---|---|
| Training time (10 epochs, CPU) | ~0.276 hours | ~0.35 hours |
| Accuracy | Higher | Lower |
| Inference output | Cleaner | — |

**YOLOv8n was the preferred model** — faster training, simpler setup, and slightly better accuracy on CPU hardware, making it the stronger choice for real-time face detection under compute constraints.

## Error Analysis

The model performed well overall but showed consistent failure patterns:
- **Tiny/distant faces** were often missed due to insufficient pixel detail
- **Extreme side angles or unusual poses** were sometimes undetected, likely due to underrepresentation in training data
- **Crowded scenes** occasionally produced false positives, misidentifying round objects (e.g. clocks) as faces

Suggested improvements: augmenting the training set with more small-face, side-angle, and hard-negative (no-face) examples.

## Tech Stack

Python, Ultralytics YOLOv8, YOLOv5

