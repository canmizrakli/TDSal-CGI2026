# TDSal: A Task-Based Top-Down Saliency Prediction Model

TDSal is a task-conditioned visual saliency prediction model that estimates where humans are likely to look in an image given an explicit natural-language viewing goal. Unlike conventional free-viewing saliency models, TDSal focuses on **top-down visual attention**, where gaze behavior is shaped by task intent.

The model combines spatial visual features from a truncated YOLOv5su backbone with task embeddings from Sentence-BERT, then fuses them through a lightweight transformer module to produce dense task-conditioned saliency maps.

## Overview

Human attention is not only driven by low-level visual cues such as color, contrast, or edges. In many real-world settings, people look at different regions of the same scene depending on what they are trying to do. For example, the same image can produce different fixation patterns when the viewer is asked to count people, detect motion, or identify actions.

TDSal models this behavior by conditioning saliency prediction on textual task prompts. Given an input image and a task description, the network predicts a saliency map that reflects goal-directed human attention.

## Key Features

- **Task-conditioned saliency prediction** using natural-language task descriptions.
- **YOLO-based visual backbone** for spatially rich object-centered image features.
- **Sentence-BERT task encoder** for compact semantic task representations.
- **Transformer-based fusion module** for cross-modal interaction between visual and textual features.
- **Dense saliency-map output** aligned with human fixation density maps.
- Evaluation using standard saliency metrics including CC, KLDiv, SIM, NSS, AUC-Borji, AUC-J, and sAUC.

## Architecture

TDSal consists of four main components:

1. **YOLO Backbone**  
   A pretrained YOLOv5su model is truncated at the SPPF module. The detection head is removed, and the resulting spatial feature map is used as the visual representation.

2. **Feature Projection Module (FPM)**  
   A 1×1 convolution reduces the YOLO feature dimensionality from 512 channels to 128 channels while preserving spatial structure.

3. **Task Encoder**  
   A Sentence-BERT MiniLM-L6 model encodes the natural-language task prompt into a sentence-level embedding, which is projected into the same dimensional space as the visual tokens.

4. **Transformer Fusion Module and Saliency Decoder**  
   Visual tokens and the task token are fused with a shallow transformer encoder. The fused representation is then decoded into a dense saliency map.

## Dataset

The experiments use a task-oriented eye-tracking dataset originally collected for studying visual saliency under free-viewing and task-oriented conditions. The dataset contains fixation density maps under four explicit task conditions and includes image stimuli from the Emotional Attention and Saliency in Crowd datasets.

**Dataset link:** [Task-based eye-fixation dataset](https://drive.google.com/drive/folders/18Q_9JxKyqPAGOL3WbDC7zwUnomNtYwbV?usp=share_link)

## Experimental Setup

- Input image size: `384 × 384`
- Ground-truth fixation density map size: `48 × 48`
- Predicted saliency map size: `96 × 96`, downsampled to `48 × 48` during training and evaluation
- Train/validation/test split: `70% / 15% / 15%`
- Optimizer: Adam
- Learning rate: `1e-4`
- Batch size: `8`
- Training epochs: `50`
- Random seed: `42`

## Results

TDSal was evaluated on a task-conditioned test split using standard saliency metrics. The final model achieved:

| Metric | Test Score |
|---|---:|
| CC | 0.6423 |
| KLDiv | 0.9270 |
| SIM | 0.5010 |
| NSS | 3.4583 |
| AUC-Borji | 0.9177 |
| AUC-J | 0.9515 |
| sAUC | 0.8649 |

## Authors

- Can Mizrakli — Karlsruhe Institute of Technology, Germany; TED University, Türkiye
- Tolga K. Capin — TED University, Türkiye

## License

The license for this repository will be specified before public release. Please check the repository license file before using the code, dataset utilities, or supplementary material.
