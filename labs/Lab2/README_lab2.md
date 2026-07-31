# Lab 2: AI Inference with Jetson-Inference

**ECE 4930/6930 – Artificial Intelligence of Things**

> [!NOTE]
> **Total time:** Two class sessions  
> **Platform:** Seeed Studio reComputer with NVIDIA Jetson Nano  
> **Camera:** Sony IMX219 CSI camera  
> **Software:** Jetson-Inference

---

## Overview

In this laboratory, you will install and use the **Jetson-Inference** library to run pretrained deep-learning models on an NVIDIA Jetson edge AI platform.

Jetson-Inference provides optimized examples for several computer-vision tasks:

- Image classification
- Object detection
- Semantic segmentation
- Human pose estimation
- Real-time camera inference

Jetson-Inference is not required for every AI inference application. Other frameworks, such as PyTorch, TensorFlow, TensorRT, OpenCV, and NVIDIA DeepStream, can also perform inference.

This course uses Jetson-Inference because it provides a consistent and accessible interface for exploring multiple edge AI tasks.

---

## Software Stack

```text
Ubuntu and JetPack
        │
        ▼
CUDA, cuDNN, and TensorRT
        │
        ▼
Jetson-Inference
        │
        ▼
Classification, Detection,
Segmentation, and Pose Estimation
        │
        ▼
AIoT Application
```

JetPack provides the operating system and NVIDIA acceleration libraries. Jetson-Inference provides pretrained models, example applications, camera interfaces, and visualization tools.

---

## Learning Objectives

After completing both parts of this laboratory, you will be able to:

- Explain the role of Jetson-Inference in the Jetson software stack.
- Build and install an AI inference library on an embedded platform.
- Run a pretrained image-classification model.
- Perform object detection using static images and a live camera.
- Compare classification, detection, segmentation, and pose estimation.
- Monitor CPU, GPU, memory, and temperature during inference.
- Explain the benefits and limitations of real-time edge AI.

---

## Laboratory Organization

### [Part 1: Setup and Image Classification](Part1_Setup_and_Classification.md)

During the first class, you will:

1. Verify the Jetson software environment.
2. Obtain the Jetson-Inference source code.
3. Build and install Jetson-Inference.
4. Run a pretrained image-classification model.
5. Interpret the classification label and confidence.

### [Part 2: Detection, Segmentation, and Pose Estimation](Part2_Vision_Inference_Tasks.md)

During the second class, you will:

1. Perform object detection on a static image.
2. Run real-time object detection using the CSI camera.
3. Monitor system performance during inference.
4. Perform semantic segmentation.
5. Perform human pose estimation.
6. Compare the outputs and applications of the three tasks.

---

## Class Schedule

| Session | Activities |
|---------|------------|
| **Class 1 – 75 minutes** | Installation, system verification, and image classification |
| **Class 2 – 75 minutes** | Object detection, segmentation, pose estimation, and performance analysis |

---

## Equipment

Each group will receive:

- One Seeed Studio reComputer with NVIDIA Jetson Nano
- One Sony IMX219 CSI camera
- One prepared microSD card
- HDMI monitor
- Keyboard and mouse
- Power supply
- Network connection

Students will work in groups of approximately three to five.

Suggested group roles:

- **System operator:** enters commands and controls the Jetson
- **Recorder:** records outputs, measurements, and observations
- **Experiment coordinator:** manages objects, camera tests, and task timing

Rotate roles during the laboratory.

---

## Required Deliverables

Each group will submit one report containing:

- Installation and system-verification results
- Image-classification output
- Static object-detection output
- Screenshot of real-time object detection
- Screenshot of `tegrastats` during inference
- Semantic-segmentation output
- Pose-estimation output
- Completed observation tables
- Answers to all required questions
- A brief comparison of the four inference tasks

---

## Important Notes

> [!WARNING]
> Do not upgrade JetPack, CUDA, TensorRT, or the operating system during the laboratory.

> [!NOTE]
> The first execution of a model may take longer because TensorRT may optimize and cache the model. Later executions should start more quickly.

> [!TIP]
> If a group cannot complete the installation within the time allocated by the instructor, use the instructor-provided backup SD card and continue with the inference activities.
